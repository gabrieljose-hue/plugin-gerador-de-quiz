---
name: construtor-bloco1
description: Gera as seções 1-5 (BLOCO 1 Hero/Persuasão) do HTML com <!DOCTYPE>, <head>, <style>, <script>, opening <body>.
---

# Skill: Construtor de BLOCO 1 (Hero/Persuasão)

## Missão

Gerar as **seções 1-5** (BLOCO 1: Hero/Persuasão) do HTML da página de vendas, incluindo a estrutura base do documento (<!DOCTYPE>, <head>, <style>, <script>) e abertura da tag <body>.

---

## Entrada (INPUT)

**Leia os arquivos** (não são fornecidos no chat):
- `output/config-pagina.json` — Respostas do mentorado (Passo 2)
- `output/design-tokens.json` — Design system validado (Passo 4)
- `output/copy-estruturada.json` — Copy estruturado (Passo 3)

---

## Saída (OUTPUT)

Arquivo salvo em: **`output/bloco1.html`**

**Estrutura**:
- Começa com `<!DOCTYPE html>`
- Inclui: `<meta>` tags, `<title>`, link de Google Fonts
- Inclui: `<style>` com todas as regras CSS (inline)
- Inclui: `<script>` com JS inline para scroll behavior (se necessário)
- Inclui: abertura de `<body>`
- **NÃO fecha** `</body></html>` (será adicionado pelo bloco4)

**Retorno ao chat**:
- ✅ Confirmação: "Bloco 1 salvo em output/bloco1.html"
- Sem emitir o HTML no chat

---

## Estrutura HTML — BLOCO 1

**CRÍTICO — REGRA INVIOLÁVEL**: Os pontos 1, 2, 2.5 e 3 (Logo, Headline, Subheadline, Urgência, Bullets, Vídeo, Botão CTA) ficam TODOS dentro de **UMA ÚNICA `<section id="hero">`**. NÃO crie sections separadas para bullets, vídeo ou botão. Tudo é separado internamente por `<div class="container">`, mas dentro da MESMA section. Se você criar múltiplas sections para esses elementos, a página ficará com o hero quebrado visualmente.

```html
<section id="hero" class="section--dark">
  <!-- Ponto 1: Logo + Headline + Subheadline + Urgência -->
  <div class="container text-center">
    [Logo, Headline, Subheadline, Urgência]
  </div>

  <!-- Ponto 2: Bullet points (CTA Text) -->
  <div class="container container--text">
    [Bullets mostrando benefícios/resultados]
  </div>

  <!-- Ponto 2.5: Vídeo (se houver) -->
  <div class="container">
    [Vídeo iframe 16:9 ou omitir]
  </div>

  <!-- Ponto 3: Botão CTA -->
  <div class="container text-center">
    [Botão principal]
  </div>
</section>

<!-- Ponto 4: Demonstração (seção separada) -->
<section id="demonstracao" class="section--light-alt">
  ...
</section>

<!-- Ponto 5: Para Quem É (seção separada) -->
<section id="para-quem" class="section--dark-alt">
  ...
</section>
```

---

## Seções (1-5) — Detalhes

### PONTO 1: Logo + Headline + Subheadline + Urgência
- **Local**: Dentro de `<section id="hero" class="section--dark">`
- **Não há navbar/header** — Página limpa, sem distrações
- Logo (se `LOGO_URL` preenchida):
  - Centralizada no topo do hero
  - Max-width: 120px (mobile), 150px (desktop)
  - Margem: 20px top
  - **Se vazio**: omitir (não mostrar placeholder)
- H1 headline — centralizada, `titulo` font, weight 900, 48px mobile / 72px desktop, **text-align: center**
  - Margem: 20px top (se logo presente) ou 40px top (se sem logo)
- Subheadline — centralizada, `subtitulo` font, 20px / 24px, **text-align: center**
- Bloco urgência/escassez (`URGENCIA_ESCASSEZ`) se preenchido
  - Centralizado, abaixo da subheadline, cor `accent` ou destaque
  - Padding bottom: 30px (separa dos bullets)

### PONTO 2: Bullet Points (Chamada para Ação Textual)
- **Local**: Dentro de `<section id="hero">` (mesma seção do ponto 1)
- **Fundo**: herança do hero (bg_dark) — sem cor separada
- Bullet points mostrando o que a pessoa aprende/ganha
- Cada bullet = resultado prático, curiosidade ou benefício real (sem exageros)
- **Container**: `container--text` para largura restrita
- Centralizado na página, **alinhamento do texto à esquerda** dentro do container
- Objetivo: convencer sem entregar tudo
- Padding: 20px top/bottom (separa do ponto 2.5/3)

