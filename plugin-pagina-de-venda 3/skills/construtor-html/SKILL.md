---
name: construtor-html
description: "⚠️ DEPRECATED — Substituído por construtor-bloco1, bloco2, bloco3, bloco4."
---

> ## ⚠️ DEPRECATED — NÃO USAR
>
> Este skill foi substituído pelos 4 construtores de bloco:
> - `construtor-bloco1` — Seções 1-5 (Hero/Persuasão) + `<head>`, `<style>`, CSS completo
> - `construtor-bloco2` — Seções 6-9 (Oferta/Detalhes)
> - `construtor-bloco3` — Seções 10-13 (Credibilidade) + Accordion JS
> - `construtor-bloco4` — Seções 14-17 (Emoção/Lógica) + fechamento `</body></html>`
>
> O fluxo atual em `commands/criar-pagina.md` usa os 4 blocos sequenciais (Passos 5a-5d)
> e depois concatena em `output/pagina-de-venda.html` (Passo 6).
>
> **Problemas deste skill antigo:**
> - Navbar hardcoded (o fluxo atual NÃO usa navbar)
> - Montserrat + Open Sans hardcoded (deve usar DESIGN_TOKENS)
> - Sticky CTA button (removido no fluxo atual)
> - Não implementa `DESIGN_TOKENS.elementos` (separador, cards_estilo, botao_estilo)
> - CSS genérico que produz páginas com cara de IA
>
> Mantido apenas como referência histórica.

# Skill: Construtor de HTML (DEPRECATED)

## Missão

Receber um JSON estruturado + 5 respostas do mentorado e gerar um arquivo HTML completo, profissional e pronto para publicar.

---

## Entrada (INPUT)

- `JSON_ESTRUTURADO` — JSON do agente extrator-copy
- `DESIGN_TOKENS` — JSON de design da skill designer-html (cores, tipografia, espaçamento, etc)
- `COR_PRINCIPAL` — Cor hex (ex: `#FF6B00`) — **SERÁ SUBSTITUÍDA pela cor em DESIGN_TOKENS**
- `NOME_PRODUTO` — Nome para `<title>`
- `PRECO_PRODUTO` — Preço (ex: `R$ 297`)
- `VIDEO_URL` — URL do vídeo de vendas (YouTube ou Vimeo) ou vazio
- `LINK_CHECKOUT` — URL de compra
- `URGENCIA_ESCASSEZ` — Texto de urgência
- `TIPO_DEPOIMENTOS` — `"texto"` ou `"video"`
- `LOGO_URL` — URL externa da logo (ex: `https://cdn.cloudron.com/logo.png`) ou vazio
- `EXPERT_FOTO_URL` — URL externa da foto do expert (ex: `https://cdn.cloudron.com/expert.jpg`) ou vazio
- `PRODUTO_IMAGEM_URL` — URL da imagem do produto (fallback se VIDEO_URL vazio) ou vazio

---

## Saída (OUTPUT)

Arquivo HTML salvo em: **`output/pagina-de-venda.html`**

---

## 17 Seções obrigatórias (4 Blocos) — ESTRUTURA COMPLETA

---

### BLOCO 1: HERO / PERSUASÃO INICIAL

### 1. HERO — Cabeçalho
- **Fundo**: `bg_dark` (DESIGN_TOKENS)
- **Não há navbar/header** — Página limpa, sem distrações
- H1 headline — centralizada, `titulo` font, weight 900, 48px mobile / 72px desktop, **text-align: center**
- Subheadline — centralizada, `subtitulo` font, 20px / 24px, **text-align: center**
- **VÍDEO DE VENDAS** (se `VIDEO_URL` preenchida):
  - Aparece **imediatamente abaixo da subheadline**
  - Se YouTube: extrair video ID e renderizar como iframe 16:9
    - Exemplo URL: `https://youtube.com/watch?v=ABC123` → iframe src: `https://www.youtube.com/embed/ABC123`
  - Se Vimeo: renderizar iframe Vimeo 16:9
    - Exemplo URL: `https://vimeo.com/123456789` → iframe src: `https://player.vimeo.com/video/123456789`
  - Max-width: 100% (mobile), 800px (desktop)
  - Margem: 40px top/bottom
  - **Se VIDEO_URL vazio**: mostrar `PRODUTO_IMAGEM_URL` ou placeholder genérico (não imagem de vídeo)
