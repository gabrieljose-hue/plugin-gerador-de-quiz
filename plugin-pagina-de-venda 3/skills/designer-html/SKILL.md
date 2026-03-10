---
name: designer-html
description: Analisa nicho e produto para definir design system (cores, tipografia, estética). Gera preview HTML e aguarda aprovação do mentorado.
---

# Skill: Designer de HTML

## Missão

Pensar como um designer profissional: analisar o nicho, preço e produto para definir uma paleta de cores, tipografia e estética visual. Questionar a cor principal se não combinar com o nicho. Gerar um preview visual e aguardar aprovação do mentorado.

---

## Entrada (INPUT)

- `output/copy-estruturada.json` — Leia o arquivo gerado pelo extrator-copy (Passo 3)
- `COR_PRINCIPAL` — Cor hex sugerida pelo mentorado (ex: `#FF6B00`)
- `NOME_PRODUTO` — Nome do produto
- `PRECO_PRODUTO` — Preço (ex: `R$ 297`)

---

## Saída (OUTPUT)

### Output 1 — Pergunta sobre cor (se necessário)

Se a cor principal não combinar com o nicho identificado, usar `AskUserQuestion` com 1 pergunta:

- Pergunta: "Analisei seu produto ([nicho], faixa R$[X]). A cor que você escolheu (#XXXX) [problema]. Para esse perfil, sugiro #YYYY porque [razão]. Qual cor prefere?" (header: "Cor")
- Opções:
  - "Manter minha cor original (#XXXX)" (description: "Seguir com a cor que eu escolhi")
  - "Usar a sugestão (#YYYY)" (description: "[razão resumida da sugestão]")
  - "Outra cor" (description: "Vou informar outro hex")

Aguardar a resposta e confirmar a cor escolhida.

### Output 2 — Preview HTML

Salvar em: **`output/design-preview.html`**

Um style guide visual em HTML contendo:
- Paleta de cores (swatches com hex codes)
- Tipografia (headline / subtítulo / corpo / CTA) em tamanho real
- Botão CTA no design escolhido
- Cards de exemplo (benefício, depoimento)
- Seção hero em miniatura (fundo + headline + botão)
- Seção escura de exemplo
- **Importante**: Nenhum copy real — usar apenas "Lorem ipsum" ou texto de exemplo para o mentorado focar no visual

### Output 3 — DESIGN_TOKENS (salvo em arquivo)

1. **Gere o DESIGN_TOKENS JSON** (estrutura abaixo)
2. **Salve em arquivo**: `output/design-tokens.json`
3. **NÃO emita o JSON completo no chat** (para economizar tokens)

**Estrutura do DESIGN_TOKENS**:
```json
{
  "big_idea_visual": "descrição do elemento/conceito visual único que encarna a Big Idea",
  "identidade_comunicador": "resumo de como o comunicador quer ser visto visualmente",
  "identidade_consumidor": "resumo do estado aspiracional visual (após transformação)",
  "identidade_produto": "resumo visual do que o produto representa",
  "estetica": "premium | bold | minimalista | urgente | tecnico | inspiracional",
  "cores": {
    "primary": "#hex",
    "primary_dark": "#hex (primary escurecido 15-20%)",
    "primary_light": "#hex (primary clareado 80-90% — tint sutil)",
    "accent": "#hex",
    "bg_dark": "#hex",
    "bg_dark_alt": "#hex (bg_dark levemente mais claro, +5-8% luminosidade)",
    "bg_light": "#hex",
    "bg_light_alt": "#hex (bg_light levemente mais escuro, -3-5% luminosidade)",
    "text_dark": "#hex",
    "text_muted": "#hex (text_dark com 60% opacidade equivalente)",
    "text_light": "#hex",
    "text_muted_light": "#hex (text_light com 60% opacidade equivalente)"
  },
  "tipografia": {
    "google_fonts_url": "https://fonts.googleapis.com/...",
    "titulo": {
      "family": "Font Name",
      "weights": "700,800,900"
    },
    "subtitulo": {
      "family": "Font Name",
      "weights": "400,600"
    },
    "corpo": {
      "family": "Font Name",
      "weights": "400,500"
    }
  },
  "espacamento": {
    "secao_padding": "60px | 80px | 100px | 120px",
    "border_radius": "4px | 8px | 16px | 24px"
  },
  "elementos": {
    "separador": "linha | gradiente | nenhum | ondulado",
    "cards_estilo": "flat | elevado | borda-esquerda | glassmorphism",
    "botao_estilo": "solido | outline | gradiente | arredondado"
  }
}
```