### PONTO 2.5: Vídeo de Vendas
- **Local**: Dentro de `<section id="hero">` (mesma seção, após bullets)
- **Se `VIDEO_URL` preenchida**: Aparece **APÓS os bullet points**
- **Se VIDEO_URL vazio**: omitir completamente (não mostrar seção vazia)
- Max-width: 100% (mobile), 800px (desktop)
- Centralizado com `.video-wrapper` (aspect-ratio 16:9)
- Padding: 20px top/bottom

#### Regras de Parsing da URL do Vídeo (OBRIGATÓRIO seguir):

**YouTube** — Extrair APENAS o video ID (11 caracteres), ignorar todos os query params extras:
- `https://www.youtube.com/watch?v=ABC123def45` → ID: `ABC123def45`
- `https://youtube.com/watch?v=ABC123def45&si=xxxxx&t=120` → ID: `ABC123def45` (ignorar &si, &t, etc)
- `https://youtu.be/ABC123def45` → ID: `ABC123def45` (formato curto)
- `https://youtu.be/ABC123def45?si=xxxxx` → ID: `ABC123def45` (ignorar ?si)
- `https://www.youtube.com/embed/ABC123def45` → ID: `ABC123def45` (já é embed)
- **iframe src**: `https://www.youtube.com/embed/{VIDEO_ID}`

**Vimeo** — Extrair APENAS o video ID numérico:
- `https://vimeo.com/123456789` → ID: `123456789`
- `https://vimeo.com/123456789?share=copy` → ID: `123456789` (ignorar query params)
- `https://player.vimeo.com/video/123456789` → ID: `123456789` (já é embed)
- **iframe src**: `https://player.vimeo.com/video/{VIDEO_ID}`

#### Atributos do iframe (OBRIGATÓRIO):
```html
<iframe
  src="{EMBED_URL}"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  referrerpolicy="strict-origin-when-cross-origin"
  allowfullscreen
></iframe>
```
**IMPORTANTE**: Os atributos `allow` e `referrerpolicy` são OBRIGATÓRIOS — sem eles, players modernos do YouTube/Vimeo retornam erro de configuração (Erro 153 e similares).

### PONTO 3: Botão CTA (primeiro)
- **Local**: Dentro de `<section id="hero">` (mesma seção, após vídeo ou bullets)
- Background `primary` (DESIGN_TOKENS), texto branco, verbo de ação + decorado
- Ex: "Quero [Resultado do Quadro] Agora" ou "Garantir Minha Vaga"
- Largura: auto (desktop) / 100% (mobile via CSS responsivo)
- Padding: 30px top/bottom (marca final do hero)
- **Repetido em**: seções posteriores (blocos 2, 3, 4)

---

### PONTO 4: Demonstração (nova seção separada)
- **Local**: `<section id="demonstracao" class="section--light-alt">`
- Título: "Veja como funciona na prática"
- Explicação do método (trechos da Furadeira, passo a passo ou screenshot)
- Pode incluir card visual com etapas numeradas
- Objetivo: quebrar objeção "Mas isso funciona mesmo?"

### PONTO 5: Para Quem É (nova seção separada)
- **Local**: `<section id="para-quem" class="section--dark-alt">`
- 2 colunas lado a lado:
  - Coluna 1 (verde/accent): "Isso é para você se..." (4-5 bullets de perfil/situação)
  - Coluna 2 (vermelho atenuado): "Não é para você se..." (4-5 bullets)
- Cada bullet = cenário real que o leitor reconhece em si
- Objetivo: identificação imediata, eliminar "será que funciona pra mim?"
- Se vazio: placeholders genéricos

---

## CSS Reference Completo (OBRIGATÓRIO)

**REGRA**: O CSS abaixo é a **fundação obrigatória** de toda página. Copie integralmente para o `<style>` do bloco1, substituindo apenas os valores de variáveis pelos DESIGN_TOKENS reais. NÃO improvise CSS — use exatamente estas classes e regras.

### 1. Reset + Base

```css
*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  font-family: var(--corpo-family), sans-serif;
  font-size: 16px;
  line-height: 1.6;
  color: var(--text-dark);
  overflow-x: hidden;
  background: var(--bg-light);
}

img { max-width: 100%; height: auto; display: block; }
a { color: inherit; text-decoration: none; }
```

### 2. CSS Variables (preencher com DESIGN_TOKENS)

