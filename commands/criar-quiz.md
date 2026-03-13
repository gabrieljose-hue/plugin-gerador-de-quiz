---
description: Gera um quiz de vendas completo com 18 telas estruturadas para converter leads em compradores
---

# Comando /criar-quiz

Orquestra o fluxo completo: extrai dados do copy → revisa e ajusta → gera as 18 telas → revisa qualidade.

## REGRA DE FLUXO CONTINUO

Cada passo deve fluir automaticamente para o proximo sem anunciar, pedir confirmacao ou esperar o usuario dizer "continue". So pausar quando houver uma decisao real do usuario (AskUserQuestion). Nunca emitir mensagens como "Pronto para...", "Agora vamos...", "Dados salvos!", "Otimo!".

## REGRA DE EXIBICAO DE ARQUIVOS (OBRIGATORIO — SEM EXCECAO)

**Nunca mencionar caminhos de arquivo como texto** (ex: "output/quiz.md", "Abra o arquivo X"). Isso forca o usuario a abrir pastas manualmente.

**Sempre usar a ferramenta `Read`** para ler cada arquivo de output logo apos salva-lo. Quando o `Read` e usado, o Claude Code renderiza um card clicavel do arquivo diretamente no chat — o usuario clica e ve o conteudo sem sair da conversa.

**Nunca instruir o usuario a interagir com os cards**. Os cards gerados pelo `Read` ja sao autoexplicativos. Frases como "Clique nos arquivos acima", "Veja o preview no card", "Abra o arquivo X" sao **proibidas**. O card aparece, o usuario entende sozinho.

Esta regra se aplica a este comando e a todas as skills chamadas por ele, sem excecao.

## REGRA DE PERGUNTAS (OBRIGATORIO — SEM EXCECAO)

**Sempre usar a ferramenta `AskUserQuestion`** para qualquer pergunta, escolha ou coleta de input do usuario. Isso vale para TODAS as etapas deste comando e de todas as skills chamadas por ele. Em todos os casos, sem nenhuma excecao. Nunca fazer perguntas apenas no texto do chat. Nenhuma skill ou passo pode ignorar esta regra.

---

## PASSO 0: Verificar se o usuario tem copy

**Objetivo**: Descobrir se o usuario ja tem copy ou briefing do produto, ou se precisa responder perguntas para gerar os dados do zero.

**Usar `AskUserQuestion`** com 1 pergunta:
- Header: "Voce tem copy ou briefing do produto?"
- Pergunta: "Para comecar, me diga: voce ja tem um copy ou briefing do produto para eu extrair os dados automaticamente?"
- Tipo: single select
- Opcoes:
  - "Sim, tenho copy ou briefing" (description: "Vou colar meu texto e voce extrai os dados automaticamente")
  - "Nao, vou responder perguntas" (description: "Voce me guia com perguntas e monta os dados do zero")

---

### PASSO 0 — Caminho A: Usuario TEM copy

**Se escolheu "Sim, tenho copy ou briefing":**

Usar `AskUserQuestion` com 1 pergunta:
- Header: "Cole seu copy ou briefing"
- Pergunta: "Cole aqui o seu copy ou briefing completo do produto (landing page, roteiro de vendas, descricao, briefing, etc.):"
- Tipo: other (campo de texto livre para colar o copy)

Chamar skill **extrator-dados** com o texto recebido.
Skill salva `output/dados-quiz.json` a partir do copy.
Ir para **PASSO 0.5**.

---

### PASSO 0 — Caminho B: Usuario NAO tem copy

**Se escolheu "Nao, vou responder perguntas":**

Coletar todos os dados inline via 5 rodadas de `AskUserQuestion`. Nao chamar nenhuma skill externa.

---

#### Rodada B-1: Produto e Oferta (7 perguntas)

Usar `AskUserQuestion` com 7 perguntas:

1. **Tema do Quiz**
   - Header: "Tema do Quiz"
   - Pergunta: "Qual e o tema do quiz? (Ex: Leitura rapida, Emagrecimento, Vendas online, Ansiedade)"
   - Tipo: other

2. **Produto**
   - Header: "Produto"
   - Pergunta: "Qual e o nome exato do produto que sera vendido no quiz?"
   - Tipo: other
   - Sugestoes (exemplos de nomes de produto — nao de tipos): "Metodo MIL de Leitura", "Programa Corpo em Forma 30 Dias", "Curso Vendas do Zero ao Pro", "Imersao Mente de Alta Performance"

3. **Preco**
   - Header: "Preco"
   - Pergunta: "Qual e o preco do produto? (Ex: R$ 97, R$ 297, 12x de R$ 19,90)"
   - Tipo: other

4. **Link Checkout**
   - Header: "Link Checkout"
   - Pergunta: "Qual e o link do checkout (pagina de compra)? Pode deixar em branco se ainda nao tem."
   - Tipo: other

5. **Transformacao Principal**
   - Header: "Transformacao Principal"
   - Pergunta: "Qual e a principal transformacao que o produto promete? (Ex: Ler 3x mais rapido em 30 dias)"
   - Tipo: other

6. **Logo do Produto**
   - Header: "Logo do Produto"
   - Pergunta: "O produto tem logo? Se sim, cole a URL da imagem da logo. Se nao, deixe em branco."
   - Tipo: other

7. **Imagens do Quiz**
   - Header: "Imagens do Quiz"
   - Pergunta: "Deseja adicionar imagens para ilustrar as telas do quiz? (ate 5 imagens)"
   - Tipo: single-select
   - Opcoes:
     - "Sim, vou enviar as URLs" (description: "Posso enviar ate 5 URLs de imagens para distribuir nas telas")
     - "Nao, seguir sem imagens" (description: "O quiz sera gerado sem imagens ilustrativas")

**Se a resposta da pergunta 7 for "Sim, vou enviar as URLs"** → Usar `AskUserQuestion` com 1 pergunta adicional (Rodada B-1b):

- Header: "URLs das Imagens"
- Pergunta: "Cole as URLs das imagens (ate 5, uma por linha). A primeira imagem sera usada na tela inicial do quiz. As demais serao distribuidas nas outras telas sem repetir."
- Tipo: other

**Se a resposta for "Nao, seguir sem imagens"**: Registrar `imagens_quiz = []`.

---

#### Rodada B-2: Publico-Alvo (2 perguntas)

Usar `AskUserQuestion` com 2 perguntas:

6. **Publico-Alvo**
   - Header: "Publico-Alvo"
   - Pergunta: "Quem e o publico-alvo do quiz? Descreva o perfil: idade, situacao atual, nivel de conhecimento, principal dor."
   - Tipo: other

7. **Segmentar por Genero**
   - Header: "Segmentacao por Genero"
   - Pergunta: "O quiz vai segmentar por genero? Se sim, a tela de abertura pergunta o genero e as telas seguintes terao versoes masculina e feminina."
   - Tipo: single-select
   - Opcoes:
     - "Sim, segmentar por genero" (description: "A abertura pergunta o genero e personaliza as proximas telas")
     - "Nao, quiz unico para todos" (description: "Sem segmentacao, perguntas iguais para todos")

---

#### Rodada B-3: Dores e Desejos (3 perguntas)

Usar `AskUserQuestion` com 3 perguntas:

8. **Dor Principal**
   - Header: "Dor Principal"
   - Pergunta: "Qual e a dor/problema numero 1 do seu publico? O que o incomoda mais?"
   - Tipo: other

9. **Consequencias Negativas**
    - Header: "Consequencias Negativas"
    - Pergunta: "Quais sao as consequencias negativas mais comuns desse problema? Liste de 3 a 6 consequencias separadas por virgula."
    - Tipo: other