### Output 4 — Preview inline + Aprovação

Após salvar `output/design-preview.html` com a tool Write:

1. **Retornar ao chat** o resumo do design:
   ```
   Resumo do design:
     • Estética: [estética escolhida]
     • Big Idea Visual: [descrição resumida]
     • Paleta: primary [#hex] | accent [#hex]
     • Tipografia: [Título] / [Subtítulo] / [Corpo]
     • Espaçamento: [secao_padding] padding, [border_radius] border-radius

   Clique no card acima para visualizar o preview do design.
   ```

2. **Na mesma resposta**, usar `AskUserQuestion` com 1 pergunta:
   - Pergunta: "O design está do seu agrado?" (header: "Preview")
   - Opções:
     - "Aprovado, seguir em frente" (description: "O design está bom, continuar para revisão")
     - "Quero ajustar" (description: "Vou descrever o que quero mudar")

**IMPORTANTE**: NÃO usar nenhum padrão de espera bloqueante. A tool Write gera um card clicável no chat — o mentorado clica nele para ver o preview e depois responde a pergunta de aprovação. Tudo na mesma resposta.

Se o mentorado pedir ajustes: refazer o preview e perguntar novamente.
Se aprovar: confirmação de que design está pronto para Passo 4.5

---

## Raciocínio de Design (obrigatório)

Antes de definir cores e tipografia, raciocinar em voz alta:

### 0. Entender os 3i's do Marketing (novo passo!)

Extrair do JSON_ESTRUTURADO os 3i's:

- **Identidade do Comunicador**: Como o criador quer ser visto? (especialista, mentor, criador, inovador, etc)
- **Identidade do Consumidor**: Como o cliente quer se ver após o produto? (aspiracional, transformado, empoderado, etc)
- **Identidade do Produto**: O que o produto representa e promete?

**Usar na paleta e estética**: O design deve refletir visualmente esses 3 aspectos:
- Se comunicador é "mentor amigável" → cores quentes, tipografia approachable
- Se consumidor quer se ver "empoderado/profissional" → design deve transmitir autoridade
- Se produto é "transformação" → usar gradientes, movimento visual

### 1. Identificar o nicho

Analisar o JSON_ESTRUTURADO e determinar:
- Educação/Cursos?
- Finanças/Investimentos?
- Saúde/Fitness?
- Coaching/Mentoria?
- Negócios/Empreendedorismo?
- Beleza/Moda?
- Tecnologia/SaaS?
- Outro?

### 2. Nível de sofisticação pelo preço

- `< R$ 200` → direto, urgente, contrastante, bold
- `R$ 200–R$ 1.000` → profissional, limpo, autoridade
- `R$ 1.000–R$ 5.000` → premium, sofisticado, elegante
- `> R$ 5.000` → luxury, minimalista, espaçamento generoso

### 2.5. Identificar a Big Idea Visual (novo!)

Antes de escolher estética, extrair a **Big Idea** do copy:

- Qual é a "sacada criativa" que diferencia este produto?
- Qual é o insight único que faz as pessoas pararem para ouvir?
- Como traduzir isso **visualmente**?

**Exemplos de Big Idea → Visual**:

| Big Idea | Tradução Visual |
|----------|-----------------|
| "Aprenda inglês assistindo séries" | Usar imagens de tela/série na hero, cores inspiradas em cinema |
| "15 minutos por dia, sem sair de casa" | Design compacto, seções curtas, ícone de relógio em destaque |
| "De inadimplente a milionário" | Gráfico de crescimento visual, transformação de cores (vermelho → verde) |
| "Transformação interior refletida no exterior" | Espelho visual, antes/depois lado a lado, contraste de cores |

**Como usar**:
- Escolha um elemento visual único que encarne a Big Idea
- Repita esse elemento ao longo do design (coerência)
- Use-o na seção hero, botões, separadores

### 3. Definir estética

Escolher uma: `bold | minimalista | elegant | urgente | tecnico | inspiracional`

**Dica**: A estética deve refletir a Big Idea Visual identificada acima.

### 4. Paleta de cores

Definir 6 cores:
- `PRIMARY`: principal dos CTAs (pode ser diferente do que o mentorado sugeriu)
- `ACCENT`: detalhes, bordas, ícones
- `BG_DARK`: seções escuras
- `BG_LIGHT`: seções claras
- `TEXT_DARK`: texto em seções claras
- `TEXT_LIGHT`: texto em seções escuras