```css
:root {
  /* Cores base — de DESIGN_TOKENS.cores */
  --primary: /* cores.primary */;
  --primary-dark: /* cores.primary_dark */;
  --primary-light: /* cores.primary_light */;
  --accent: /* cores.accent */;
  --bg-dark: /* cores.bg_dark */;
  --bg-dark-alt: /* cores.bg_dark_alt */;
  --bg-light: /* cores.bg_light */;
  --bg-light-alt: /* cores.bg_light_alt */;
  --text-dark: /* cores.text_dark */;
  --text-muted: /* cores.text_muted */;
  --text-light: /* cores.text_light */;
  --text-muted-light: /* cores.text_muted_light */;

  /* Tipografia — de DESIGN_TOKENS.tipografia */
  --titulo-family: /* tipografia.titulo.family */;
  --subtitulo-family: /* tipografia.subtitulo.family */;
  --corpo-family: /* tipografia.corpo.family */;

  /* Espaçamento — de DESIGN_TOKENS.espacamento */
  --secao-padding: /* espacamento.secao_padding */;
  --border-radius: /* espacamento.border_radius */;

  /* Sombras — 4 níveis */
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.08);
  --shadow-md: 0 4px 12px rgba(0,0,0,0.12);
  --shadow-lg: 0 8px 30px rgba(0,0,0,0.16);
  --shadow-xl: 0 16px 50px rgba(0,0,0,0.2);

  /* Transições */
  --transition-fast: 0.15s ease;
  --transition-base: 0.3s ease;
  --transition-slow: 0.5s ease;

  /* Container */
  --container-max: 1200px;
  --container-narrow: 900px;
  --container-text: 720px;
}
```

### 3. Tipografia Completa (escala responsiva)

```css
h1, h2, h3 {
  font-family: var(--titulo-family), sans-serif;
  line-height: 1.15;
  letter-spacing: -0.02em;
}

h4, h5 {
  font-family: var(--subtitulo-family), sans-serif;
  line-height: 1.3;
}

/* Mobile (base) */
h1 { font-size: 2.75rem; font-weight: 900; }       /* 44px */
h2 { font-size: 2rem; font-weight: 800; }           /* 32px */
h3 { font-size: 1.5rem; font-weight: 700; }         /* 24px */
h4 { font-size: 1.25rem; font-weight: 600; }        /* 20px */
h5 { font-size: 1.1rem; font-weight: 600; }         /* 17.6px */
p  { font-size: 1.05rem; line-height: 1.7; }        /* 16.8px */

/* Tablet (768px+) */
@media (min-width: 768px) {
  h1 { font-size: 3.5rem; }     /* 56px */
  h2 { font-size: 2.5rem; }     /* 40px */
  h3 { font-size: 1.75rem; }    /* 28px */
  h4 { font-size: 1.35rem; }    /* 21.6px */
  p  { font-size: 1.1rem; }     /* 17.6px */
}

/* Desktop (1024px+) */
@media (min-width: 1024px) {
  h1 { font-size: 4.5rem; }     /* 72px */
  h2 { font-size: 3rem; }       /* 48px */
  h3 { font-size: 2rem; }       /* 32px */
  h4 { font-size: 1.5rem; }     /* 24px */
  p  { font-size: 1.15rem; }    /* 18.4px */
}
```

### 4. Layout / Container

```css
.container {
  width: 100%;
  max-width: var(--container-max);
  margin: 0 auto;
  padding: 0 20px;
}

@media (min-width: 768px) {
  .container { padding: 0 40px; }
}

@media (min-width: 1024px) {
  .container { padding: 0 60px; }
}

.container--narrow { max-width: var(--container-narrow); }
.container--text { max-width: var(--container-text); }
```

### 5. Section Base (padding responsivo)

```css
section {
  padding: 60px 0;
}

@media (min-width: 768px) {
  section { padding: calc(var(--secao-padding) * 0.8) 0; }
}

@media (min-width: 1024px) {
  section { padding: var(--secao-padding) 0; }
}
```

### 6. Grids Responsivos

```css
.grid-2 {
  display: grid;
  grid-template-columns: 1fr;
  gap: 30px;
}

.grid-3 {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24px;
}

@media (min-width: 768px) {
  .grid-2 { grid-template-columns: 1fr 1fr; gap: 40px; }
  .grid-3 { grid-template-columns: repeat(2, 1fr); gap: 30px; }
}

@media (min-width: 1024px) {
  .grid-3 { grid-template-columns: repeat(3, 1fr); }
}
```

### 7. Botões por `DESIGN_TOKENS.elementos.botao_estilo`

**REGRA**: Usar EXATAMENTE o CSS do estilo correspondente ao `botao_estilo` do DESIGN_TOKENS.