10. **Desejos Principais**
    - Header: "Desejos Principais"
    - Pergunta: "Quais sao os principais objetivos/desejos do seu publico? Liste de 3 a 5 desejos separados por virgula."
    - Tipo: other

---

#### Rodada B-4: Prova Social (2 perguntas + 1 condicional)

Usar `AskUserQuestion` com 2 perguntas:

11. **Numeros Sociais**
    - Header: "Numeros Sociais"
    - Pergunta: "Quantos alunos, clientes ou usuarios o produto ja tem? (Ex: +1.000 alunos, +500 clientes)"
    - Tipo: other

12. **Depoimentos**
    - Header: "Depoimentos"
    - Pergunta: "Voce tem depoimentos de clientes ou alunos disponiveis agora?"
    - Tipo: single-select
    - Opcoes:
      - "Sim, 3 ou mais" (description: "Tenho varios resultados de clientes/alunos para mostrar")
      - "Sim, 1 ou 2" (description: "Tenho alguns resultados iniciais")
      - "Nenhum" (description: "Produto novo ou ainda sem depoimentos coletados")

**Se a resposta for "Sim, 3 ou mais" ou "Sim, 1 ou 2"** → Usar `AskUserQuestion` com 1 pergunta adicional (Rodada B-4b):

- Header: "Imagens dos Depoimentos"
- Pergunta: "Cole as URLs das imagens dos seus depoimentos (prints de WhatsApp, Instagram, Google, etc.). Uma URL por linha."
- Tipo: other

**EXCECAO — Colar URLs diretamente no chat**: Se o usuario responder algo como "vou colar abaixo", "cola aqui", "vou digitar" ou qualquer variacao indicando que vai colar o conteudo diretamente no chat, NAO usar `AskUserQuestion`. Em vez disso, responder apenas com uma frase curta no chat pedindo que cole as URLs (ex: "Pode colar as URLs aqui:") e aguardar a resposta do usuario. Apos receber as URLs no chat, voltar a usar `AskUserQuestion` normalmente para tudo.

**Se a resposta for "Nenhum"**: Registrar `depoimentos_quantidade = "0"` e `depoimentos = []`.

---

#### Rodada B-5: Detalhes do Produto (3 perguntas)

Usar `AskUserQuestion` com 3 perguntas:

13. **Objetivos Especificos do Produto**
    - Header: "Objetivos Especificos do Produto"
    - Pergunta: "Quais sao os 4 resultados especificos que o seu produto entrega? Esses serao as opcoes da tela de personalizacao do quiz. Separe por virgula, quebra de linha ou |."
    - Tipo: other
    - Sugestoes: Antes de chamar `AskUserQuestion`, gerar 4 sugestoes de objetivos especificos baseadas no tema, transformacao principal e desejos ja coletados nas rodadas anteriores. Usar essas sugestoes como exemplos no campo de exemplo da pergunta, no formato: "(Ex: [sugestao 1], [sugestao 2], [sugestao 3], [sugestao 4])". Nunca usar exemplos fixos de leitura rapida ou de outros nichos.

14. **Ampliar Oportunidade**
    - Header: "Ampliar Oportunidade"
    - Pergunta: "Como o seu produto facilita o resultado — qual barreira ele elimina? Escreva uma frase curta. (Ex: 15 minutos por dia / sem experiencia anterior / sem equipamentos caros)"
    - Tipo: other

15. **Bonus e Entregaveis**
    - Header: "O Que Sera Entregue"
    - Pergunta: "Liste o que o lead recebe ao comprar: modulos, bonus, materiais extras e valores de cada item. Separe por quebra de linha ou |. Inclua o valor de cada item se quiser. (Ex: Modulo 1 — Tecnicas de Leitura Rapida (R$ 97) | Bonus — Planner de Leitura (R$ 47) | Bonus — Grupo VIP no WhatsApp (R$ 97))"
    - Tipo: other

---

#### Montar e salvar o JSON (apos Rodada B-5)

