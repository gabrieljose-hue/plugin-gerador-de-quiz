# Skill: integracao-sheets

Gera o codigo de rastreamento para enviar as respostas do quiz em tempo real para o Google Sheets via Google Apps Script.

---

## REGRA DE PERGUNTAS (OBRIGATORIO)

**Sempre usar a ferramenta `AskUserQuestion`** para qualquer pergunta, escolha ou coleta de input do usuario. Em todos os casos, sem excecao. Nunca fazer perguntas apenas no texto do chat.

---

## ETAPA 1: Perguntar se deseja ativar a integracao

Usar `AskUserQuestion` com 1 pergunta:
- Header: "Integracao com Google Sheets"
- Pergunta: "Deseja rastrear as respostas dos participantes em tempo real no Google Sheets?"
- Opcoes:
  - "Sim, quero rastrear as respostas" (description: "Gero o codigo do Apps Script e o snippet JS para embedar no quiz")
  - "Nao, seguir sem rastreamento" (description: "Pular esta etapa")

**Se "Nao"**: Encerrar a skill imediatamente e retornar ao fluxo.

---

## ETAPA 2: Gerar o codigo do Apps Script

### ETAPA 2A: Gerar `output/apps-script.gs`

Criar o arquivo com o codigo completo do Google Apps Script:

```gs
// ============================================================
// Google Apps Script — Rastreamento de Respostas do Quiz
// ============================================================
// Como usar:
//   1. Abra sua planilha no Google Sheets
//   2. Extensions > Apps Script
//   3. Cole este codigo substituindo o conteudo existente
//   4. Salve (Ctrl+S)
//   5. Clique em "Deploy" > "New deployment"
//   6. Tipo: Web App | Execute as: Me | Who has access: Anyone
//   7. Autorize e copie a URL gerada — volte ao Claude e cole a URL
// ============================================================

const ABA_RESPOSTAS = 'Respostas';

// Mapeamento de ID de tela → numero da coluna na planilha
// Telas 2A e 2B compartilham a coluna tela_2 (so uma delas e respondida por sessao)
// Tela 11B e a segunda tela de depoimentos (opcional)
const TELA_COL = {
  '1':4, '2':5, '2a':5, '2A':5, '2b':5, '2B':5,
  '3':6, '4':7, '5':8, '6':9, '7':10, '8':11,
  '9':12, '10':13, '11':14, '11b':15, '11B':15, '12':16,
  '13':17, '14':18, '15':19, '16':20, '17':21, '18':22
};

// Colunas fixas
const COL_SESSION_ID    = 1;  // A
const COL_CRIADO_EM     = 2;  // B
const COL_ATUALIZADO_EM = 3;  // C
// D(4)–V(22): tela_1 a tela_18 (via TELA_COL acima)
const COL_TELA_ATUAL    = 23; // W
const COL_CHECKOUT      = 24; // X — preenchido quando o lead clica no botao de compra
const COL_INICIOU_EM    = 25; // Y — preenchido quando o quiz e iniciado (primeira carga)

function doPost(e) {
  try {
    const sheet = obterOuCriarAba_();
    // Aceita tanto application/json quanto text/plain (sendBeacon envia como text/plain)
    const raw   = e.postData.contents;
    const data  = JSON.parse(raw);
    const { sessionId, tela, resposta, respostas, evento, timestamp } = data;

    const ts      = timestamp || new Date().toISOString();
    const rows    = sheet.getDataRange().getValues();
    let rowIndex  = -1;

    // Busca sessionId na coluna A (ignora cabecalho na linha 0)
    for (let i = 1; i < rows.length; i++) {
      if (rows[i][0] === sessionId) {
        rowIndex = i + 1; // indice 1-based do Sheets
        break;
      }
    }

    if (rowIndex === -1) {
      // Nova linha — inicializar com todas as colunas vazias
      const novaLinha = Array(COL_INICIOU_EM).fill('');
      novaLinha[COL_SESSION_ID    - 1] = sessionId;
      novaLinha[COL_CRIADO_EM     - 1] = ts;
      novaLinha[COL_ATUALIZADO_EM - 1] = ts;

      // Sempre registrar tela_atual quando tela esta presente
      if (tela) {
        novaLinha[COL_TELA_ATUAL - 1] = tela;
      }

      // Gravar resposta/evento na coluna da tela (se mapeada)
      if (tela && TELA_COL[String(tela)]) {
        const col = TELA_COL[String(tela)];
        novaLinha[col - 1] = formatarResposta_(resposta, respostas, evento);
      }

      if (evento === 'clicou_checkout') {
        novaLinha[COL_CHECKOUT - 1] = ts;
        novaLinha[COL_TELA_ATUAL - 1] = 'checkout';
      }

      if (evento === 'iniciou_quiz') {
        novaLinha[COL_INICIOU_EM - 1] = ts;
        novaLinha[COL_TELA_ATUAL - 1] = '1';
      }

      sheet.appendRow(novaLinha);

    } else {
      // Atualizar linha existente (upsert por sessionId)
      sheet.getRange(rowIndex, COL_ATUALIZADO_EM).setValue(ts);

      // Sempre atualizar tela_atual quando tela esta presente
      if (tela) {
        sheet.getRange(rowIndex, COL_TELA_ATUAL).setValue(tela);
      }

      // Gravar resposta/evento na coluna da tela (se mapeada)
      if (tela && TELA_COL[String(tela)]) {
        const col = TELA_COL[String(tela)];
        sheet.getRange(rowIndex, col).setValue(formatarResposta_(resposta, respostas, evento));
      }

      if (evento === 'clicou_checkout') {
        sheet.getRange(rowIndex, COL_CHECKOUT).setValue(ts);
        sheet.getRange(rowIndex, COL_TELA_ATUAL).setValue('checkout');
      }
      // iniciou_quiz em linha existente = lead recarregou a pagina — nao sobrescreve iniciou_em
    }

    return ContentService.createTextOutput('ok');

  } catch (err) {
    // Nao expor detalhes do erro publicamente
    return ContentService.createTextOutput('erro');
  }
}

// GET de healthcheck para confirmar que o deploy esta ativo
function doGet(e) {
  return ContentService.createTextOutput('quiz-tracker-ok');
}

// ---- Helpers ----

function formatarResposta_(resposta, respostas, evento) {
  if (evento === 'avancou') return 'avancou';
  if (evento === 'visualizou') return 'visualizou';
  if (respostas && respostas.length) return respostas.join(' | ');
  if (resposta)  return resposta;
  if (evento) return evento;
  return '';
}

function obterOuCriarAba_() {
  const ss    = SpreadsheetApp.getActiveSpreadsheet();
  let sheet   = ss.getSheetByName(ABA_RESPOSTAS);

  if (!sheet) {
    sheet = ss.insertSheet(ABA_RESPOSTAS);
    const cabecalhos = [
      'session_id', 'criado_em', 'atualizado_em',
      'tela_1',  'tela_2',  'tela_3',  'tela_4',  'tela_5',
      'tela_6',  'tela_7',  'tela_8',  'tela_9',  'tela_10',
      'tela_11', 'tela_11B','tela_12', 'tela_13', 'tela_14',
      'tela_15', 'tela_16', 'tela_17', 'tela_18',
      'tela_atual', 'clicou_checkout', 'iniciou_em'
    ];
    sheet.appendRow(cabecalhos);
    sheet.setFrozenRows(1);
    sheet.getRange(1, 1, 1, cabecalhos.length)
      .setFontWeight('bold')
      .setBackground('#4285F4')
      .setFontColor('#FFFFFF');
  }

  return sheet;
}
```

