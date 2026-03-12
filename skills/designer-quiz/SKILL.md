---
name: designer-quiz
description: Define o design system do quiz — paleta, tipografia, componentes — gera um preview HTML do kit visual e aguarda aprovacao do usuario antes de continuar.
---

# Skill: Designer de Quiz

## Missao

Pensar como um designer profissional especializado em quiz de conversao: analisar o tema, produto e publico para criar uma identidade visual coerente. Gerar um preview HTML mostrando todos os componentes do quiz (kit completo) e aguardar aprovacao antes de continuar.

---

## Entrada (INPUT)

- `output/dados-quiz.json` (tema, produto, publico, preco)
- `output/quiz.json` (telas geradas — para saber quais componentes existem no quiz)

---

## Saida (OUTPUT)

1. `output/design-tokens.json` — paleta, tipografia, estilo dos componentes
2. `output/design-preview.html` — preview visual do kit completo de componentes do quiz
3. Aprovacao do usuario via AskUserQuestion antes de prosseguir

---

## PASSO 1: Pergunta ao Usuario (1 pergunta — logo no inicio)

**Obrigatorio**: Esta e a UNICA pergunta feita ao usuario nesta skill. Usar `AskUserQuestion` com 1 pergunta antes de qualquer outra acao:

**Pergunta: Cor principal**
- Header: "Cor Principal do Quiz"
- Descricao: "Qual e a cor principal do seu produto ou marca? Pode informar hex (#E63946), nome da cor (vermelho, azul...) ou deixar em branco para definir automaticamente pelo tema."
- Tipo: other
- Exemplo: "#E63946" ou "vermelho" ou deixe em branco

O **estilo visual** (Moderno e Limpo, Bold e Impactante, etc.) e determinado automaticamente no PASSO 2 com base no nicho e publico — nao perguntar ao usuario.

---

## PASSO 2: Raciocinar sobre o Design (em voz alta)

Antes de definir qualquer cor ou fonte, raciocinar e mostrar o raciocinio no chat:

### A. Identificar o nicho e publico

Ler `produto.tema` e `publico.descricao` dos dados coletados. Determinar:
- Categoria: Educacao | Saude/Bem-estar | Financas | Negocios | Marketing | Tech | Outro
- Perfil do publico: Iniciante / Intermediario / Avancado | Faixa etaria estimada | Genero predominante

### A.1. Auto-determinar o estilo visual pelo nicho

Com base na categoria e perfil do publico, escolher automaticamente um dos 5 estilos:

| Nicho / Perfil | Estilo Auto-determinado |
|----------------|------------------------|
| Fitness, vendas, motivacao, leitura rapida, produtividade intensa | **Bold e Impactante** |
| Saude mental, bem-estar, maternidade, meditacao, gestantes | **Suave e Acolhedor** |
| Financas corporativas, juridico, RH, educacao executiva, B2B | **Profissional e Serio** |
| Idiomas, cursos para jovens (18-30), gamificacao, entretenimento | **Energico e Jovem** |
| Produtividade, tech, financas pessoais, minimalismo, SaaS | **Moderno e Limpo** |

Registrar no raciocinio: `Estilo auto-determinado: [ESTILO] — motivo: [nicho/publico identificado]`

### B. Sofisticacao pelo preco

Se `produto.preco` estiver disponivel:
- `< R$ 100` → direto, contrastante, urgente, bold
- `R$ 100–R$ 500` → profissional, limpo, autoridade moderada
- `R$ 500–R$ 2.000` → premium, sofisticado
- `> R$ 2.000` → luxury, minimalista, espaçamento generoso

Se nao houver preco: usar sofisticacao media (R$ 100-500).

### C. Validar a cor do usuario

- Se o usuario forneceu uma cor E ela combina com o nicho → usar como `cor_primaria`
- Se a cor NAO combina: explicar o problema e sugerir 2 alternativas