Com todas as respostas coletadas, aplicar as seguintes conversoes de tipo:

- `dores.consequencias`: `split(",").map(s => s.trim())`
- `desejos`: `split(",").map(s => s.trim())`
- `produto.objetivos_especificos`: `split(/[|\n,]/).map(s => s.trim()).filter(Boolean)`
- `produto.entregaveis`: preservar como string exatamente como digitado pelo usuario; se vazio → `"[INSERIR ENTREGAVEIS]"`
- `publico.segmentar_genero`: "Sim, segmentar por genero" → `true`; "Nao, quiz unico para todos" → `false`
- `depoimentos` (URLs da Rodada B-4b): `split("\n").map(url => ({ imagem_url: url.trim() }))`
- `depoimentos_quantidade`: "Sim, 3 ou mais" → `"3+"`; "Sim, 1 ou 2" → `"1-2"`; "Nenhum" → `"0"`
- `imagens_quiz` (URLs da Rodada B-1b): `split("\n").map(url => url.trim()).filter(Boolean).slice(0, 5)` — maximo 5 URLs; se "Nao, seguir sem imagens" → `[]`

**Aplicar inferencias automaticas** (completar dados que o usuario pode ter informado de forma insuficiente):

- Se `dores.consequencias` tiver menos de 5 itens: inferir os faltantes a partir de `dores.principal` e `produto.tema`. Consequencias devem ser especificas ao tema, em primeira pessoa ("Nao consigo...", "Perco tempo..."), e verossimeis para o publico
- Se `desejos` tiver menos de 4 itens: inferir a partir de `transformacao_principal` e do tema
- Se `produto.objetivos_especificos` tiver menos de 4 itens: completar com resultados especificos e acionaveis que o produto entrega
- Se `produto.amplificar_oportunidade` estiver vazio: inferir pelo contexto do tema (ex: metodo → "sem precisar de muito tempo por dia", produto online → "de qualquer lugar, no seu ritmo")
- Se `publico.numeros_sociais` estiver vazio: usar forma generica "centenas de alunos" (nunca inventar numeros)

Salvar em `output/dados-quiz.json`:

```json
{
  "origem": "coleta",
  "produto": {
    "tema": "string",
    "nome": "string",
    "preco": "string",
    "link_checkout": "string",
    "transformacao_principal": "string",
    "logo_url": "string (URL da logo ou vazio)",
    "objetivos_especificos": ["string", "string", "string", "string"],
    "amplificar_oportunidade": "string",
    "entregaveis": "string"
  },
  "publico": {
    "descricao": "string",
    "segmentar_genero": true,
    "numeros_sociais": "string"
  },
  "dores": {
    "principal": "string",
    "consequencias": ["string", "string", "string", "string", "string"]
  },
  "desejos": ["string", "string", "string", "string"],
  "depoimentos_quantidade": "string",
  "depoimentos": [{ "imagem_url": "string" }],
  "imagens_quiz": ["string (URLs das imagens — max 5)"],
  "campos_ausentes": []
}
```

Ir direto para **PASSO 1** (os dados ja estao completos — pular PASSO 0.5).

---

## PASSO 0.5: Revisar e Validar os Dados (somente Caminho A)

**Objetivo**: Garantir que os dados extraidos do copy estao completos antes de gerar o quiz.

**IMPORTANTE**: Este passo so e executado quando o usuario veio pelo **Caminho A** (colou copy/briefing). No **Caminho B** (respondeu perguntas), os dados ja foram completados com inferencias automaticas na etapa anterior — ir direto para o PASSO 1.

Chamar skill **revisor-dados** para validar e ajustar `output/dados-quiz.json`.

**O revisor**:
- Verifica se todos os campos obrigatorios estao presentes
- Ajusta proativamente o que puder (sem perguntar): completa consequencias, desejos, objetivos especificos, infere amplificar_oportunidade
- Perguntas ao usuario APENAS para campos criticos ausentes que nao podem ser inferidos (maximo 3 perguntas, 1 rodada)
- Aprova o JSON quando todos os campos obrigatorios estiverem OK

