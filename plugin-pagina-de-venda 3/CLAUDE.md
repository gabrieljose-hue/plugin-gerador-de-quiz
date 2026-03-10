# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Plugin Página de Venda RTG v2.0.0** is a Claude Code plugin that transforms marketing copy into professional, responsive HTML sales pages without requiring programming knowledge.

The plugin orchestrates a sophisticated pipeline of 7 AI "skills" that:
1. Generate or extract structured copy (QFD framework)
2. Validate copy for persuasion, structure, and non-AI language
3. Design a custom design system based on niche and price point
4. Validate design for visual identity and niche alignment
5. Generate HTML with 17 sections and design tokens

**Key Architecture**: Command (`/criar-pagina`) → Copywriter → Extrator → Revisor-Copy → Designer → Revisor-Design → Construtor-HTML

---

## High-Level Architecture

### Fluxo Principal (8 Passos)

```
Passo 0: Bifurcação inicial (Copy pronto vs. Gerar novo)
         → Se novo: Skill copywriter faz 4 rodadas de perguntas
         → Se pronto: Usuário cola o texto

Passo 1: Obter copy confirmado do mentorado
         → COPY_BRUTO estabelecido

Passo 2: Coletar 8 respostas interativas (cor, nome, preço, link, etc)
         → SETUP_RESPOSTAS estabelecidas

Passo 3: Skill extrator-copy extrai JSON estruturado
         → INPUT: COPY_BRUTO
         → OUTPUT: JSON_ESTRUTURADO (com campos_ausentes[])

Passo 3.5: Skill revisor-copy valida copy (APENAS se origem='copywriter')
           → INPUT: COPY_BRUTO + JSON_ESTRUTURADO
           → OUTPUT: COPY_VALIDADA ou feedback para refazer
           → Checklist: Premissa (≤10 palavras) | QFD completo | Promessa clara
           →            "Para quem é/não é" claro | Argumento incontestável | Sem emojis de IA

Passo 4: Skill designer-html define design system
         → INPUT: JSON_ESTRUTURADO + COR_PRINCIPAL + NOME_PRODUTO + PRECO_PRODUTO
         → OUTPUT: DESIGN_TOKENS (JSON) + output/design-preview.html
         → Raciocínio: Analisar nicho → Sopesar por preço → Big Idea Visual → Estética
         → Questiona cor se não combinar, aguarda aprovação

Passo 4.5: Skill revisor-design valida design (SEMPRE)
           → INPUT: DESIGN_TOKENS + design-preview.html + JSON_ESTRUTURADO
           → OUTPUT: DESIGN_APROVADO ou feedback para designer refazer
           → Checklist: 3i's visuais (Comunicador|Consumidor|Produto)
           →            Big Idea Visual distinta | Alinhamento com nicho | Sem parecer IA

Passo 5: Skill construtor-html gera HTML final com 17 seções
         → INPUT: JSON_ESTRUTURADO + DESIGN_APROVADO + SETUP_RESPOSTAS
         → OUTPUT: output/pagina-de-venda.html
         → 17 seções em 4 blocos (Hero|Oferta|Credibilidade|Emoção+Lógica)
         → DESIGN_TOKENS é fonte de verdade para cores, tipografia, espaçamento
         → Placeholders [INSERIR X] para campos faltando

Passo 6: Confirmação ao mentorado
         → Listar campos ausentes
         → Instruções de publicação
```

### Comando Orquestrador

- **`/criar-pagina`** (arquivo: `commands/criar-pagina.md`)
  - Define o fluxo completo
  - Gerencia variáveis entre passos
  - Chama skills na sequência correta
  - Implementa bifurcação do Passo 0 (copy pronto vs. gerar)
  - Condicional: Passo 3.5 (revisor-copy) SÓ se `COPY_ORIGEM = 'copywriter'`

---

## Framework QFD (Quadro/Furadeira/Decorado)

**Toda copy deve conter essas 3 camadas estruturais:**