---

## ETAPA 2.5: Pedir a URL do Apps Script

Apos salvar o `apps-script.gs`, exibir no chat o codigo completo diretamente como card copiavel, seguido das instrucoes de deploy. O usuario NAO deve ser direcionado a abrir nenhum arquivo — tudo deve estar visivel no chat.

Exibir no chat:

```
Codigo do Google Apps Script gerado! Copie o bloco abaixo e cole no editor do Apps Script:
```

Em seguida, exibir o codigo completo do `apps-script.gs` dentro de um bloco de codigo ```gs ... ``` no chat.

Depois do bloco de codigo, exibir as instrucoes de deploy:

```
Como fazer o deploy:
  1. Abra sua planilha no Google Sheets → Extensions > Apps Script
  2. Apague o conteudo existente e cole o codigo acima
  3. Salve (Ctrl+S)
  4. Clique em Deploy > New deployment
  5. Tipo: Web App | Execute as: Me | Who has access: Anyone
  6. Autorize e copie a URL gerada (formato: https://script.google.com/macros/s/XXXXX/exec)

Cole a URL aqui para eu incluir ela no codigo do quiz automaticamente.
```

Em seguida, usar `AskUserQuestion` com 1 pergunta:
- Header: "URL do Apps Script"
- Pergunta: "Cole aqui a URL gerada pelo Google Apps Script apos o deploy:"
- Tipo: other (campo de texto livre)

Armazenar a URL respondida como `[APPS_SCRIPT_URL]` para usar na ETAPA 3A.