- Bloco urgência/escassez (`URGENCIA_ESCASSEZ`) se preenchido
- (Abaixo do vídeo/imagem) Imagem placeholder do produto

### 2. CHAMADA PARA AÇÃO TEXTUAL
- **Fundo**: contraste suave (variação de `bg_light`)
- Bullet points mostrando o que a pessoa aprende/ganha assistindo ao vídeo
- Cada bullet = resultado prático, curiosidade ou benefício real (sem exageros)
- Centralizado na página, **alinhamento do texto à esquerda** dentro do container
- Objetivo: convencer a assistir ao vídeo sem entregar tudo

### 3. BOTÃO CTA (primeiro, acima do fold)
- Background `primary` (DESIGN_TOKENS), texto branco, verbo de ação + decorado
- Ex: "Quero [Resultado do Quadro] Agora" ou "Garantir Minha Vaga"
- Largura: auto (desktop) / 100% (mobile)
- Repetido nas seções 7, 13 e 17

### 4. DEMONSTRAÇÃO
- **Fundo**: `bg_light`
- Título: "Veja como funciona na prática"
- Explicação do método (trechos da Furadeira, passo a passo ou screenshot)
- Pode incluir card visual com etapas numeradas
- Objetivo: quebrar objeção "Mas isso funciona mesmo?"

### 5. PARA QUEM É
- **Fundo**: `bg_dark`
- 2 colunas lado a lado:
  - Coluna 1 (verde/accent): "Isso é para você se..." (4-5 bullets de perfil/situação)
  - Coluna 2 (vermelho atenuado): "Não é para você se..." (4-5 bullets)
- Cada bullet = cenário real que o leitor reconhece em si
- Objetivo: identificação imediata, eliminar "será que funciona pra mim?"
- Se vazio: placeholders genéricos

---

### BLOCO 2: OFERTA / DETALHES

### 6. FORMATO DO PRODUTO
- **Fundo**: `bg_light`
- Ícone visual representando o tipo (curso, comunidade, mentoria, acervo)
- Explicação de como funciona e como é entregue (plataforma, lives, acesso imediato)
- Detalhe técnico: duração, módulos, frequência de aulas, suporte incluso

### 7. OFERTA COMPLETA + PREÇO + GARANTIA
- **Fundo**: gradiente escuro (`bg_dark` → variação)
- Lista de entregáveis com valor percebido individual
- Rodapé: "Valor total: R$ X — Hoje por apenas R$ Y"
- Preço em destaque (`titulo` font 900, 72px+, `primary`)
- Preço antigo riscado (se disponível no JSON)
- Bloco garantia: ícone de cadeado + texto direto, sem linguagem jurídica
- Botão CTA (repetição da seção 3)
- Countdown timer (bloco comentado, fácil de ativar)

### 8. PROVAS / DEPOIMENTOS
- **Fundo**: `bg_light`
- Grid 3 colunas (1 mobile)
- Se `tipo_depoimentos = "texto"`: cards com aspas, nome, função, resultado
- Se `tipo_depoimentos = "video"`: iframes YouTube/Vimeo (16:9)
- Provas alinhadas com o **Quadro** (resultado) e **Decorados** (benefícios emocionais)
- Se vazio: 3 placeholders genéricos