### Quadro (Q) — O Resultado / Transformação
A transformação prometida ao aluno após usar o produto.
- Exemplo: "Ganhar R$ 5.000/mês trabalhando de casa"
- ✅ Específico (com números, contexto)
- ❌ Não é "melhorar sua vida"

### Furadeira (F) — O Método / Passo a Passo
O caminho/processo que o aluno segue para atingir o Quadro (não é prova).
- Exemplo: "3 fases: descobrir nicho → criar produto → perpetuar vendas"
- ✅ Descreve COMO funciona
- ❌ Não é "tem 500+ alunos satisfeitos" (isso é prova)

### Decorado (D) — Benefícios Emocionais pós-Quadro
Consequências aspiracionais que acontecem DEPOIS que o Quadro é atingido.
- Exemplo: "Mais tempo com família, liberdade de horário, fim da insegurança"
- ✅ Emocionais e aspiracionais
- ❌ Não é o resultado principal (é consequência do resultado)

---

## 7 Skills Structure

Each skill is a `SKILL.md` markdown file inside `skills/{skill-name}/` with documented:
- Missão (purpose)
- Entrada (INPUT)
- Saída (OUTPUT)
- Checklist/Validações
- Procedimento
- Exemplos
- Restrições

### 1. **copywriter** (`skills/copywriter/SKILL.md`)
- **When**: Passo 0, only if user chooses "Gerar novo copy"
- **Input**: Interactive questions (4 rodadas)
- **Output**: COPY_BRUTO structured with 17 sections in QFD framework
- **Critical**: No AI-typical emojis (🎯 🚀 ⚡ 📈 🎥)
- **Key Validation**: Premissa (≤10 palavras, curiosity-driven)

### 2. **extrator-copy** (`skills/extrator-copy/SKILL.md`)
- **When**: Passo 3 (always)
- **Input**: COPY_BRUTO
- **Output**: JSON_ESTRUTURADO (with `campos_ausentes[]` array)
- **Critical Rule**: Never invent — empty fields go to `campos_ausentes[]`, not guessing
- **Output Format**: Wrapped in `<COPY_ESTRUTURADA>...</COPY_ESTRUTURADA>` tags