**Exemplos de conflito a questionar:**
- Nicho: Financas | Cor: Rosa fluorescente → "Para financas, rosa transmite diversao, nao confiança. Sugiro azul marinho #1D3557 ou verde escuro #2D6A4F."
- Nicho: Bem-estar | Cor: Vermelho agressivo → "Vermelho forte cria ansiedade. Para bem-estar, sugiro lilas #6D6875 ou verde suave #52B788."
- Nicho: Vendas/Motivacao | Cor: Cinza neutro → "Cinza nao converte em quiz de vendas. Sugiro vermelho #E63946 ou laranja #F4A261."

Se precisar questionar a cor: usar `AskUserQuestion` com 1 pergunta antes de continuar.

### D. Definir paleta completa

```
cor_primaria     → botoes CTA, barra de progresso, links, destaques
cor_primaria_dark → hover dos botoes (primaria escurecida 15%)
cor_primaria_light → fundo de cards selecionados (primaria em 10% opacidade)
cor_acento       → elementos secundarios, badges, icones especiais
cor_fundo        → SEMPRE #FFFFFF (fixo — todas as telas tem fundo branco)
cor_fundo_card   → fundo dos cards de opcao (levemente diferente do fundo, ex: #F8F8F8)
cor_borda        → borda dos cards e inputs
cor_borda_ativa  → borda dos cards quando selecionados (geralmente cor_primaria)
cor_texto        → texto principal (preto ou cinza muito escuro)
cor_texto_suave  → texto secundario / subtitulos (cinza medio)
cor_progresso_bg → fundo da barra de progresso (cinza claro)
```

### E. Tipografia

O Inlead carrega exclusivamente **Inter** (400, 500, 700) via Google Fonts. Nao suporta outras fontes — qualquer outra escolha sera ignorada pela plataforma.

**Fixo para todos os quizzes**:
- Fonte titulo: Inter, weight 700
- Fonte corpo: Inter, weight 400 e 500
- Google Fonts URL: `https://fonts.googleapis.com/css?family=Inter:400,500,700&display=swap`

A diferenciacao visual entre estilos (Bold, Suave, etc.) deve ser feita exclusivamente por **peso** (700 vs 400), **tamanho**, **espacamento** e **cor** — nao por familia de fonte.

---

## PASSO 3: Gerar `output/design-tokens.json`

```json
{
  "estilo": "string",
  "nicho_identificado": "string",
  "sofisticacao": "basica | media | premium | luxury",
  "paleta": {
    "cor_primaria": "#RRGGBB",
    "cor_primaria_dark": "#RRGGBB",
    "cor_primaria_light": "#RRGGBB (primaria a 10% opacidade sobre branco)",
    "cor_acento": "#RRGGBB",
    "cor_fundo": "#FFFFFF",
    "cor_fundo_card": "#RRGGBB",
    "cor_borda": "#RRGGBB",
    "cor_borda_ativa": "#RRGGBB (igual a cor_primaria)",
    "cor_texto": "#RRGGBB",
    "cor_texto_suave": "#RRGGBB",
    "cor_progresso_bg": "#RRGGBB"
  },
  "tipografia": {
    "google_fonts_url": "https://fonts.googleapis.com/css?family=Inter:400,500,700&display=swap",
    "fonte_titulo": {
      "family": "Inter",
      "weight_normal": "700",
      "weight_bold": "700"
    },
    "fonte_corpo": {
      "family": "Inter",
      "weight_normal": "400",
      "weight_medio": "500"
    }
  },
  "componentes": {
    "botao_raio": "string (ex: 8px, 12px, 50px)",
    "botao_estilo": "solido | gradiente | pill",
    "card_raio": "string (ex: 8px, 12px, 16px)",
    "card_estilo": "flat | elevado | borda-esquerda",
    "progresso_raio": "string (ex: 4px, 8px, 50px)",
    "progresso_altura": "string (ex: 4px, 6px, 8px)"
  }
}
```

Salvar em `output/design-tokens.json`.

---

## PASSO 4: Gerar `output/design-preview.html`

Gerar um HTML completo e autocontido que mostre o KIT VISUAL COMPLETO do quiz.

O preview deve ter as seguintes secoes (em um unico arquivo HTML):

---

### Secao 1 — Cabecalho do Preview