```css
/* === SOLIDO === */
.btn-cta--solido {
  display: inline-block;
  background: var(--primary);
  color: #fff;
  border: none;
  padding: 18px 48px;
  font-family: var(--titulo-family), sans-serif;
  font-size: 1.15rem;
  font-weight: 700;
  border-radius: var(--border-radius);
  cursor: pointer;
  transition: background var(--transition-base), transform var(--transition-fast), box-shadow var(--transition-base);
  text-align: center;
}
.btn-cta--solido:hover {
  background: var(--primary-dark);
  transform: translateY(-2px);
  box-shadow: var(--shadow-lg);
}
.btn-cta--solido:active { transform: translateY(0); }

/* === GRADIENTE === */
.btn-cta--gradiente {
  display: inline-block;
  background: linear-gradient(135deg, var(--primary), var(--primary-dark));
  color: #fff;
  border: none;
  padding: 18px 48px;
  font-family: var(--titulo-family), sans-serif;
  font-size: 1.15rem;
  font-weight: 700;
  border-radius: var(--border-radius);
  cursor: pointer;
  transition: transform var(--transition-fast), box-shadow var(--transition-base);
  text-align: center;
}
.btn-cta--gradiente:hover {
  background: linear-gradient(315deg, var(--primary), var(--primary-dark));
  transform: translateY(-2px);
  box-shadow: var(--shadow-lg);
}
.btn-cta--gradiente:active { transform: translateY(0); }

/* === OUTLINE === */
.btn-cta--outline {
  display: inline-block;
  background: transparent;
  color: var(--primary);
  border: 2px solid var(--primary);
  padding: 16px 46px;
  font-family: var(--titulo-family), sans-serif;
  font-size: 1.15rem;
  font-weight: 700;
  border-radius: var(--border-radius);
  cursor: pointer;
  transition: background var(--transition-base), color var(--transition-base), transform var(--transition-fast);
  text-align: center;
}
.btn-cta--outline:hover {
  background: var(--primary);
  color: #fff;
  transform: translateY(-2px);
}
.btn-cta--outline:active { transform: translateY(0); }

/* === ARREDONDADO (pill) === */
.btn-cta--arredondado {
  display: inline-block;
  background: var(--primary);
  color: #fff;
  border: none;
  padding: 18px 52px;
  font-family: var(--titulo-family), sans-serif;
  font-size: 1.15rem;
  font-weight: 700;
  border-radius: 50px;
  cursor: pointer;
  transition: box-shadow var(--transition-base), transform var(--transition-fast);
  text-align: center;
}
.btn-cta--arredondado:hover {
  box-shadow: 0 6px 25px rgba(0,0,0,0.2);
  transform: translateY(-2px);
}
.btn-cta--arredondado:active { transform: translateY(0); }

/* Classe genérica — usar a classe específica baseada em DESIGN_TOKENS.elementos.botao_estilo */
/* Ex: se botao_estilo = "gradiente", usar .btn-cta--gradiente em todos os botões CTA */

/* Mobile: botão full-width */
@media (max-width: 767px) {
  [class*="btn-cta"] {
    display: block;
    width: 100%;
    text-align: center;
  }
}
```

### 8. Cards por `DESIGN_TOKENS.elementos.cards_estilo`

**REGRA**: Usar EXATAMENTE o CSS do estilo correspondente ao `cards_estilo` do DESIGN_TOKENS.

```css
/* === FLAT === */
.card--flat {
  background: var(--bg-light);
  border: 1px solid rgba(0,0,0,0.08);
  border-radius: var(--border-radius);
  padding: 32px 28px;
  transition: border-color var(--transition-base);
}
.card--flat:hover { border-color: var(--primary); }

/* === ELEVADO === */
.card--elevado {
  background: var(--bg-light);
  border-radius: var(--border-radius);
  padding: 32px 28px;
  box-shadow: var(--shadow-sm);
  transition: box-shadow var(--transition-base), transform var(--transition-fast);
}
.card--elevado:hover {
  box-shadow: var(--shadow-lg);
  transform: translateY(-4px);
}

/* === BORDA-ESQUERDA === */
.card--borda-esquerda {
  background: var(--bg-light);
  border-left: 4px solid var(--primary);
  border-radius: 0 var(--border-radius) var(--border-radius) 0;
  padding: 32px 28px;
  transition: background var(--transition-base);
}
.card--borda-esquerda:hover {
  background: var(--primary-light);
}

/* === GLASSMORPHISM === */
.card--glassmorphism {
  background: rgba(255,255,255,0.08);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255,255,255,0.15);
  border-radius: var(--border-radius);
  padding: 32px 28px;
  transition: background var(--transition-base), border-color var(--transition-base);
}
.card--glassmorphism:hover {
  background: rgba(255,255,255,0.14);
  border-color: rgba(255,255,255,0.25);
}

/* Card em fundo escuro — inversão de cores */
.section--dark .card--flat,
.section--dark .card--elevado {
  background: var(--bg-dark-alt);
  color: var(--text-light);
  border-color: rgba(255,255,255,0.1);
}
.section--dark .card--borda-esquerda {
  background: var(--bg-dark-alt);
  color: var(--text-light);
}
```

### 9. Separadores por `DESIGN_TOKENS.elementos.separador`