---

## ETAPA 3: Gerar os arquivos finais

Executar as etapas 3A e 3B em sequencia.

### ETAPA 3A: Gerar `output/sheets-tracking.js`

Criar o arquivo substituindo `[APPS_SCRIPT_URL]` pela URL real informada pelo usuario:

```js
// ============================================================
// sheets-tracking.js — Rastreamento do Quiz no Google Sheets
// ============================================================
// Gerado automaticamente pelo plugin Gerador de Quiz RTG
// Apps Script URL ja configurada — pronto para embedar no quiz.html
// ============================================================

(function () {
  'use strict';

  // ---- Configuracao ----
  const APPS_SCRIPT_URL = '[APPS_SCRIPT_URL]';

  // ---- Session ID ----
  function gerarSessionId() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
      var r = Math.random() * 16 | 0;
      return (c === 'x' ? r : (r & 0x3 | 0x8)).toString(16);
    });
  }

  var sessionId;
  var isNovaVisita = true;
  try {
    sessionId = localStorage.getItem('quiz_session_id');
    if (sessionId) {
      isNovaVisita = false;
    } else {
      sessionId = gerarSessionId();
      localStorage.setItem('quiz_session_id', sessionId);
    }
  } catch (e) {
    // localStorage indisponivel (iframe, privacidade) — gerar sessionId em memoria
    sessionId = gerarSessionId();
  }

  // ---- Envio para Sheets ----
  // Fire-and-forget: nao bloqueia a navegacao entre telas
  window.enviarParaSheets = function (payload) {
    if (!APPS_SCRIPT_URL) return;
    try {
      var dados = Object.assign({ sessionId: sessionId, timestamp: new Date().toISOString() }, payload);
      // navigator.sendBeacon e mais confiavel para Apps Script (sem redirect issues)
      // Fallback para fetch com text/plain (unico Content-Type simples permitido em no-cors)
      var blob = new Blob([JSON.stringify(dados)], { type: 'text/plain' });
      if (navigator.sendBeacon) {
        navigator.sendBeacon(APPS_SCRIPT_URL, blob);
      } else {
        fetch(APPS_SCRIPT_URL, {
          method: 'POST',
          mode: 'no-cors',
          body: blob
        });
      }
    } catch (e) {
      // Silencia erros para nao interromper o quiz
    }
  };

  // ---- Registro de inicio ----
  // Cria a linha na planilha imediatamente quando o quiz carrega pela primeira vez.
  // Em visitas subsequentes (reload, volta pela aba), a linha ja existe — o servidor
  // ignora o evento iniciou_quiz para nao sobrescrever o timestamp original.
  if (isNovaVisita) {
    window.addEventListener('load', function () {
      window.enviarParaSheets({ evento: 'iniciou_quiz' });
    });
  }

  // ---- Rastreamento automatico ----
  // O html-builder gera o quiz.html com tracking em TODOS os pontos:
  //
  // 1. irPara() — TODA navegacao entre telas registra {tela: ID, evento: 'visualizou'}
  //    Isso garante que cada tela alcancada pelo lead aparece na planilha.
  //
  // 2. Escolha unica — selecionarUnica() sobrescreve 'visualizou' com a resposta real:
  //    {tela: '4', resposta: 'Todos os dias'}
  //
  // 3. Multipla escolha — irParaMultipla() envia as respostas selecionadas:
  //    {tela: '9', respostas: ['Nao consigo focar', 'Perco tempo relendo']}
  //
  // 4. Apresentacao — botao "Continuar" sobrescreve 'visualizou' com 'avancou':
  //    {tela: '3', evento: 'avancou'}
  //
  // 5. Genero (tela 1) — registra o genero escolhido:
  //    {tela: '1', resposta: 'Masculino'}
  //
  // 6. Carregamento (tela 16) — registra ao completar:
  //    {tela: '16', evento: 'avancou'}
  //
  // 7. Checkout (tela 18) — registra clique no botao de compra:
  //    {evento: 'clicou_checkout'}
  //
  // Resultado: a coluna tela_atual sempre mostra a ultima tela alcancada,
  // e cada coluna tela_N mostra a resposta ou 'avancou'/'visualizou'.

})();
```

---

### ETAPA 3B: Gerar `output/instrucoes-sheets.md`

Criar o arquivo com o passo a passo de configuracao:

```md
# Integracao Google Sheets — Passo a Passo

## Pre-requisitos

- Conta Google (Gmail)
- Planilha Google Sheets criada (pode estar vazia)

---

## Passo 1: Abrir o Google Apps Script

1. Abra sua planilha no Google Sheets
2. No menu superior: **Extensions** (Extensoes) > **Apps Script**
3. Uma nova aba abre com o editor de codigo

---

## Passo 2: Colar o codigo

1. Apague todo o conteudo existente no editor (Ctrl+A, Delete)
2. Abra o arquivo `output/apps-script.gs` gerado pelo plugin
3. Copie o conteudo completo e cole no editor
4. Salve: **Ctrl+S** (ou icone de disquete)

---

## Passo 3: Fazer o Deploy como Web App

1. Clique em **Deploy** > **New deployment**
2. Clique no icone de engrenagem ao lado de "Select type" e escolha **Web App**
3. Configure:
   - **Description**: Quiz Tracker
   - **Execute as**: Me (sua conta Google)
   - **Who has access**: Anyone
4. Clique em **Deploy**
5. Autorize as permissoes solicitadas (clique em "Authorize access" > selecione sua conta > "Allow")
6. Copie a **Web App URL** exibida na tela de confirmacao

> A URL tem o formato: `https://script.google.com/macros/s/XXXXXX/exec`

---

## Passo 4: Embedar no HTML do quiz

O arquivo `output/sheets-tracking.js` ja esta configurado com a URL do seu Apps Script.

Adicione antes do `</body>` no seu `quiz.html`:

```html
<script src="sheets-tracking.js"></script>
```

Ou inline: copie o conteudo do `sheets-tracking.js` diretamente no HTML entre tags `<script>`.

---

## Verificacao

1. Abra o quiz no navegador
2. Avance 3 telas clicando em "Continuar"
3. Abra a planilha — deve aparecer a aba **Respostas** com 1 linha nova
4. Avance mais telas — a mesma linha deve ser atualizada (nao duplicada)
5. Abra em aba anonima — deve aparecer uma segunda linha (novo sessionId)

---

## Estrutura da planilha (aba "Respostas")

| Coluna | Campo | Descricao |
|--------|-------|-----------|
| A | session_id | ID unico do participante (UUID v4) |
| B | criado_em | Timestamp da primeira interacao |
| C | atualizado_em | Timestamp da ultima interacao |
| D | tela_1 | Abertura (genero selecionado ou avancou) |
| E | tela_2 | Dados demograficos — tela 2, 2A ou 2B (faixa etaria) |
| F–M | tela_3 a tela_10 | Respostas das telas 3 a 10 |
| N | tela_11 | Primeiro bloco de depoimentos (avancou) |
| O | tela_11B | Segundo bloco de depoimentos, se existir (avancou) |
| P–V | tela_12 a tela_18 | Respostas das telas 12 a 18 |
| W | tela_atual | Ultima tela atingida pelo participante |
| X | clicou_checkout | Timestamp do clique no botao de compra (vazio = nao clicou) |
| Y | iniciou_em | Timestamp de quando o quiz foi aberto pela primeira vez |

---

## Solucao de problemas

**Nao aparece nenhuma linha na planilha**
- Verifique se a URL no `sheets-tracking.js` foi substituida corretamente
- Confirme que o deploy foi feito com "Who has access: Anyone"
- Abra o console do navegador (F12) e verifique se ha erros de rede

**A aba "Respostas" nao foi criada**
- O Apps Script cria a aba automaticamente na primeira requisicao
- Se a planilha estava vazia e nenhuma requisicao chegou, a aba nao existe ainda

**Linhas duplicadas por participante**
- Verifique se o `localStorage` do navegador nao foi limpo entre sessoes
- Em modo anonimo cada abertura gera um novo sessionId (comportamento esperado)
```

---

## ETAPA 4: Retornar resumo ao chat e continuar

Apos gerar os 3 arquivos, usar a ferramenta `Read` em `output/sheets-tracking.js` e `output/instrucoes-sheets.md` para exibi-los como cards clicaveis. NAO mencionar caminhos de arquivo como texto.

Em seguida, exibir no chat com linguagem simples e direta, sem jargoes tecnicos:

```
Rastreamento configurado!

O Agente Programador vai incluir o codigo de rastreamento no quiz automaticamente.
Assim que um participante comecar o quiz, uma linha aparece na sua planilha.
Conforme ele avanca nas telas, a mesma linha e atualizada em tempo real.

Para verificar depois que o quiz estiver no ar:
  1. Abra o quiz no navegador e clique em algumas telas
  2. Abra a planilha — aparecera a aba "Respostas" com 1 linha por participante
  3. Cada participante tem sua propria linha, sem duplicatas
```

**Seguir imediatamente para o proximo passo do fluxo** — NAO pausar, NAO aguardar input do usuario.