### 9. PALIATIVO (Entregável Rápido)
- **Fundo**: destaque com gradiente ou borda `primary` (3-5px)
- Card central proeminente
- Título: "Acesso imediato:" ou "Você também recebe:"
- Descrição clara do paliativo (ex: "Checklist prático com X passos")
- CTA secundário de acesso/download
- Objetivo: ganho rápido que reforça a decisão de compra

---

### BLOCO 3: CREDIBILIDADE

### 10. AUTORIDADE DO EXPERT
- **Fundo**: `bg_dark`
- 2 colunas: foto (esquerda) + bio e credenciais (direita)
- Foto: `EXPERT_FOTO_URL` ou placeholder cinza com ícone
- Nome (titulo font 700, 32px), bio, lista de credenciais
- Resultado destaque em card (fundo `accent`)
- Mini jornada de herói: vulnerabilidade + conquista (quando disponível)

### 11. OBJEÇÕES E RESPOSTAS
- **Fundo**: `bg_light`
- Lista de 3-5 objeções comuns com resposta empática e direta
- Formato: pergunta em destaque (bold) + resposta em parágrafo
- Foco: dúvidas que bloqueiam a compra (não FAQ técnico)

### 12. FAQ — PERGUNTAS FREQUENTES
- **Fundo**: `bg_light`
- Accordion CSS+JS puro (sem jQuery), ícone +/−
- Dúvidas operacionais, técnicas e de acesso
- Se vazio: 5 FAQs padrão (garantia, acesso, parcelamento, suporte, prazo)

### 13. SUPORTE
- **Fundo**: `bg_dark`
- Canais de atendimento (WhatsApp, e-mail, comunidade)
- Tempo de resposta e horários
- Mensagem humanizada: "Você não estará sozinho(a)"
- Botão CTA (repetição da seção 3)

---

### BLOCO 4: EMOÇÃO / LÓGICA

### 14. ARGUMENTOS INCONTESTÁVEIS
- **Fundo**: `bg_dark` ou gradiente
- Número/estatística central em destaque (titulo font 900, 96px+, cor `primary`)
- Texto de suporte (18px+)
- Fonte citada em subtítulo cinza (pesquisa, dado, estudo)
- Exemplos: "97% dos alunos..." / "2.500+ vidas transformadas" / "Comprovado por..."
- Objetivo: desarmar ceticismo com fatos objetivos

### 15. DORES E DESEJOS
- **Fundo**: `bg_light`
- 2 colunas ou seção dividida:
  - **Dores** (hoje): o que a pessoa sente/vive atualmente
  - **Desejos** (depois): o estado que quer alcançar após o produto
- Linguagem visceral, emocional, de identificação profunda
- Objetivo: conexão emocional antes do CTA final

### 16. SOBRE O AUTOR
- **Fundo**: `bg_dark`
- Diferente da seção 10 (Autoridade): foco na **história pessoal e emocional**
- Mini jornada de herói: ponto de virada + vulnerabilidade + conquista
- Foto do autor em contexto humanizado (não pose formal)
- "Por que eu criei este produto" — motivação real

### 17. COMPARAÇÃO COM OUTRAS SOLUÇÕES
- **Fundo**: `bg_light`
- Tabela comparativa ou dois blocos "Com [produto]" vs. "Sem [produto]"
- Colunas: outros métodos (✗) vs. seu produto (✓)
- Visual claro e organizado, sem denegrir concorrentes
- Objetivo: mostrar diferenciação de forma objetiva
- Botão CTA final (repetição da seção 3, última aparição)

---

## Design System

### Tipografia (de DESIGN_TOKENS)

O construtor-html **DEVE usar** os valores de `DESIGN_TOKENS.tipografia.google_fonts_url` para importar as fonts corretas.

**Exemplo com DESIGN_TOKENS**:
```json
{
  "google_fonts_url": "https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800;900&family=DM+Sans:wght@400;500;600&display=swap",
  "titulo": { "family": "Inter", "weights": "700,800,900" },
  "subtitulo": { "family": "DM Sans", "weights": "400,600" },
  "corpo": { "family": "DM Sans", "weights": "400,500" }
}
```