```html
<!-- Titulo do preview -->
<div class="preview-header">
  <h1>[NOME DO PRODUTO] — Kit Visual do Quiz</h1>
  <p>Estilo: [ESTILO] | Nicho: [NICHO] | Cor principal: [HEX]</p>
</div>
```

---

### Secao 2 — Paleta de Cores

Exibir swatches coloridos de todas as cores do design system com hex codes e nomes.

```
[swatch cor_primaria]     [swatch cor_primaria_dark]    [swatch cor_acento]
[swatch cor_fundo]        [swatch cor_fundo_card]       [swatch cor_borda]
[swatch cor_texto]        [swatch cor_texto_suave]      [swatch cor_progresso_bg]
```

---

### Secao 3 — Tipografia

Mostrar exemplos reais de cada nivel tipografico:

```
Titulo principal (fonte_titulo, weight_bold, 28px)
Subtitulo / Pergunta do quiz (fonte_titulo, weight_normal, 20px)
Texto de corpo (fonte_corpo, weight_normal, 16px)
Texto suave / rodape (fonte_corpo, weight_normal, 14px, cor_texto_suave)
```

---

### Secao 4 — Barra de Progresso

A barra de progresso fica **fixada no topo de todas as telas** do quiz, preenchendo conforme o lead avanca.

**Regras obrigatorias**:
- Apenas a barra visual — **sem texto de porcentagem** nenhum
- Transicao suave tanto ao **avancar** (barra cresce) quanto ao **voltar** (barra diminui) — mesma animacao nas duas direcoes
- A transicao e a unica forma de comunicar o progresso

Mostrar 3 exemplos em estados diferentes:
- 25% preenchido
- 60% preenchido
- 90% preenchido

Usar `cor_primaria` para o fill e `cor_progresso_bg` para o fundo.

CSS obrigatorio da barra de progresso:
```css
.barra-progresso-wrapper {
  position: fixed;
  top: 16px; /* margem do topo da tela */
  left: 50%;
  transform: translateX(-50%);
  width: calc(100% - 48px); /* mesma largura do conteudo de texto (padding lateral de 24px em cada lado) */
  max-width: 432px; /* 480px - 2x24px de padding do container */
  height: var(--progresso-altura); /* ex: 6px */
  background: var(--cor-progresso-bg);
  border-radius: var(--progresso-raio);
  z-index: 200;
  /* sem texto, sem contador, sem label */
}
.barra-progresso-fill {
  height: 100%;
  background: var(--cor-primaria);
  border-radius: var(--progresso-raio);
  transition: width 0.5s ease; /* funciona identicamente ao avancar e ao voltar */
}
```

JS para atualizar a barra (funciona nas duas direcoes):
```js
function atualizarProgresso(pct) {
  document.querySelector('.barra-progresso-fill').style.width = pct + '%';
  // a transition CSS cuida da animacao automaticamente, tanto crescendo quanto diminuindo
}
// ao avancar: atualizarProgresso(novoProgresso)
// ao voltar:  atualizarProgresso(progressoAnterior)
```

O conteudo da tela deve ter `padding-top` suficiente para nao ser coberto pela barra. Como a barra fica em `top: 16px` com altura de `progresso_altura`, usar: `padding-top: calc(16px + var(--progresso-altura) + 12px)` (ex: resultado ~34px para barra de 6px).

---

### Secao 5 — Botao CTA (Principal)

Mostrar os estados do botao principal:
- **Estado normal**: fundo `cor_primaria`, texto branco
- **Estado hover**: fundo `cor_primaria_dark`
- **Texto de exemplo**: "Continuar", "Quero saber mais e continuar"

---

### Secao 6 — Cards de Opcao (Escolha Unica)

Mostrar como ficam as opcoes de resposta de uma tela do tipo `escolha-unica`:

**Titulo da pergunta**: "Voce se distrai facilmente durante a leitura?"

Cards de opcao (4 opcoes empilhadas verticalmente):
- **Nao selecionado**: fundo `cor_fundo_card`, borda `cor_borda`, texto `cor_texto`
- **Selecionado/hover**: fundo `cor_primaria_light`, borda `cor_borda_ativa` (2px), texto `cor_texto`
- Mostrar letra da opcao (A, B, C, D) em destaque com `cor_primaria`