```css
/* === LINHA === */
.separator--linha {
  border: none;
  border-bottom: 1px solid rgba(0,0,0,0.1);
  margin: 0;
}
.section--dark .separator--linha {
  border-bottom-color: rgba(255,255,255,0.1);
}

/* === GRADIENTE === */
.separator--gradiente {
  border: none;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--primary), transparent);
  margin: 0;
}

/* === ONDULADO (SVG inline) === */
.separator--ondulado {
  display: block;
  width: 100%;
  line-height: 0;
  margin: 0;
  overflow: hidden;
}
.separator--ondulado svg {
  display: block;
  width: 100%;
  height: 40px;
}

/* === NENHUM === */
.separator--nenhum { display: none; }
```

**Nota sobre ondulado**: Inserir entre seções como:
```html
<div class="separator--ondulado">
  <svg viewBox="0 0 1200 40" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M0,20 C200,40 400,0 600,20 C800,40 1000,0 1200,20 L1200,40 L0,40 Z" fill="var(--bg-light)"/>
  </svg>
</div>
```
(Ajustar `fill` conforme a seção seguinte.)

### 10. Fundos Variados para Seções

**REGRA**: NÃO alternar monotonamente bg_dark/bg_light. Usar variações para criar ritmo visual.

```css
/* Escuros */
.section--dark {
  background: var(--bg-dark);
  color: var(--text-light);
}
.section--dark-alt {
  background: var(--bg-dark-alt);
  color: var(--text-light);
}
.section--dark-gradient {
  background: linear-gradient(180deg, var(--bg-dark), var(--bg-dark-alt));
  color: var(--text-light);
}

/* Claros */
.section--light {
  background: var(--bg-light);
  color: var(--text-dark);
}
.section--light-alt {
  background: var(--bg-light-alt);
  color: var(--text-dark);
}
.section--light-gradient {
  background: linear-gradient(180deg, var(--bg-light), var(--bg-light-alt));
  color: var(--text-dark);
}

/* Especial: tint da cor primária (para seções de destaque) */
.section--primary-tint {
  background: var(--primary-light);
  color: var(--text-dark);
}
```

**Sugestão de distribuição**:

| Seção | Background Sugerido |
|-------|---------------------|
| 1. Hero | `section--dark` |
| 2. CTA Text | `section--light` |
| 3. Botão CTA | `section--primary-tint` |
| 4. Demonstração | `section--light-alt` |
| 5. Para Quem É | `section--dark-alt` |
| 6. Formato | `section--light` |
| 7. Oferta + Preço | `section--dark-gradient` |
| 8. Depoimentos | `section--light` |
| 9. Paliativo | `section--primary-tint` |
| 10. Autoridade | `section--dark` |
| 11. Objeções | `section--light-alt` |
| 12. FAQ | `section--light` |
| 13. Suporte | `section--dark-alt` |
| 14. Argumentos | `section--dark-gradient` |
| 15. Dores/Desejos | `section--light` |
| 16. Sobre Autor | `section--dark` |
| 17. Comparação | `section--light-alt` |

### 11. Padrões CSS Humanizados

**REGRA**: Estes são os elementos que fazem a página NÃO parecer IA. Usar ativamente nas seções.