**CSS gerado dinamicamente**:
- `h1, h2, h3` → usar `DESIGN_TOKENS.tipografia.titulo.family` + weights
- `h4, h5` → usar `DESIGN_TOKENS.tipografia.subtitulo.family` + weights
- `body, p, span` → usar `DESIGN_TOKENS.tipografia.corpo.family` + weights

### Cores (de DESIGN_TOKENS)

O construtor-html **DEVE usar** DESIGN_TOKENS como **fonte de verdade**:

```json
{
  "primary": "#FF6B00",      // Botões CTA
  "accent": "#FF8C00",       // Detalhes, bordas, ícones
  "bg_dark": "#0D0D0D",      // Seções escuras (hero, CTA final)
  "bg_light": "#F7F7F7",     // Seções claras (benefícios, depoimentos)
  "text_dark": "#222222",    // Texto em fundos claros
  "text_light": "#FFFFFF"    // Texto em fundos escuros
}
```

**Aplicação**:
- Botões CTA → `DESIGN_TOKENS.cores.primary`
- Ícones/detalhes → `DESIGN_TOKENS.cores.accent`
- Seções hero, footer → `DESIGN_TOKENS.cores.bg_dark`
- Seções alternadas → `DESIGN_TOKENS.cores.bg_light`
- Texto em claro → `DESIGN_TOKENS.cores.text_dark`
- Texto em escuro → `DESIGN_TOKENS.cores.text_light`

### Botões (usando DESIGN_TOKENS)

```css
.btn-cta {
  background-color: var(--primary);  /* De DESIGN_TOKENS.cores.primary */
  color: white;
  border: none;
  padding: 16px 40px;
  font-size: 18px;
  font-family: var(--titulo-family);  /* De DESIGN_TOKENS.tipografia.titulo */
  font-weight: 700;
  border-radius: var(--border-radius);  /* De DESIGN_TOKENS.espacamento.border_radius */
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.1s ease;
}

.btn-cta:hover {
  filter: brightness(0.85);  /* Em vez de darken */
  transform: translateY(-2px);
}
```

### Espaçamento (de DESIGN_TOKENS)

Use `DESIGN_TOKENS.espacamento`:
- `secao_padding`: padding entre seções (60px, 80px, 100px, 120px)
- `border_radius`: raio de cantos (4px, 8px, 16px, 24px)

### Elementos (de DESIGN_TOKENS)

Use `DESIGN_TOKENS.elementos` para determinar estilo:
- `separador`: "linha" | "gradiente" | "nenhum" | "ondulado"
- `cards_estilo`: "flat" | "elevado" | "borda-esquerda" | "glassmorphism"
- `botao_estilo`: "solido" | "outline" | "gradiente" | "arredondado"

### Responsive

Mobile-first:
```css
/* Mobile (padrão) */
body { font-size: 16px; }
h1 { font-size: 48px; }

/* Tablet+ */
@media (min-width: 768px) {
  h1 { font-size: 72px; }
}
```

---

## Extras

1. **Countdown Timer** (bloco comentado)
   - Fácil de ativar removendo comentário
   - Timer até data especificada
   - Mostra: dias, horas, minutos, segundos

3. **Scroll to anchor**
   - Nav links scrollam suavemente
   - `scroll-behavior: smooth;`

---

## Placeholders (quando faltar info)