```
[A]  Me distraio toda hora, se passar uma mosca perco a atencao
[B]  Ocasionalmente me distraio
[C]  Raramente perco a concentracao
[D]  Sou muito concentrado
```

---

### Secao 7 — Cards de Opcao (Multipla Escolha)

Mostrar como ficam as opcoes de uma tela `multipla-escolha`:

**Titulo**: "Quais sao as consequencias negativas?"
**Subtitulo**: "Selecione uma ou mais opcoes para avancar"

Cards com checkbox visual:
- **Nao selecionado**: borda `cor_borda`, checkbox vazio
- **Selecionado**: borda `cor_borda_ativa`, checkbox preenchido com `cor_primaria`, fundo `cor_primaria_light`

```
[ ] Nao consigo lembrar onde parei a leitura
[x] Perco tempo refazendo a leitura varias vezes   ← mostrar selecionado
[x] A lista de leituras pendentes so aumenta         ← mostrar selecionado
[ ] Nao consigo crescer na carreira
[ ] Abandono livros pela metade
```

---

### Secao 8 — Tela de Carregamento

Mostrar o estado visual do loading screen com o contador animado funcionando.

**Titulo**: "Analisando o seu perfil..."
**Subtitulo**: "Isso levara apenas alguns segundos"

**Contador animado**: numero grande centralizado (ex: 64px, Inter 700, `cor_primaria`) que conta de 0 ate 100 em ~6 segundos. Exibir o simbolo `%` fixo ao lado.

```
         64%
```

**Checklist animado** (cada item aparece conforme o contador avanca):
```
[✓] Analisando suas respostas...           ← concluido (cor_primaria)
[✓] Identificando seu perfil...             ← concluido (cor_primaria)
[·] Calculando seu potencial de melhora...  ← em andamento (pulsando)
[ ] Criando seu plano personalizado...      ← pendente (cinza)
```

Barra de progresso sincronizada com o contador (64% no estado de exemplo).

**JS do contador** (incluir no preview HTML):
```js
function animarContador(duracaoMs) {
  const el = document.getElementById('contador-pct');
  const barra = document.getElementById('loading-barra');
  const itens = document.querySelectorAll('.loading-item');
  const marcos = [0, 25, 50, 75]; // % em que cada item do checklist aparece
  let inicio = null;

  function tick(ts) {
    if (!inicio) inicio = ts;
    const pct = Math.min(Math.round(((ts - inicio) / duracaoMs) * 100), 100);
    el.textContent = pct + '%';
    if (barra) barra.style.width = pct + '%';

    itens.forEach((item, i) => {
      if (pct >= marcos[i]) item.classList.add('ativo');
      if (pct >= marcos[i] + 25 && i < itens.length - 1) item.classList.remove('em-andamento'), item.classList.add('concluido');
    });

    if (pct < 100) requestAnimationFrame(tick);
  }
  requestAnimationFrame(tick);
}
animarContador(6000);
```

---

### Secao 9 — Componente Antes & Depois com Termometro (Tela 17)

Mostrar o componente de resultado usado na Tela 17 do quiz.

**Layout**: 2 colunas lado a lado, separadas por uma linha divisoria vertical

**Coluna Antes**:
- Label "Antes" como badge escuro no topo da imagem
- Imagem quadrada arredondada (ilustracao ou foto de pessoa frustrada/cansada)
- 2 metricas com termometro parcial

**Coluna Depois**:
- Label "Depois" como badge escuro no topo da imagem
- Imagem quadrada arredondada (ilustracao ou foto de pessoa celebrando/energizada)
- 2 metricas com termometro completo

**Cada metrica**:
```
[Nome da metrica — bold, 14px, cor_texto]
[adjetivo — 13px, cor_texto_suave]    [XX% — bold, 14px, cor_texto]
[termometro de 5 segmentos]
```