```css
/* Highlight — underline parcial para palavras-chave */
.highlight {
  position: relative;
  display: inline;
  z-index: 1;
}
.highlight::after {
  content: '';
  position: absolute;
  bottom: 2px;
  left: -2px;
  right: -2px;
  height: 35%;
  background: var(--primary-light);
  z-index: -1;
  border-radius: 2px;
}
.section--dark .highlight::after {
  background: rgba(255,255,255,0.15);
}

/* Citação estilizada */
.quote {
  border-left: 4px solid var(--accent);
  padding: 20px 24px;
  margin: 24px 0;
  font-family: var(--subtitulo-family), sans-serif;
  font-style: italic;
  font-size: 1.15rem;
  color: var(--text-muted);
  background: rgba(0,0,0,0.02);
  border-radius: 0 var(--border-radius) var(--border-radius) 0;
}
.section--dark .quote {
  color: var(--text-muted-light);
  background: rgba(255,255,255,0.04);
}

/* Benefit list — bullets customizados */
.benefit-list {
  list-style: none;
  padding: 0;
}
.benefit-list li {
  position: relative;
  padding: 10px 0 10px 36px;
  font-size: 1.05rem;
  line-height: 1.6;
}
.benefit-list li::before {
  content: '✓';
  position: absolute;
  left: 0;
  top: 10px;
  width: 24px;
  height: 24px;
  background: var(--primary);
  color: #fff;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.75rem;
  font-weight: 700;
}

/* Badge / etiqueta */
.badge {
  display: inline-block;
  background: var(--primary);
  color: #fff;
  padding: 4px 14px;
  border-radius: 50px;
  font-size: 0.8rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}
.badge--accent { background: var(--accent); }
.badge--muted  { background: var(--text-muted); }

/* Stat number — números de destaque (seção 14 Argumentos Incontestáveis) */
.stat-number {
  font-family: var(--titulo-family), sans-serif;
  font-size: 4.5rem;
  font-weight: 900;
  color: var(--primary);
  line-height: 1;
  letter-spacing: -0.03em;
}
@media (min-width: 768px)  { .stat-number { font-size: 6rem; } }
@media (min-width: 1024px) { .stat-number { font-size: 7.5rem; } }

/* Urgency bar — barra de urgência/escassez */
.urgency-bar {
  display: inline-block;
  background: var(--accent);
  color: #fff;
  padding: 10px 24px;
  border-radius: var(--border-radius);
  font-family: var(--subtitulo-family), sans-serif;
  font-size: 0.95rem;
  font-weight: 600;
  letter-spacing: 0.02em;
  text-align: center;
}

/* Video wrapper — 16:9 responsivo */
.video-wrapper {
  position: relative;
  padding-bottom: 56.25%; /* 16:9 */
  height: 0;
  overflow: hidden;
  border-radius: var(--border-radius);
  box-shadow: var(--shadow-lg);
  max-width: 800px;
  margin: 0 auto;
}
.video-wrapper iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border: none;
}

/* Accordion — FAQ (JS em bloco3) */
.accordion-item {
  border-bottom: 1px solid rgba(0,0,0,0.08);
}
.section--dark .accordion-item {
  border-bottom-color: rgba(255,255,255,0.1);
}
.accordion-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  cursor: pointer;
  font-family: var(--subtitulo-family), sans-serif;
  font-weight: 600;
  font-size: 1.1rem;
  background: none;
  border: none;
  width: 100%;
  text-align: left;
  color: inherit;
  transition: color var(--transition-fast);
}
.accordion-header:hover { color: var(--primary); }
.accordion-icon {
  font-size: 1.4rem;
  font-weight: 300;
  transition: transform var(--transition-base);
  flex-shrink: 0;
  margin-left: 16px;
}
.accordion-item.active .accordion-icon {
  transform: rotate(45deg);
}
.accordion-body {
  max-height: 0;
  overflow: hidden;
  transition: max-height var(--transition-slow);
}
.accordion-body-inner {
  padding: 0 0 20px;
  font-size: 1.05rem;
  line-height: 1.7;
  color: var(--text-muted);
}
.section--dark .accordion-body-inner {
  color: var(--text-muted-light);
}

/* Comparison table */
.comparison-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  border-radius: var(--border-radius);
  overflow: hidden;
  box-shadow: var(--shadow-sm);
}
.comparison-table th,
.comparison-table td {
  padding: 16px 20px;
  text-align: left;
  font-size: 1rem;
}
.comparison-table thead th {
  background: var(--primary);
  color: #fff;
  font-family: var(--subtitulo-family), sans-serif;
  font-weight: 600;
}
.comparison-table tbody tr:nth-child(even) {
  background: rgba(0,0,0,0.03);
}
.comparison-table .check { color: var(--primary); font-weight: 700; font-size: 1.2rem; }
.comparison-table .cross { color: var(--text-muted); font-size: 1.2rem; }

/* Texto centralizado padrão para headlines */
.text-center { text-align: center; }
.text-left { text-align: left; }
.mx-auto { margin-left: auto; margin-right: auto; }
.mb-sm { margin-bottom: 12px; }
.mb-md { margin-bottom: 24px; }
.mb-lg { margin-bottom: 40px; }
.mb-xl { margin-bottom: 60px; }
```

---

## Extras

### SEM Sticky CTA Button
- **NUNCA** floating buttons no rodapé ou cantos da tela
- **NUNCA** botões flutuantes fixos (fixed/sticky)
- Página limpa, sem elementos distratores
- Botões CTA aparecem naturalmente no fluxo da página (seções 3, 7, 13, 17)

### Scroll to anchor
- Scroll behavior suave já incluído no reset (`scroll-behavior: smooth`)

---

## Placeholders (quando faltar info)

| Campo | Placeholder |
|-------|------------|
| headline | `[INSERIR HEADLINE PRINCIPAL]` |
| subheadline | `[INSERIR SUBHEADLINE]` |
| bullets_cta[] | 3 bullets genéricos |
| demonstracao | `[INSERIR DEMONSTRAÇÃO]` |
| para_quem.e_para[] | 3 bullets genéricos |
| para_quem.nao_e_para[] | 3 bullets genéricos |
| VIDEO_URL | (omitir vídeo) |
| PRODUTO_IMAGEM_URL | Placeholder genérico |
| URGENCIA_ESCASSEZ | (omitir se vazio) |
| LOGO_URL | (omitir se vazio) |
| LINK_CHECKOUT | `href="#"` + `[INSERIR LINK DE CHECKOUT]` |

