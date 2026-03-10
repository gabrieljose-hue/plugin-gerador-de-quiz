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

## PASSO 1: Perguntas ao Usuario (2 perguntas)

Usar `AskUserQuestion` com 2 perguntas:

**Pergunta 1: Cor principal**
- Header: "Cor Principal"
- Descricao: Qual e a cor principal do seu produto ou marca? Sera usada nos botoes, barra de progresso e destaques. Pode informar hex, nome da cor, ou deixar em branco para definir automaticamente pelo tema.
- Tipo: other
- Exemplo: "#E63946" ou "vermelho" ou deixe em branco

**Pergunta 2: Estilo visual**
- Header: "Estilo Visual"
- Tipo: single select
- Opcoes:
  - "Moderno e Limpo" (description: "Minimalista, muito espaco em branco, fontes leves. Ex: produtividade, tech, financas pessoais")
  - "Bold e Impactante" (description: "Cores vibrantes, tipografia forte, alto contraste. Ex: fitness, vendas, motivacao, leitura rapida")
  - "Suave e Acolhedor" (description: "Tons pasteis, fontes arredondadas, elementos organicos. Ex: saude mental, bem-estar, maternidade")
  - "Profissional e Serio" (description: "Cores sobrias, tipografia classica, layout estruturado. Ex: financas corporativas, juridico, RH, educacao executiva")
  - "Energico e Jovem" (description: "Cores saturadas, dinamismo, grafismos modernos. Ex: idiomas, cursos para jovens, gamificacao")

---

## PASSO 2: Raciocinar sobre o Design (em voz alta)

Antes de definir qualquer cor ou fonte, raciocinar e mostrar o raciocinio no chat:

### A. Identificar o nicho e publico

Ler `produto.tema` e `publico.descricao` dos dados coletados. Determinar:
- Categoria: Educacao | Saude/Bem-estar | Financas | Negocios | Marketing | Tech | Outro
- Perfil do publico: Iniciante / Intermediario / Avancado | Faixa etaria estimada | Genero predominante

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
cor_fundo        → fundo geral das telas (branco ou off-white)
cor_fundo_card   → fundo dos cards de opcao (levemente diferente do fundo)
cor_borda        → borda dos cards e inputs
cor_borda_ativa  → borda dos cards quando selecionados (geralmente cor_primaria)
cor_texto        → texto principal (preto ou cinza muito escuro)
cor_texto_suave  → texto secundario / subtitulos (cinza medio)
cor_progresso_bg → fundo da barra de progresso (cinza claro)
```

### E. Tipografia

Escolher do Google Fonts baseado no estilo:

| Estilo | Fonte Titulo | Fonte Corpo | Pesos |
|--------|-------------|-------------|-------|
| Moderno e Limpo | Inter | Inter | 400,600,700 |
| Bold e Impactante | Montserrat | Open Sans | 700,800,900 / 400,600 |
| Suave e Acolhedor | Nunito | Nunito | 400,600,700 |
| Profissional e Serio | Lato | Lato | 400,700 |
| Energico e Jovem | Poppins | Poppins | 400,600,700 |

Nao usar sempre as mesmas fontes — variar baseado no nicho:
- Leitura/Aprendizado: Plus Jakarta Sans
- Saude Mental: Nunito ou DM Sans
- Negocios Premium: Inter + DM Serif Display
- Tech: JetBrains Mono para destaques + Inter para corpo

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
    "cor_fundo": "#RRGGBB",
    "cor_fundo_card": "#RRGGBB",
    "cor_borda": "#RRGGBB",
    "cor_borda_ativa": "#RRGGBB (igual a cor_primaria)",
    "cor_texto": "#RRGGBB",
    "cor_texto_suave": "#RRGGBB",
    "cor_progresso_bg": "#RRGGBB"
  },
  "tipografia": {
    "google_fonts_url": "https://fonts.googleapis.com/css2?family=...",
    "fonte_titulo": {
      "family": "string",
      "weight_normal": "700",
      "weight_bold": "800"
    },
    "fonte_corpo": {
      "family": "string",
      "weight_normal": "400",
      "weight_medio": "600"
    }
  },
  "componentes": {
    "botao_raio": "string (ex: 8px, 12px, 50px)",
    "botao_estilo": "solido | gradiente | pill",
    "card_raio": "string (ex: 8px, 12px, 16px)",
    "card_estilo": "flat | elevado | borda-esquerda",
    "input_raio": "string (ex: 8px, 12px)",
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

Mostrar 3 exemplos em estados diferentes:
- 25% preenchido
- 60% preenchido
- 90% preenchido

Usar `cor_primaria` para o fill e `cor_progresso_bg` para o fundo.

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

### Secao 8 — Tela de Captura (Formulario)

Mostrar como fica a tela de nome e email:

**Titulo**: "Qual e o seu nome?"

```
[Campo de texto: "Seu primeiro nome"]
[Campo de texto: "seu@email.com"]
[Botao: "Continuar" →]
```

Estilo dos inputs: fundo branco, borda `cor_borda`, raio `input_raio`, foco com borda `cor_primaria`.

---

### Secao 9 — Tela de Carregamento

Mostrar o estado visual do loading screen:

**Titulo**: "Analisando o seu perfil..."
**Subtitulo**: "Isso levara apenas alguns segundos"

Checklist animado (mostrar estado intermediario — 2 de 4 concluidos):
```
[✓] Analisando suas respostas...           ← concluido (cor_primaria)
[✓] Identificando seu perfil...             ← concluido (cor_primaria)
[·] Calculando seu potencial de melhora...  ← em andamento (pulsando)
[ ] Criando seu plano personalizado...      ← pendente (cinza)
```

Barra de progresso em 55%.

---

### Secao 10 — Exemplo de Tela Completa (Mockup)

Mostrar uma tela completa do quiz simulada (nao precisa ser funcional):

**Estrutura da tela mockup**:
```
[barra de progresso topo]
[seta voltar]