**Termometro de 5 segmentos**:
- Cada segmento e uma pilula (border-radius: 50px) de altura 10px e largura proporcional, com gap de 4px entre eles
- Cores dos segmentos (fixas, independente da cor_primaria): `#E63946` → `#F4720B` → `#F4C430` → `#90BE6D` → `#43AA8B`
- Estado **Antes**: apenas 1 segmento preenchido (o vermelho), os outros em `#E0E0E0` (cinza claro)
- Estado **Depois**: todos os 5 segmentos preenchidos nas cores acima

CSS do termometro:
```css
.termometro {
  display: flex;
  gap: 4px;
  margin-top: 6px;
}
.termometro-segmento {
  flex: 1;
  height: 10px;
  border-radius: 50px;
  background: #E0E0E0;
}
.termometro-segmento.ativo-1 { background: #E63946; }
.termometro-segmento.ativo-2 { background: #F4720B; }
.termometro-segmento.ativo-3 { background: #F4C430; }
.termometro-segmento.ativo-4 { background: #90BE6D; }
.termometro-segmento.ativo-5 { background: #43AA8B; }
```

Exemplo de metricas a exibir no preview (baseadas no tema do produto lido de `dados-quiz.json`):
```
Antes:                          Depois:
Velocidade de leitura           Velocidade de leitura
Baixa                  22%      Rapida                 96%
[■ □ □ □ □]                     [■ ■ ■ ■ ■]

Foco e Compreensao              Foco e Compreensao
Fraco                  24%      Forte                  92%
[■ □ □ □ □]                     [■ ■ ■ ■ ■]
```

---

### Secao 10 — Exemplo de Tela Completa (Mockup)

Mostrar uma tela completa do quiz simulada (nao precisa ser funcional).

**Layout obrigatorio**: fundo branco (`#FFFFFF`) em toda a pagina. O conteudo do quiz e centralizado horizontalmente com `max-width: 480px` e `margin: 0 auto` — como uma pagina unica sem colunas ou paineis laterais. Nao usar fundo cinza nem contrastes de area.

**Estrutura da tela mockup**:
```
[fundo: #FFFFFF — pagina inteira]

[barra de progresso — FIXA com margem do topo (position: fixed; top: 16px; centrada; largura = conteudo de texto)]
  → preenchida em ~60% (cor_primaria sobre cor_progresso_bg)

[.quiz-container — max-width: 480px; margin: 0 auto; display: flex; flex-direction: column; min-height: 100dvh]

  [.quiz-conteudo — flex: 1; padding-top para nao ser coberta pela barra]
    [seta voltar — canto superior esquerdo]
    [titulo da pergunta]
    [subtitulo se houver]

    [card opcao A]
    [card opcao B]
    [card opcao C]
    [card opcao D]

  [.botao-wrapper — position: sticky; bottom: 0; sempre visivel]
    [botao CTA] — Texto: "Continuar" ou "Quero saber mais e continuar"
```

**Comportamento do botao**:
- Conteudo curto (nao rola): flex empurra o `.botao-wrapper` para o final do `100dvh`, botao fica junto ao conteudo sem espaco vazio entre eles
- Conteudo longo (rola): `position: sticky; bottom: 0` cola o botao no rodape da viewport enquanto o usuario rola

CSS obrigatorio:
```css
body {
  background: #FFFFFF;
  margin: 0;
}
.quiz-container {
  max-width: 480px;
  margin: 0 auto;
  background: #FFFFFF;
  min-height: 100dvh; /* 100dvh evita o problema de barra do browser em mobile */
  display: flex;
  flex-direction: column;
}
.quiz-conteudo {
  flex: 1; /* empurra o botao para o final quando o conteudo e curto */
  padding-top: calc(16px + var(--progresso-altura) + 12px);
  padding-left: 24px;
  padding-right: 24px;
}
.botao-wrapper {
  position: sticky;
  bottom: 0;
  background: #FFFFFF;
  padding: 16px 24px;
  /* sem padding-bottom extra — o safe-area-inset cuida do notch em iPhones */
  padding-bottom: max(16px, env(safe-area-inset-bottom));
}
.botao-cta {
  width: 100%;
  padding: 16px;
  background: var(--cor-primaria);
  color: white;
  font-size: 16px;
  font-weight: 700;
  border: none;
  border-radius: var(--botao-raio);
  cursor: pointer;
}
.botao-cta:hover {
  background: var(--cor-primaria-dark);
}
```