| Campo | Placeholder |
|-------|------------|
| headline | `[INSERIR HEADLINE PRINCIPAL]` |
| expert.nome | `[NOME DO EXPERT]` |
| PRECO_PRODUTO | `[INSERIR PREÇO]` |
| VIDEO_URL | (omitir vídeo, mostrar PRODUTO_IMAGEM_URL ou placeholder genérico) |
| PRODUTO_IMAGEM_URL | Placeholder genérico (imagem cinza com ícone 📷) |
| beneficios[] | 3 cards genéricos |
| depoimentos[] | 3 cards genéricos |
| bonus[] | "Bônus adicionais" |
| faq[] | 5 FAQs padrão |
| LOGO_URL | (navbar mostra apenas texto do produto) |
| EXPERT_FOTO_URL | Placeholder cinza com ícone 👤 |
| COR_PRINCIPAL | `#FF6B00` |
| LINK_CHECKOUT | `href="#"` + `[INSERIR LINK DE CHECKOUT]` |

---

## Validações

- ✅ 8 seções presentes e em ordem
- ✅ Cores aplicadas corretamente
- ✅ Botões CTAs com links corretos
- ✅ Responsive (mobile/tablet/desktop)
- ✅ JSON parseado corretamente
- ✅ Placeholders para campos faltando
- ✅ HTML válido (sem erros de sintaxe)
- ✅ Self-contained (tudo em `<style>` e `<script>`)

---

## Estrutura HTML mínima

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[NOME_PRODUTO]</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@600;700;800;900&family=Open+Sans:wght@400;600&display=swap" rel="stylesheet">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: "Open Sans", sans-serif; color: #222; line-height: 1.6; }
    h1, h2, h3 { font-family: "Montserrat", sans-serif; font-weight: 700; }

    /* 8 seções aqui com CSS inline */
  </style>
</head>
<body>
  <!-- Seção 1-8 -->
  <section id="hero">...</section>
  <section id="beneficios">...</section>
  <!-- ... -->

  <!-- Scripts inline para FAQ accordion, etc -->
  <script>
    // FAQ accordion
    // Countdown timer (comentado)
  </script>
</body>
</html>
```

---

## Procedimento

1. **Valide o JSON_ESTRUTURADO** — Se houver erro, aborte e avise
2. **Parse de DESIGN_TOKENS** — Extraia cores, tipografia, espaçamento
3. **Crie variáveis CSS** — Defina `--primary`, `--titulo-family`, `--border-radius`, etc
4. **Parse de VIDEO_URL** — Se preenchida:
   - Se contém `youtube.com` ou `youtu.be`: extrair video ID
     - Padrão: `https://youtube.com/watch?v=ABC123` → iframe: `https://www.youtube.com/embed/ABC123`
   - Se contém `vimeo.com`: extrair video ID
     - Padrão: `https://vimeo.com/123456789` → iframe: `https://player.vimeo.com/video/123456789`
   - Renderizar iframe com aspect-ratio 16:9, responsive
5. **Parse dos outros inputs** — LINK_CHECKOUT, LOGO_URL, EXPERT_FOTO_URL, PRODUTO_IMAGEM_URL
6. **Gere o HTML** — Com as 8 seções em ordem
7. **Substitua placeholders** — Use valores do JSON ou defaults (VIDEO_URL, PRODUTO_IMAGEM_URL, etc)
8. **Aplique DESIGN_TOKENS** — cores, fontes, espaçamento, elementos
9. **Inline CSS + JS** — Tudo em `<style>` e `<script>`
10. **Salve em** `output/pagina-de-venda.html`
11. **Retorne confirmação** — Arquivo criado + campos ausentes

---

## ⚠️ Restrições

- ❌ Não use CDNs externas (exceto Google Fonts)
- ❌ Não use JavaScript frameworks (jQuery, Vue, React)
- ❌ Não invente informações não presentes no JSON
- ❌ **NÃO override** DESIGN_TOKENS com valores hardcoded
- ✅ DESIGN_TOKENS é fonte de verdade para cores, fontes, espaçamento
- ✅ CSS Flexbox/Grid para layout
- ✅ HTML semântico (nav, section, article, footer)
- ✅ Meta tags essenciais (viewport, charset, title)
- ✅ Aria labels para acessibilidade
- ✅ Use `var(--primary)`, `var(--titulo-family)`, etc nas regras CSS