### 5. Tipografia

Escolher 3 famílias (não usar sempre Montserrat+Open Sans):
- **Título** (Headline): família + weights (700, 800, 900)
- **Subtítulo**: família + weights (400, 600)
- **Corpo**: família + weights (400, 500)

**Exemplos por nicho**:
- Negócios → Inter + DM Sans
- Premium → Playfair Display + Lato
- Urgência → Oswald + Roboto
- Coaching → Raleway + Nunito
- Educação → Plus Jakarta Sans + Nunito
- Finanças → Inter + DM Sans
- Saúde/Fitness → Oswald + Roboto
- Beleza/Moda → Cormorant Garamond + Jost

### 6. Questionar a cor do mentorado

- **Se combinar com o nicho**: usar diretamente como `PRIMARY`
- **Se não combinar**: mostrar o problema e sugerir 2 alternativas com hex code

**Exemplos de questionamento**:
- Nicho: Finanças | Cor sugerida: Rosa fluorescente → "Para finanças, você precisa de confiança, não diversão."
- Nicho: Fitness | Cor sugerida: Azul pastel → "Esse azul é muito suave. Fitness precisa de energia!"
- Nicho: Coaching | Cor sugerida: Cinza → "Cinza é corporativo demais. Coaching precisa de calore e inspiração."

---

## Referência: Tabelas por nicho

### Tabela 1: Estética e tipografia por nicho