Usar os componentes ja definidos acima. Titulo de exemplo baseado no tema do produto.

---

### CSS do Preview

O HTML do preview deve:
- Importar as fontes do Google Fonts via `<link>`
- Usar CSS variables para todas as cores do design system: `--cor-primaria`, `--cor-fundo-card`, etc.
- Ser responsivo (funcionar bem em mobile e desktop)
- Ter fundo `#FFFFFF` em toda a pagina (sem fundos cinza ou de contraste)
- Conteudo centralizado com `max-width: 480px; margin: 0 auto`
- Cada secao ter titulo descritivo ("Secao 4 — Barra de Progresso")
- Ser autocontido (sem dependencias externas alem do Google Fonts)
- Nao ter copy real — usar exemplos genericos baseados no tema mas nao o conteudo final do quiz
- **Secao 10 deve ocupar 100vw x 100dvh** simulando tela inteira com `display: flex; flex-direction: column`
- **Botao CTA da Secao 10**: usar `.botao-wrapper` com `position: sticky; bottom: 0` dentro do flex container — NAO usar `position: fixed`

---

## PASSO 4.5: Auto-Validacao (antes de salvar)

Antes de salvar e apresentar ao usuario, validar internamente os seguintes pontos. Se qualquer check critico falhar, corrigir e regenerar o preview. Nunca apresentar um design que nao passe nos checks criticos.

### Checks Criticos

1. **Coerencia com nicho**: A `cor_primaria` combina com o tema e publico do produto (cor vibrante para fitness ✓, cor vibrante para juridico ✗)
2. **Paleta completa**: Todos os 11 tokens definidos. `cor_primaria` nao e cinza nem branco. `cor_fundo` = #FFFFFF. `cor_fundo_card` diferente de #FFFFFF. `cor_borda` e `cor_borda_ativa` diferentes entre si. Contraste texto/fundo minimo 4.5:1
3. **Cards de opcao**: Estado selecionado e nao-selecionado visivelmente distintos no preview (Secoes 6 e 7)
4. **Botao CTA**: Alto contraste entre texto e fundo do botao. Hover definido. Elemento mais chamativo da tela

### Checks de Qualidade