### 3. **revisor-copy** (`skills/revisor-copy/SKILL.md`)
- **When**: Passo 3.5, ONLY if `COPY_ORIGEM = 'copywriter'` (skip if user's own copy)
- **Input**: COPY_BRUTO + JSON_ESTRUTURADO + nicho context
- **Output**: COPY_VALIDADA (approved) OR feedback to refactor
- **Checklist** (show work):
  - Premissa: ≤10 words, creative, curiosity-generating
  - QFD: Quadro + Furadeira + Decorado concepts correct
  - Promise: Clear, specific, measurable
  - Segmentation: "Para quem é / Para quem NÃO é" explicit
  - Incontestable Argument: Data/research-backed claim
  - Language: Natural Portuguese, no jargon
  - **No AI markers**: No 🎯 🚀 ⚡ 📈 🎥 emojis
- **Tone**: Senior copywriter reviewing for persuasion + structure

### 4. **designer-html** (`skills/designer-html/SKILL.md`)
- **When**: Passo 4
- **Input**: JSON_ESTRUTURADO + COR_PRINCIPAL (hex) + NOME_PRODUTO + PRECO_PRODUTO
- **Output**: DESIGN_TOKENS (JSON) + `output/design-preview.html` (visual style guide, no real copy)
- **Critical Process**:
  1. Analyze niche → determine sophistication (price bands: <R$200 vs R$1k–R$5k, etc)
  2. Extract "Big Idea Visual" (unique visual element that encodes the unique insight)
  3. Choose aesthetic (premium | bold | minimalista | urgente | tecnico | inspiracional)
  4. Define 6-color palette (primary, accent, bg_dark, bg_light, text_dark, text_light)
  5. Choose 3 typography families (titulo, subtitulo, corpo) from Google Fonts
  6. Question color choice if misaligned with niche (ask user to confirm)
  7. Generate preview HTML (visual style guide only, no real copy)
  8. Return DESIGN_TOKENS between `<DESIGN_TOKENS>...</DESIGN_TOKENS>` tags
- **Key Tables**:
  - Niche → Aesthetic + Typography + Common primary colors (see SKILL.md)
  - Price band → Sophistication + Padding + Border-radius
- **User Interaction**: Show preview, ask "Deseja fazer ajustes?" → iterate if needed

### 5. **revisor-design** (`skills/revisor-design/SKILL.md`)
- **When**: Passo 4.5 (always)
- **Input**: DESIGN_TOKENS + design-preview.html + JSON_ESTRUTURADO + NOME_PRODUTO
- **Output**: DESIGN_APROVADO (approved) OR feedback to designer-html to refactor
- **Checklist** (show work):
  - 3i's visually reflected (Identidade do Comunicador | Consumidor | Produto)
  - Big Idea Visual exists and repeats (distinct, not stock icons)
  - Niche alignment (e.g., education → professional | fitness → energetic)
  - **No AI appearance**: Colors/typography not generic template
  - Palette consistent, typography clear, hierarchy defined, spacing adequate
  - Headlines/Subheadlines centered, bullet points left-aligned
- **Tone**: Senior designer validating for visual identity + niche fit

### 6. **construtor-html** (`skills/construtor-html/SKILL.md`)
- **When**: Passo 5
- **Input**: JSON_ESTRUTURADO + DESIGN_APROVADO + all SETUP_RESPOSTAS (8 answers)
- **Output**: `output/pagina-de-venda.html` (complete, self-contained HTML)
- **17 Sections in 4 Blocks**:
  - **BLOCO 1 (Hero)**: Hero | CTA Text | Button | Demo | Para Quem É/Não É
  - **BLOCO 2 (Offer)**: Formato | O que recebe | Garantia | Depoimentos
  - **BLOCO 3 (Credibility)**: Autoridade | Objeções | FAQ | Suporte
  - **BLOCO 4 (Emotion)**: Argumentos Incontestáveis | Dores & Desejos | Comparação
- **Design Tokens as Source of Truth**: DESIGN_TOKENS.cores, .tipografia, .espacamento guide ALL styling
- **CSS Variables**: Use `var(--primary)`, `var(--titulo-family)`, etc throughout
- **Responsive**: Mobile-first (48px h1 mobile → 72px desktop)
- **Extras**: Sticky CTA button (after 400px scroll), FAQ accordion (JS inline), countdown timer (commented)
- **Placeholders**: [INSERIR X] for missing fields (never invent)

---

## Key Files & Directories

```
plugin-pagina-de-venda/
├── .claude-plugin/
│   └── plugin.json                 # Plugin metadata (v2.0.0, name, author)
├── commands/
│   └── criar-pagina.md             # Main orchestration command (8 steps, bifurcation logic)
├── skills/
│   ├── copywriter/SKILL.md         # Generate copy from questions (4 rodadas)
│   ├── extrator-copy/SKILL.md      # Extract JSON from copy (with campos_ausentes[])
│   ├── revisor-copy/SKILL.md       # Validate copy (QFD, promise, no AI markers)
│   ├── designer-html/SKILL.md      # Design system + preview (DESIGN_TOKENS)
│   ├── revisor-design/SKILL.md     # Validate design (3i's, Big Idea, niche alignment)
│   └── construtor-html/SKILL.md    # Generate HTML with 17 sections
├── entrada/                        # Input files (copy.docx, etc)
├── output/                         # Generated files
│   ├── pagina-de-venda.html       # Final sales page
│   └── design-preview.html         # Design system preview (generated by designer-html)
├── README.md                       # English documentation
└── LEIA-ME.md                     # Portuguese user guide (for mentorado)
```

---

## Critical Validations & Patterns

### Copy Validation (Revisor-Copy)

**Must-have checklist**:
- [ ] Premissa: ≤10 words, creative, generates curiosity
- [ ] QFD: Quadro (result) + Furadeira (method) + Decorado (emotional benefits)
- [ ] Promise: Clear, specific, measurable (e.g., "R$ 5.000/mês em 90 dias")
- [ ] Segmentation: "Para quem é" (3-5 bullets) + "Para quem não é" (3-5 bullets)
- [ ] Incontestable Argument: Data-backed claim (e.g., "92% relatam melhora em 30 dias")
- [ ] Language: Natural Portuguese, no buzzwords or AI jargon
- [ ] **NO AI markers**: 🎯 🚀 ⚡ 📈 🎥 and similar MUST NOT appear
- Result: APROVADO | REJEITADO com feedback | MELHORIAS SUGERIDAS

### Design Validation (Revisor-Design)

**Must-have checklist**:
- [ ] 3i's reflected visually (Comunicador identity, Consumidor aspirations, Produto promise)
- [ ] Big Idea Visual: unique, repeatable, not generic stock element
- [ ] Niche alignment: e.g., education→professional, coaching→warm, finance→sober
- [ ] **No AI appearance**: Template-free, distinctive, not generic landing page
- [ ] Color palette consistency, typography clarity, hierarchy, spacing
- [ ] Headlines/Subheadlines centered, bullet points left-aligned
- Result: APROVADO | AJUSTES NECESSÁRIOS | REJEIÇÃO

### Copy Structure (Extrator-Copy)

**Never invent rule**: If a field is not explicitly in COPY_BRUTO → empty string + add to `campos_ausentes[]`

Example of correct handling:
```json
"expert": {
  "nome": "João Silva",
  "bio": "",                    // Not found → empty
  "credenciais": []             // Not found → empty array
},
"campos_ausentes": ["expert.bio", "expert.credenciais"]
```

### Design Tokens Structure

**DESIGN_TOKENS keys used by construtor-html**:
```json
{
  "cores": { "primary", "accent", "bg_dark", "bg_light", "text_dark", "text_light" },
  "tipografia": {
    "google_fonts_url",
    "titulo": { "family", "weights" },
    "subtitulo": { "family", "weights" },
    "corpo": { "family", "weights" }
  },
  "espacamento": { "secao_padding", "border_radius" },
  "elementos": { "separador", "cards_estilo", "botao_estilo" },
  "big_idea_visual": "description of unique visual element",
  "estetica": "enum (premium|bold|minimalista|urgente|tecnico|inspiracional)"
}
```

---

## Development Patterns

### Adding a New Skill

1. Create `skills/{new-skill-name}/SKILL.md` with documented structure:
   - Missão, Entrada, Saída
   - Step-by-step procedure
   - Validation checklist
   - Example input/output
   - Restrictions

2. Update `commands/criar-pagina.md` to reference the skill in the correct step

3. Document the INPUT/OUTPUT contract clearly (JSON tags like `<COPY_ESTRUTURADA>`)

### Modifying an Existing Skill

1. Read the full SKILL.md to understand contracts
2. Identify INPUT/OUTPUT format (look for `<TAG>...</TAG>` markers)
3. Do NOT change input/output structure without updating dependent skills
4. Update validation checklist if logic changes
5. Update example (at bottom of SKILL.md) to match new logic

### Variable Flow Between Steps

- **Passo 2 Responses** → `COR_PRINCIPAL`, `NOME_PRODUTO`, `PRECO_PRODUTO`, `LINK_CHECKOUT`, `URGENCIA_ESCASSEZ`, `TIPO_DEPOIMENTOS`, `LOGO_URL`, `EXPERT_FOTO_URL`
- **Passo 3 Output** → `JSON_ESTRUTURADO` (between `<COPY_ESTRUTURADA>` tags)
- **Passo 4 Output** → `DESIGN_TOKENS` (between `<DESIGN_TOKENS>` tags)
- **Passo 5 Input**: ALL the above + `COPY_VALIDADA` + `DESIGN_APROVADO`

---

## Common Tasks

### Running /criar-pagina Flow

1. User types `/criar-pagina` in Claude Code chat
2. Command asks bifurcation: copy pronto or gerar novo?
3. If novo: copywriter skill runs 4 rodadas of questions
4. Collect 8 setup answers
5. Call extrator-copy (Passo 3)
6. If copy was generated: call revisor-copy (Passo 3.5)
7. Call designer-html (Passo 4) + revisor-design (Passo 4.5)
8. Call construtor-html (Passo 5)
9. Output summary + list campos_ausentes

### Editing Generated HTML

Users can manually edit `output/pagina-de-venda.html`:
- Self-contained (all CSS/JS inline)
- Replace color hex codes (e.g., `#FF6B00` → new color)
- Replace `[INSERIR X]` placeholders with real content
- Change text directly (HTML is semantic)

### Rerunning After Changes

If user changes copy and runs `/criar-pagina` again:
- `output/pagina-de-venda.html` gets overwritten
- If design changes needed, designer-html will re-analyze niche/price
- All 8 steps run again

---

## Important Conventions

1. **Always reason out loud** (especially designer-html, revisor-copy, revisor-design)
   - Show analysis: nicho → sofisticação → estética
   - Show QFD breakdown
   - Show color logic

2. **No invented content**
   - extrator-copy: empty field + add to campos_ausentes[]
   - construtor-html: use [INSERIR X] placeholder

3. **No external dependencies**
   - Google Fonts only (for typography)
   - All CSS inline in `<style>`
   - All JS inline in `<script>` (vanilla JS only)
   - No jQuery, React, Vue, etc.

4. **Responsive mobile-first**
   - 16px base font size (mobile)
   - 48px h1 (mobile), 72px (desktop)
   - Flexbox/Grid for layout
   - No fixed widths, max-width: 1200px container

5. **Language & Tone**
   - Copywriter: specificity, emotional connection, no jargon
   - Designer: "thinking out loud" about niche + aesthetic
   - Revisor-Copy: senior copywriter voice, constructive feedback
   - Revisor-Design: senior designer voice, explaining visual choices
   - Construtor-HTML: implementation detail, placeholders clear

6. **QFD Framework**
   - Every copy MUST have Quadro (result) + Furadeira (method) + Decorado (emotions)
   - These are non-negotiable for persuasion
   - Revisor-Copy verifies; revisor-design ensures visual alignment

7. **SVG Geometric Icons (CRITICAL)**
   - **JAMAIS use emojis** (🎯 🚀 ⚡ 📈 🎥 ✅ ❌ etc.)
   - All icons are **inline SVG with pure geometry** (circles, lines, polygons)
   - Refer to `icons-system.md` for complete icon library & usage
   - Icons inherit color from text via `currentColor` or `color: var(--primary)`
   - Standard size: 24px, stroke-width: 2 (outline) or fill (solid)
   - Each skill uses only 3-5 relevant icons (no icon overload)
   - Exemplos por nicho:
     - **Educação**: Checkmark, Arrow Right, Trending Up, Heart, Bell
     - **Fitness/Wellness**: Zap, Heart, Trending Up, Bell
     - **Business/Coaching**: Briefcase, User/Person, Trending Up, Lock, Shield

---

## References

- **Plugin Metadata**: `.claude-plugin/plugin.json` (v2.0.0)
- **User Documentation**: `LEIA-ME.md` (Portuguese guide for mentorado)
- **Icon System**: `icons-system.md` (SVG geometric icons library, usage guidelines)
- **Technical Documentation**: Each `SKILL.md` file (see "Skills Structure" above)
- **Command Orchestration**: `commands/criar-pagina.md` (step-by-step flow, variables, bifurcation)
- **Example Page Structure**: construtor-html SKILL.md contains HTML minimal template + 17 sections breakdown