| Nicho | Estética | Fontes sugeridas | Cor primária comum |
|-------|----------|------------------|--------------------|
| Finanças/Investimentos | Sóbrio, confiança | Inter + DM Sans | Azul marinho (#003366), verde (#1B7245) |
| Saúde/Fitness | Energético, vitality | Oswald + Roboto | Verde (#22C55E), laranja (#FF8C00), vermelho (#EF4444) |
| Educação/Cursos | Profissional, moderno | Plus Jakarta Sans + Nunito | Azul (#3B82F6), roxo (#8B5CF6), verde (#10B981) |
| Coaching/Mentoria | Inspiracional, cálido | Raleway + Nunito | Dourado (#F59E0B), bordô (#991B1B), terracota (#CC5500) |
| Negócios/Empreendedorismo | Bold, autoridade | Barlow + Inter | Preto (#111111), azul (#1E40AF), cinza (#374151) |
| Beleza/Moda | Elegant, suave | Cormorant Garamond + Jost | Rosa (#EC4899), nude (#F5DEB3), preto (#000000) |
| Tecnologia/SaaS | Técnico, clean | Inter + Fira Sans | Azul elétrico (#0EA5E9), roxo (#A78BFA) |

### Tabela 2: Sofisticação por preço

| Faixa | Estilo | Padding | Border-radius | Tipografia | Exemplo |
|-------|--------|---------|---------------|------------|---------|
| < R$ 200 | Urgente, bold | Compacto (60px) | Baixo (4px) | Pesada, contrastante | Emagrecimento express |
| R$ 200–R$ 1.000 | Profissional, limpo | Médio (80px) | Médio (8px) | Equilibrada | Curso de programação |
| R$ 1.000–R$ 5.000 | Premium, autoridade | Generoso (100px) | Alto (16px) | Refinada | Mastermind de negócios |
| > R$ 5.000 | Luxury, minimalista | Muito generoso (120px+) | Muito alto (24px) | Elegante, leve | Consultoria premium |

---

## Procedimento

1. **Ler JSON_ESTRUTURADO** e extrair nicho, benefícios, público
2. **Analisar preço** para determinar sofisticação
3. **Raciocinar em voz alta** sobre nicho, estética, cores
4. **Comparar cor do mentorado** com o nicho
5. **Se não combinar**: perguntar qual preferir (original, sugestão, ou custom)
6. **Gerar preview HTML** com design system visual
7. **Criar DESIGN_TOKENS** JSON
8. **Salvar em arquivo**: `output/design-tokens.json` (criar arquivo se não existir)
9. **Salvar em arquivo**: `output/design-preview.html` (já existente no step 6)
10. **Retornar ao chat**: APENAS o resumo de 5 linhas (não o JSON completo)
11. **Aguardar aprovação** ou ajustes do mentorado

---

## Mapeamento: `elementos` → CSS concreto

Os construtores de bloco usam os valores de `DESIGN_TOKENS.elementos` para aplicar CSS específico. A tabela abaixo mostra o que cada opção produz:

### `botao_estilo`

| Valor | Comportamento CSS |
|-------|-------------------|
| `solido` | `background: var(--primary); border: none;` — hover escurece 10% |
| `gradiente` | `background: linear-gradient(135deg, var(--primary), var(--primary-dark));` — hover inverte direção |
| `outline` | `background: transparent; border: 2px solid var(--primary); color: var(--primary);` — hover preenche |
| `arredondado` | `border-radius: 50px; background: var(--primary);` — pill shape, hover com sombra |

### `cards_estilo`

| Valor | Comportamento CSS |
|-------|-------------------|
| `flat` | Sem sombra, fundo sólido, borda 1px sutil |
| `elevado` | `box-shadow` em 2 níveis (repouso + hover), sem borda |
| `borda-esquerda` | `border-left: 4px solid var(--primary);` fundo sutil |
| `glassmorphism` | `backdrop-filter: blur(10px); background: rgba(255,255,255,0.08); border: 1px solid rgba(255,255,255,0.15);` |

### `separador`

| Valor | Comportamento CSS |
|-------|-------------------|
| `linha` | `border-bottom: 1px solid` com opacidade 10-15% |
| `gradiente` | `background: linear-gradient(90deg, transparent, var(--primary), transparent); height: 2px;` |
| `ondulado` | SVG inline com curva suave (`<svg viewBox="0 0 1200 40">`) entre seções |
| `nenhum` | Sem separador entre seções |

### Cores extras: como computar

Ao definir o DESIGN_TOKENS, compute automaticamente as variantes:

1. **`primary_dark`**: Escurecer `primary` em 15-20% (ex: `#FF6B00` → `#CC5500`)
2. **`primary_light`**: Clarear `primary` para tint 80-90% (ex: `#FF6B00` → `#FFF0E6`)
3. **`bg_dark_alt`**: `bg_dark` com +5-8% luminosidade (ex: `#0D0D0D` → `#1A1A1A`)
4. **`bg_light_alt`**: `bg_light` com -3-5% luminosidade (ex: `#F7F7F7` → `#EDEDED`)
5. **`text_muted`**: `text_dark` diluído (ex: `#222222` → `#666666`)
6. **`text_muted_light`**: `text_light` diluído (ex: `#FFFFFF` → `#AAAAAA`)

---

## ⚠️ Restrições

- ❌ Não invente informações do JSON
- ❌ O preview é APENAS visual — sem copy real
- ❌ Não use CDNs externas (exceto Google Fonts)
- ❌ **NÃO emita o JSON completo do DESIGN_TOKENS no chat** (foi salvo em arquivo)
- ✅ Sempre raciocine em voz alta sobre o design
- ✅ Sempre questione cores que não combinam
- ✅ Sempre salve em `output/design-preview.html`
- ✅ Sempre salve em `output/design-tokens.json`
- ✅ Sempre retorne ao chat APENAS o resumo de 5 linhas
- ✅ Sempre aguarde aprovação antes de retornar final
- ✅ Sempre incluir as 6 cores extras (primary_dark, primary_light, bg_dark_alt, bg_light_alt, text_muted, text_muted_light)

---

## Exemplo de fluxo

**INPUT:**
```
JSON_ESTRUTURADO: { produto: { nome: "Python para Finanças", tipo: "Curso", preco: "R$ 1.997" }, ... }
COR_PRINCIPAL: #FF6B00 (laranja fluorescente)
NOME_PRODUTO: Python para Finanças
PRECO_PRODUTO: R$ 1.997
```

**RACIOCÍNIO (mostrado ao mentorado):**
```
Analisei seu produto: Educação em Finanças, faixa de preço R$ 1.997 (premium).

Para esse perfil, preciso de:
- Nicho: Finanças → confiança, profissionalismo, sobriedade
- Preço: Premium → espaçamento generoso, tipografia refinada
- Estética ideal: Sóbrio com elegância

A cor que você escolheu (#FF6B00 laranja) é muito vibrante e informal.
Para finanças, ela transmite diversão, não confiança.

Sugiro:
A) Manter laranja: #FF6B00 (mais dinâmico, menos tradicional)
B) Azul marinho: #003366 (confiança máxima, clássico)
C) Verde escuro: #1B7245 (crescimento, investimento)
D) Outra cor (informe hex)
```

**OUTPUT (após aprovação):**
1. `output/design-preview.html` — visual style guide
2. `<DESIGN_TOKENS>...</DESIGN_TOKENS>` — JSON de design