---

## Validações

- ✅ 5 seções presentes e em ordem
- ✅ **TODO o CSS Reference** incluso no `<style>` (reset, variáveis, tipografia, grids, botões, cards, separadores, fundos, padrões humanizados)
- ✅ Classe correta de botão usada (conforme `DESIGN_TOKENS.elementos.botao_estilo`)
- ✅ Classe correta de card usada (conforme `DESIGN_TOKENS.elementos.cards_estilo`)
- ✅ Separador correto entre seções (conforme `DESIGN_TOKENS.elementos.separador`)
- ✅ Fundos variados (NÃO monotonamente bg_dark/bg_light)
- ✅ Tipografia responsiva (mobile → tablet → desktop para h1-h5 e p)
- ✅ Container com padding responsivo
- ✅ Cores aplicadas via variáveis CSS
- ✅ Responsive (mobile/tablet/desktop)
- ✅ Placeholders para campos faltando
- ✅ HTML válido (sem erros de sintaxe)
- ✅ Self-contained (tudo em `<style>`)
- ✅ Não fecha `</body></html>` (será adicionado pelo bloco4)

---

## Estrutura HTML mínima (BLOCO 1)

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[NOME_PRODUTO]</title>
  <link href="[DESIGN_TOKENS.tipografia.google_fonts_url]" rel="stylesheet">
  <style>
    /* ===========================
       1. RESET + BASE
       =========================== */
    /* (copiar seção 1 do CSS Reference) */

    /* ===========================
       2. CSS VARIABLES
       =========================== */
    :root {
      --primary: #VALOR;
      --primary-dark: #VALOR;
      --primary-light: #VALOR;
      /* ... (preencher com DESIGN_TOKENS reais) */
    }

    /* ===========================
       3. TIPOGRAFIA
       =========================== */
    /* (copiar seção 3 do CSS Reference) */

    /* ===========================
       4. LAYOUT / CONTAINER
       =========================== */
    /* (copiar seção 4) */

    /* ===========================
       5. SECTION BASE
       =========================== */
    /* (copiar seção 5) */

    /* ===========================
       6. GRIDS
       =========================== */
    /* (copiar seção 6) */

    /* ===========================
       7. BOTÕES (usar estilo do DESIGN_TOKENS)
       =========================== */
    /* (copiar APENAS o estilo correspondente ao botao_estilo + mobile rule) */

    /* ===========================
       8. CARDS (usar estilo do DESIGN_TOKENS)
       =========================== */
    /* (copiar APENAS o estilo correspondente ao cards_estilo + dark variants) */

    /* ===========================
       9. SEPARADORES (usar estilo do DESIGN_TOKENS)
       =========================== */
    /* (copiar APENAS o estilo correspondente ao separador) */

    /* ===========================
       10. FUNDOS DE SEÇÃO
       =========================== */
    /* (copiar seção 10 — todos os backgrounds) */

    /* ===========================
       11. PADRÕES HUMANIZADOS
       =========================== */
    /* (copiar seção 11 — highlight, quote, benefit-list, badge, stat-number, urgency-bar, video-wrapper, accordion, comparison-table, utilitários) */
  </style>