[titulo da pergunta]
[subtitulo se houver]

[card opcao A]
[card opcao B]
[card opcao C]
[card opcao D]

[botao continuar — aparece apos selecionar]
```

Usar os componentes ja definidos acima. Titulo de exemplo baseado no tema do produto.

---

### CSS do Preview

O HTML do preview deve:
- Importar as fontes do Google Fonts via `<link>`
- Usar CSS variables para todas as cores do design system: `--cor-primaria`, `--cor-fundo-card`, etc.
- Ser responsivo (funcionar bem em mobile e desktop)
- Ter fundo `#F5F5F5` para contrastar com os componentes
- Cada secao ter titulo descritivo ("Secao 4 — Barra de Progresso")
- Ser autocontido (sem dependencias externas alem do Google Fonts)
- Nao ter copy real — usar exemplos genericos baseados no tema mas nao o conteudo final do quiz

---

## PASSO 5: Apresentar ao Usuario e Aguardar Aprovacao

Apos salvar os dois arquivos, retornar ao chat:

```
Design system do quiz criado!

  • Estilo: [estilo]
  • Nicho: [nicho identificado]
  • Cor principal: [HEX] — [nome da cor]
  • Fonte titulo: [fonte] | Corpo: [fonte]
  • Botao: [estilo] com raio [raio]
  • Cards: [estilo] com raio [raio]
  • Progresso: [altura] com raio [raio]

Abra output/design-preview.html para ver o kit completo de componentes.
```

Em seguida, usar `AskUserQuestion` com 1 pergunta:
- Pergunta: "O design system esta do seu agrado?" (header: "Aprovacao do Design")
- Opcoes:
  - "Aprovado, continuar" (description: "O kit visual esta bom, seguir para a revisao do quiz")
  - "Quero ajustar as cores" (description: "Vou descrever o que quero mudar na paleta")
  - "Quero mudar o estilo visual" (description: "Quero escolher outro estilo para o quiz")

**Se aprovar**: Confirmar e retornar para o fluxo do comando.

**Se querer ajuste de cores**: Pedir descricao dos ajustes, atualizar a paleta, regrear o preview HTML e perguntar novamente.

**Se querer mudar o estilo**: Pedir para escolher o novo estilo (mostrar as 5 opcoes novamente), refazer tudo e perguntar novamente.

---

## Procedimento Resumido

1. Usar AskUserQuestion — 2 perguntas (cor + estilo)
2. Raciocinar em voz alta: nicho → preco → cor → paleta → tipografia
3. Se cor do usuario nao combina: questionar e aguardar resposta
4. Montar e salvar `output/design-tokens.json`
5. Gerar e salvar `output/design-preview.html` com as 10 secoes do kit
6. Retornar resumo ao chat + AskUserQuestion de aprovacao
7. Se ajustes: refazer e perguntar de novo
8. Se aprovado: prosseguir para o revisor-quiz

---

## Restricoes

- NAO emitir o JSON completo no chat (foi salvo em arquivo)
- NAO usar copy real do quiz no preview — so exemplos genericos baseados no tema
- NAO usar CDNs externas alem do Google Fonts
- SEMPRE raciocinar em voz alta antes de definir cores
- SEMPRE questionar cores que conflitam com o nicho
- SEMPRE aguardar aprovacao antes de retornar ao fluxo
- Sem emojis nos componentes do preview

---

## Exemplo de Raciocinio (tema: Leitura Rapida, Bold e Impactante)

```
Analisando o produto:
  - Tema: Leitura rapida e memorizacao
  - Nicho: Educacao / Desenvolvimento Pessoal
  - Preco: R$ 197 → sofisticacao media (profissional, limpo, autoridade moderada)
  - Publico: Adultos produtivos, 25-45 anos, ambos os generos

Cor fornecida pelo usuario: nao informada
Auto-determinando cor pelo nicho:
  - Educacao + Desenvolvimento Pessoal + Bold = vermelho vibrante
  - Cor primaria: #E63946 (vermelho impactante — transmite urgencia e acao)
  - Alternativa caso o usuario queira: #2D6A4F (verde escuro — crescimento)

Tipografia para Bold e Impactante:
  - Titulo: Montserrat 800 — autoridade, impacto
  - Corpo: Open Sans 400/600 — legibilidade

Componentes:
  - Botao: pill (border-radius 50px) — moderno, destaca o CTA
  - Cards: elevado com borda-ativa (box-shadow + borda 2px cor_primaria quando selecionado)
  - Progresso: 6px de altura, border-radius 50px (suave)
```