5. **Barra de progresso**: Altura e raio definidos. 3 estados no preview (25%, 60%, 90%)
6. **Loading screen**: Checklist com 3 estados visuais (concluido, em andamento, pendente). Barra de progresso presente
7. **Tipografia**: Inter como fonte (unica suportada pelo Inlead). Peso do titulo > peso do corpo
8. **Kit completo**: As 10 secoes presentes no preview HTML
9. **Identidade propria**: Combinacao cor+fonte nao e a mais generica possivel (ex: #007BFF + Roboto + radius 4px = template padrao). Zero emojis em qualquer lugar do preview

Se todos os checks passam: salvar e apresentar ao usuario (PASSO 5).
Se algum check critico falha: corrigir e regenerar. Nao perguntar ao usuario sobre problemas tecnicos.

---

## PASSO 5: Apresentar ao Usuario e Aguardar Aprovacao

Apos salvar os dois arquivos, usar a ferramenta `Read` em `output/design-preview.html` e em `output/design-tokens.json` para que apareçam como cards clicaveis no chat. NAO mencionar caminhos de arquivo como texto.

Em seguida, retornar ao chat:

```
Design system do quiz criado!

  • Estilo: [estilo]
  • Nicho: [nicho identificado]
  • Cor principal: [HEX] — [nome da cor]
  • Fonte titulo: [fonte] | Corpo: [fonte]
  • Botao: [estilo] com raio [raio]
  • Cards: [estilo] com raio [raio]
  • Progresso: [altura] com raio [raio]

Clique nos arquivos acima para ver o preview e os tokens de design.
```

Em seguida, perguntar no chat (texto simples — NAO usar `AskUserQuestion` neste momento especifico):

"O design system esta do seu agrado? Responda **Aprovado** para continuar ou descreva os ajustes que deseja na paleta."

**Se aprovar**: Retornar para o fluxo do comando. Voltar a usar `AskUserQuestion` normalmente a partir daqui.

**Se querer ajuste de cores**: Usar `AskUserQuestion` com 1 pergunta (tipo: other) pedindo a descricao dos ajustes. Atualizar a paleta, regerar o preview HTML, usar `Read` nos arquivos atualizados e perguntar aprovacao novamente no chat (texto simples, sem `AskUserQuestion`).

---

## Procedimento Resumido

1. Usar AskUserQuestion — 1 pergunta (cor principal) — PRIMEIRA acao da skill
2. Raciocinar em voz alta: nicho → estilo auto-determinado → preco → cor → paleta → tipografia
3. Se cor do usuario nao combina com o nicho: usar `AskUserQuestion` com 1 pergunta (sugestoes de cor) e aguardar resposta
4. Montar e salvar `output/design-tokens.json`
5. Gerar e salvar `output/design-preview.html` com as 10 secoes do kit (sem tela de captura)
4.5. Auto-validar os 9 checkpoints — se falhar, corrigir e regenerar
6. Usar `Read` em `output/design-preview.html` e `output/design-tokens.json` — nunca mencionar os caminhos como texto
7. Retornar resumo ao chat + pergunta de aprovacao no chat (texto simples — sem AskUserQuestion)
7. Se ajuste de cores: refazer paleta e preview, usar Read nos arquivos e perguntar aprovacao novamente no chat (texto simples)
8. Se aprovado: prosseguir para o revisor-quiz

---

## REGRA DE PERGUNTAS (OBRIGATORIO)

**Sempre usar a ferramenta `AskUserQuestion`** para qualquer pergunta, escolha ou coleta de input do usuario. Em todos os casos, sem excecao. Nunca fazer perguntas apenas no texto do chat.

---

## Restricoes

- NAO emitir o JSON completo no chat (foi salvo em arquivo)
- NAO usar copy real do quiz no preview — so exemplos genericos baseados no tema
- NAO usar CDNs externas alem do Google Fonts
- SEMPRE raciocinar em voz alta antes de definir cores
- SEMPRE questionar cores que conflitam com o nicho
- SEMPRE aguardar aprovacao antes de retornar ao fluxo
- **ZERO emojis em qualquer lugar** — nem no preview HTML, nem nos tokens, nem no chat. Proibido usar qualquer emoji decorativo, especialmente os classicos de IA: 🎯 (alvo), ⚡ (raio), 📊 (grafico), ✨ (brilho), 🚀 (foguete), 💡 (lampada), 🔥 (fogo), ⭐ (estrela) e similares. O design deve ser limpo, profissional e baseado exclusivamente em tipografia, cores e layout — nunca em emojis.

---

## Exemplo de Raciocinio (tema: Leitura Rapida)

```
Analisando o produto:
  - Tema: Leitura rapida e memorizacao
  - Nicho: Educacao / Desenvolvimento Pessoal
  - Preco: R$ 197 → sofisticacao media (profissional, limpo, autoridade moderada)
  - Publico: Adultos produtivos, 25-45 anos, ambos os generos

Estilo auto-determinado: Bold e Impactante
  - Motivo: Desenvolvimento pessoal + motivacao para aprender → tipografia forte, alto contraste

Cor fornecida pelo usuario: nao informada
Auto-determinando cor pelo nicho:
  - Educacao + Desenvolvimento Pessoal + Bold = vermelho vibrante
  - Cor primaria: #E63946 (vermelho impactante — transmite urgencia e acao)
  - Alternativa caso o usuario queira: #2D6A4F (verde escuro — crescimento)

Tipografia: Inter (unica fonte suportada pelo Inlead)
  - Titulo: Inter 700 — peso maximo disponivel para impacto
  - Corpo: Inter 400/500 — legibilidade

Componentes:
  - Botao: pill (border-radius 50px) — moderno, destaca o CTA
  - Cards: elevado com borda-ativa (box-shadow + borda 2px cor_primaria quando selecionado)
  - Progresso: 6px de altura, border-radius 50px (suave)
```