**Saida**: `output/dados-quiz.json` validado e completo

---

## PASSO 0.6: Coletar Imagens do Quiz (ambos os caminhos)

**IMPORTANTE**: Este passo so e executado quando o usuario veio pelo **Caminho A** (colou copy/briefing). No **Caminho B** (respondeu perguntas), as imagens ja foram coletadas na Rodada B-1 — ir direto para o PASSO 1.

Usar `AskUserQuestion` com 1 pergunta:
- Header: "Imagens do Quiz"
- Pergunta: "Deseja adicionar imagens para ilustrar as telas do quiz? (ate 5 imagens)"
- Tipo: single-select
- Opcoes:
  - "Sim, vou enviar as URLs" (description: "Posso enviar ate 5 URLs de imagens para distribuir nas telas")
  - "Nao, seguir sem imagens" (description: "O quiz sera gerado sem imagens ilustrativas")

**Se "Sim"**: Usar `AskUserQuestion` com 1 pergunta:
- Header: "URLs das Imagens"
- Pergunta: "Cole as URLs das imagens (ate 5, uma por linha). A primeira imagem sera usada na tela inicial do quiz. As demais serao distribuidas nas outras telas sem repetir."
- Tipo: other

Parsear as URLs: `split("\n").map(url => url.trim()).filter(Boolean).slice(0, 5)`

Atualizar `output/dados-quiz.json` adicionando o campo `imagens_quiz` com o array de URLs.

**Se "Nao"**: Adicionar `"imagens_quiz": []` ao `output/dados-quiz.json`.

---

## PASSO 1: Gerar o Quiz Completo

Chamar skill **criador-quiz** para gerar todas as 18 telas.

**Entrada**: `output/dados-quiz.json`

**18 telas geradas em ordem**:
```
Tela 1:  Abertura (apresentacao — escolha de genero)
Tela 2A: Dados Demograficos — Masculino (escolha-unica — idade com fotos masculinas)
Tela 2B: Dados Demograficos — Feminino (escolha-unica — idade com fotos femininas)
Tela 3:  Prova Social (apresentacao — numeros de alunos)
Tela 4:  Diagnostico 1 — Pergunta comportamental (escolha-unica)
Tela 6:  Diagnostico 2 — Segunda dimensao do problema (escolha-unica)
Tela 7:  Diagnostico 3 — Terceira dimensao do problema (escolha-unica)
Tela 8:  Tocar na Ferida 1 — "Sente-se culpado?" (escolha-unica — SO opcoes afirmativas)
Tela 9:  Tocar na Ferida 2 — Consequencias negativas (multipla-escolha)
Tela 10: Desejo Principal — Objetivo do lead (multipla-escolha)
Tela 11: Depoimentos — Prova de solucao (apresentacao)
Tela 12: Nivel de Conhecimento — "O que voce sabe sobre [tema]?" (escolha-unica)
Tela 13: Objetivo Especifico — 1 resultado do produto (escolha-unica — resposta usada na Tela 14)
Tela 14: Promessa Personalizada — "Nos iremos te ajudar!" com {{resposta_tela_13}} (apresentacao)
Tela 15: Micro-Compromisso — Motivacao (escolha-unica — so opcoes positivas)
Tela 16: Carregamento — Analise de perfil (carregamento)
Tela 17: Resultado — Pronto para resultados + grafico (apresentacao)
Tela 18: Pagina de Vendas Interna — Oferta completa (apresentacao longa)
```

**Saidas**:
- `output/quiz.json` — estrutura JSON completa
- `output/quiz.md` — documento legivel para implementacao no Inlead

---

## PASSO 1.1: Revisao do Conteudo das Telas