</head>
<body>

  <!-- SEÇÃO HERO (Pontos 1-3: Logo + Headline + Bullets + Vídeo + Botão) -->
  <section id="hero" class="section--dark">
    <!-- Ponto 1: Logo + Headline + Subheadline + Urgência -->
    <div class="container text-center">
      <!-- Logo centralizada (se LOGO_URL preenchido) -->
      <!-- <img src="LOGO_URL" alt="Logo" style="max-width:120px; margin:0 auto 20px;"> -->

      <h1>[HEADLINE do copy-estruturada.json]</h1>
      <p class="mb-md" style="font-family:var(--subtitulo-family);font-size:1.25rem;opacity:0.9;">
        [SUBHEADLINE]
      </p>

      <!-- Urgência (se URGENCIA_ESCASSEZ preenchido) -->
      <!-- <span class="urgency-bar" style="margin-top:20px;">[URGENCIA_ESCASSEZ]</span> -->
    </div>

    <!-- Ponto 2: Bullet points (CTA Text) -->
    <div class="container container--text" style="padding-top:20px; padding-bottom:20px;">
      <ul class="benefit-list">
        <li>[Bullet 1 — resultado prático]</li>
        <li>[Bullet 2 — curiosidade]</li>
        <li>[Bullet 3 — benefício real]</li>
      </ul>
    </div>

    <!-- Ponto 2.5: Vídeo (se VIDEO_URL preenchido — omitir bloco inteiro se vazio) -->
    <!-- Exemplo YouTube: VIDEO_URL="https://youtu.be/dQw4w9WgXcQ?si=abc" → src="https://www.youtube.com/embed/dQw4w9WgXcQ" -->
    <!-- Exemplo Vimeo:   VIDEO_URL="https://vimeo.com/123456789"         → src="https://player.vimeo.com/video/123456789" -->
    <div class="container" style="padding-top:20px; padding-bottom:20px;">
      <div class="video-wrapper">
        <iframe src="https://www.youtube.com/embed/[VIDEO_ID]" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
      </div>
    </div>

    <!-- Ponto 3: Botão CTA -->
    <div class="container text-center" style="padding-top:30px; padding-bottom:30px;">
      <h3 class="mb-md">[Chamada para ação]</h3>
      <a href="[LINK_CHECKOUT]" class="btn-cta--[BOTAO_ESTILO]">
        [TEXTO DO BOTÃO]
      </a>
    </div>
  </section>

  <!-- Seção 4: Demonstração -->
  <section id="demonstracao" class="section--light-alt">
    <div class="container container--narrow">
      <h2 class="text-center mb-lg">Veja como funciona na prática</h2>
      <!-- Cards com etapas numeradas (usar .card--[CARDS_ESTILO]) -->
      <div class="grid-3">
        <div class="card--[CARDS_ESTILO] text-center">
          <span class="badge mb-sm">Passo 1</span>
          <h4>[Etapa 1]</h4>
          <p>[Descrição]</p>
        </div>
        <!-- ... -->
      </div>
    </div>
  </section>

  <!-- Seção 5: Para Quem É / Não É -->
  <section id="para-quem" class="section--dark-alt">
    <div class="container">
      <h2 class="text-center mb-lg">Para quem é este produto?</h2>
      <div class="grid-2">
        <div>
          <h3 style="color:var(--accent);" class="mb-md">✓ Isso é para você se...</h3>
          <ul class="benefit-list">
            <li>[Perfil 1]</li>
            <!-- ... -->
          </ul>
        </div>
        <div>
          <h3 style="color:#EF4444;" class="mb-md">✗ Não é para você se...</h3>
          <ul class="benefit-list" style="--bullet-color:#EF4444;">
            <li>[Anti-perfil 1]</li>
            <!-- ... -->
          </ul>
        </div>
      </div>
    </div>
  </section>

<!-- NÃO fechar </body></html> — será adicionado pelo bloco4 -->
```

---

## Procedimento

1. **Leia os 3 arquivos de entrada** (config-pagina.json, design-tokens.json, copy-estruturada.json)
2. **Parse de DESIGN_TOKENS** — Extraia cores, tipografia, espaçamento
3. **Crie variáveis CSS** — Defina `--primary`, `--titulo-family`, `--border-radius`, etc
4. **Parse de VIDEO_URL** — Se preenchida (não vazia):
   - Se contém `youtube.com/watch?v=`: extrair valor do param `v` (ignorar outros params como &si, &t)
   - Se contém `youtu.be/`: extrair ID após a barra (ignorar query params como ?si=)
   - Se contém `youtube.com/embed/`: extrair ID após `/embed/`
   - Se contém `vimeo.com/` ou `player.vimeo.com/video/`: extrair ID numérico
   - Montar iframe src: YouTube → `https://www.youtube.com/embed/{ID}` | Vimeo → `https://player.vimeo.com/video/{ID}`
   - **OBRIGATÓRIO**: incluir atributo `allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"` e `frameborder="0"` no iframe
   - Se VIDEO_URL vazia: **omitir o bloco de vídeo inteiro** (não renderizar div vazia)
5. **Gere o HTML das 5 seções** em ordem
6. **Substitua placeholders** — Use valores do JSON ou defaults
7. **Aplique DESIGN_TOKENS** — cores, fontes, espaçamento
8. **Inline CSS + JS** — Tudo em `<style>` e `<script>`
9. **Salve em** `output/bloco1.html` (sem fechar </body></html>)
10. **Retorne confirmação** — Arquivo criado, pronto para bloco2

---

## ⚠️ Restrições

- ❌ Não use CDNs externas (exceto Google Fonts)
- ❌ Não use JavaScript frameworks (jQuery, Vue, React)
- ❌ Não invente informações não presentes no JSON
- ❌ **NÃO override** DESIGN_TOKENS com valores hardcoded
- ❌ **NÃO feche** `</body></html>` (será adicionado pelo bloco4)
- ✅ DESIGN_TOKENS é fonte de verdade para cores, fontes, espaçamento
- ✅ CSS Flexbox/Grid para layout
- ✅ HTML semântico (nav, section, article, footer)
- ✅ Meta tags essenciais (viewport, charset, title)
- ✅ Aria labels para acessibilidade
- ✅ Use `var(--primary)`, `var(--titulo-family)`, etc nas regras CSS