**Objetivo**: Exibir o conteudo gerado para que o usuario possa revisar e solicitar ajustes antes de seguir para imagens e design.

Ler `output/quiz.md` e emitir o conteudo completo no chat — todas as telas, sem omitir nada.

Em seguida, usar `AskUserQuestion` com 1 pergunta:
- Header: "Revisao do Quiz"
- Pergunta: "O conteudo das telas esta do seu agrado? Voce pode aprovar ou descrever as alteracoes que deseja."
- Tipo: single-select
- Opcoes:
  - "Aprovado, continuar" (description: "O conteudo esta bom, seguir para o design")
  - "Tenho alteracoes" (description: "Vou descrever o que quero mudar")

**Se "Aprovado"**: Seguir para PASSO 1.5.

**Se "Tenho alteracoes"**: Usar `AskUserQuestion` com 1 pergunta (tipo: other) pedindo a descricao detalhada das alteracoes. Aplicar as alteracoes diretamente nas telas indicadas, atualizar `output/quiz.md` e `output/quiz.json`, emitir novamente o conteudo completo no chat e perguntar aprovacao novamente. Repetir ate o usuario aprovar.

---

## PASSO 1.5: Designer de Quiz

Chamar skill **designer-quiz** para criar a identidade visual e sugerir imagens para cada tela.

**Entrada**: `output/dados-quiz.json` + `output/quiz.json`

**O designer faz**:
- Pergunta 1: Cor principal do produto/marca (ou define automaticamente pelo tema)
- Estilo visual auto-determinado pelo nicho (Moderno e Limpo / Bold e Impactante / Suave e Acolhedor / Profissional e Serio / Energico e Jovem)
- Gera paleta de cores completa (primaria, secundaria, acento, fundo, texto)
- Escolhe tipografia do Google Fonts adequada ao estilo
- Define estilo e cor dos botoes
- Para cada tela com imagem: sugere termo de busca + link direto no Unsplash e Pexels

**Saidas**:
- `output/design-tokens.json` — paleta, tipografia, botoes
- `output/design-preview.html` — preview visual (aprovado pelo usuario dentro da skill)

A skill designer-quiz ja inclui auto-validacao interna (9 checkpoints) antes de apresentar ao usuario. Nao e necessario revisor externo.

---

## PASSO 1.8: Integracao com Google Sheets (opcional)

Chamar skill **integracao-sheets** para gerar o codigo de rastreamento.

A skill pergunta ao usuario se deseja ativar a integracao. Se sim, gera os 3 arquivos de output e exibe as instrucoes resumidas. Se nao, segue imediatamente para o PASSO 2.

**Entrada**: nenhuma (a skill e autocontida)

**Saidas** (somente se o usuario ativar):
- `output/apps-script.gs` — codigo completo do Google Apps Script
- `output/sheets-tracking.js` — snippet JS para embedar no quiz.html
- `output/instrucoes-sheets.md` — passo a passo de configuracao

---

## PASSO 2: Gerar o HTML do Quiz

Chamar skill **html-builder** para gerar o arquivo HTML funcional.

**Entrada**: `output/quiz.json` + `output/design-tokens.json`

**O html-builder faz**:
- Gera cada tela como uma `<div>` com CSS e JS puros
- Conecta todas as telas na sequencia correta
- Aplica o design system (cores, fontes, componentes)
- Injeta as URLs das imagens do Pexels onde disponiveis
- Implementa: navegacao, selecao unica/multipla, loading animado, variavel {{resposta_tela_13}}

**Saida**: `output/quiz.html`

---

## PASSO 3: Confirmacao

Exibir resultado final no chat:

```
Quiz gerado com sucesso!

Resumo:
  • Tema: [TEMA]
  • Produto: [PRODUTO]
  • Total de telas: [N] (varia conforme genero/depoimentos)
  • Resposta dinamica {{resposta_tela_13}}: Tela 14
  • Segmentacao por genero: [Sim/Nao — auto-detectado]
  • Pergunta de idade: [Sim/Nao — auto-detectado]
  • Telas de depoimentos: [0 / 1 / 2]
  • Cor principal: [HEX]
  • Estilo visual: [estilo escolhido]

Como implementar no Inlead:
  1. Abra o quiz.md — crie cada tela na ordem indicada
  2. Configure cores e fontes conforme o design-tokens.json
  3. Configure a logica condicional de genero (se houver): Tela 1 → 2A ou 2B
  4. Configure a variavel {{resposta_tela_13}} na Tela 14
  5. Teste do inicio ao fim antes de publicar
```

Em seguida, usar a ferramenta `Read` apenas em `output/quiz.html` para que o card apareça diretamente no chat.

Apos exibir o card do HTML, usar `AskUserQuestion` com 1 pergunta:
- Header: "Visualizar o Quiz"
- Pergunta: "Deseja abrir o quiz.html agora no Google Chrome para visualizar?"
- Tipo: single-select
- Opcoes:
  - "Sim, abrir no Chrome" (description: "Abre o arquivo quiz.html diretamente no navegador")
  - "Nao, ja terminei" (description: "Encerrar o processo")

**Se "Sim, abrir no Chrome"**: executar via Bash o comando `open -a "Google Chrome" "$(pwd)/output/quiz.html"` para abrir o arquivo no Google Chrome.

**Se "Nao, ja terminei"**: encerrar o fluxo sem executar nenhum comando adicional.

---

## Tratamento de Erros

### Se o usuario nao colar copy na Opcao A
Usar `AskUserQuestion` com 1 pergunta:
- Header: "Cole seu copy ou briefing"
- Pergunta: "Para extrair os dados, preciso do texto do seu copy ou briefing. Cole aqui qualquer texto que descreva o produto: landing page, roteiro de vendas, descricao, briefing, etc."
- Tipo: other (campo de texto livre)

### Se revisor-dados reprovar 2 vezes
```
Nao foi possivel validar os dados automaticamente. Vamos reiniciar a coleta.
```
Reiniciar pelo PASSO 0.

---

## Tabela de Arquivos

| Arquivo | Escrito por | Lido por |
|---------|-------------|----------|
| `output/dados-quiz.json` | Passo 0 + Passo 0.5 (se Caminho A) | Passos 1, 1.5 |
| `output/quiz.json` | Passo 1 (criador-quiz) | Passos 1.5, 2 |
| `output/quiz.md` | Passo 1 (criador-quiz) | Passo 1.1 (revisao) + Final (usuario) |
| `output/design-tokens.json` | Passo 1.5 (designer-quiz) | Passo 2 (html-builder) |
| `output/design-preview.html` | Passo 1.5 (designer-quiz) | Usuario (aprovacao visual) |
| `output/apps-script.gs` | Passo 1.8 (integracao-sheets, opcional) | Final (usuario) |
| `output/sheets-tracking.js` | Passo 1.8 (integracao-sheets, opcional) | Passo 2 (html-builder) |
| `output/instrucoes-sheets.md` | Passo 1.8 (integracao-sheets, opcional) | Final (usuario) |
| `output/quiz.html` | Passo 2 (html-builder) | Final (usuario) |

---

## Skills Utilizadas (5 no total)

- **extrator-dados**: Extrai dados de copy ou briefing colado pelo usuario (Passo 0 — Caminho A)
- **revisor-dados**: Valida e ajusta os dados extraidos do copy (Passo 0.5 — somente Caminho A)
- **criador-quiz**: Gera as telas do quiz com auto-validacao interna (Passo 1)
- **designer-quiz**: Cria identidade visual com auto-validacao interna (Passo 1.5)
- **integracao-sheets**: Gera codigo de rastreamento para Google Sheets via Apps Script (Passo 1.8, opcional)
- **html-builder**: Gera o quiz.html funcional e completo pronto para o navegador (Passo 2)
