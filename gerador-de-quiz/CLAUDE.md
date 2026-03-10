# CLAUDE.md

This file provides guidance to Claude Code when working with this plugin.

## Project Overview

**Plugin Gerador de Quiz RTG v1.0.0** is a Claude Code plugin that creates structured sales quizzes automatically, following a proven conversion framework (similar to Inlead quizzes).

The plugin orchestrates a pipeline of 3 AI skills that:
1. Collect all necessary data about the product, theme and audience
2. Generate all quiz screens (13 stages) with questions, options and logic
3. Review and validate the quiz for conversion quality

**Key Architecture**: Command (`/criar-quiz`) → Coletor/Extrator de Dados → Revisor de Dados → Criador de Quiz → Designer de Quiz → Revisor de Design → Revisor de Quiz → Output

---

## Quiz Framework (13 Etapas)

The quiz follows a proven psychological conversion flow:

```
1.  Abertura          → Titulo + genero (apresentacao)
2.  Dados Demograficos → Idade com fotos (escolha-unica, condicional por genero)
3.  Prova Social       → Social proof de alunos (apresentacao)
4.  Diagnostico        → Perguntas diagnosticas + captura nome/email
5.  Tocar na Ferida    → Consequencias negativas do problema (multipla-escolha)
6.  Desejo Principal   → Objetivo principal do lead (multipla-escolha)
7.  Depoimentos        → Provas de solucao + perguntas adicionais (apresentacao)
8.  Prometer Ajuda     → Promessa personalizada (apresentacao)
9.  Micro-Compromisso  → Nivel de motivacao (escolha-unica)
10. Carregamento       → Analise de perfil (carregamento)
11. Resultado          → Lead esta pronto + grafico (apresentacao)
12. Pagina de Vendas   → Oferta completa interna no quiz
```

**Tipos de tela:**
- `apresentacao`: Texto, titulo, subtitulo e imagens (sem interacao)
- `escolha-unica`: O lead escolhe exatamente 1 opcao
- `multipla-escolha`: O lead escolhe uma ou mais opcoes
- `captura`: Campo de texto (nome, email)
- `carregamento`: Tela de loading com progresso

---

## Architecture

### Command

- **`/criar-quiz`** (`commands/criar-quiz.md`)
  - Passo 0: Asks if user has copy or not → bifurcates
  - Passo 0.5: revisor-dados validates data from either path
  - Passo 1: criador-quiz generates 18 screens
  - Passo 2: revisor-quiz validates conversion quality

### Skills (7 total)

1. **extrator-dados** (`skills/extrator-dados/SKILL.md`)
   - Reads pasted copy/briefing and extracts structured data
   - Path A (user has copy)
   - Output: `output/dados-quiz.json`

2. **coletor-dados** (`skills/coletor-dados/SKILL.md`)
   - Interactive questions via AskUserQuestion (3 rounds)
   - Path B (user creates from scratch)
   - Output: `output/dados-quiz.json`

3. **revisor-dados** (`skills/revisor-dados/SKILL.md`)
   - Validates and proactively adjusts data from either path
   - Fills gaps by inference; only asks user for critical missing fields (max 3 questions)
   - Both paths pass through this skill
   - Output: validated `output/dados-quiz.json`

4. **criador-quiz** (`skills/criador-quiz/SKILL.md`)
   - Generates all 18 screens following the exact validated framework
   - Auto-detects gender/age segmentation and testimonial screen count
   - Output: `output/quiz.md` + `output/quiz.json`

5. **designer-quiz** (`skills/designer-quiz/SKILL.md`)
   - Asks 2 questions (brand color + visual style), then generates full design system
   - Creates color palette (11 tokens), Google Fonts typography, component styles
   - Generates interactive HTML preview with 10 sections for user approval
   - Output: `output/design-tokens.json` + `output/design-preview.html` + `output/quiz-design.md`

6. **revisor-design** (`skills/revisor-design/SKILL.md`)
   - Validates the design kit against 10 checkpoints (niche coherence, contrast, card states, etc.)
   - Checks for non-generic identity (not a plain AI template)
   - Output: APROVADO or specific fixes for designer-quiz to correct

7. **revisor-quiz** (`skills/revisor-quiz/SKILL.md`)
   - Validates 11 quality checkpoints
   - Checks: 18 screens present, tela 8 only affirmative options, tela 13+14 dynamic response, flow order
   - Output: APROVADO or specific fixes needed

---

## File Flow

| File | Written by | Read by |
|------|-----------|---------|
| `output/dados-quiz.json` | Passo 0 (extrator ou coletor) + Passo 0.5 (revisor-dados) | Passo 1 (criador-quiz), Passo 1.5 (designer-quiz), Passo 1.75 (revisor-design), Passo 2 (revisor-quiz) |
| `output/quiz.json` | Passo 1 (criador-quiz) | Passo 1.5 (designer-quiz), Passo 2 (revisor-quiz) |
| `output/quiz.md` | Passo 1 (criador-quiz) | Final (usuario) |
| `output/design-tokens.json` | Passo 1.5 (designer-quiz) | Passo 1.75 (revisor-design) + Final (usuario) |
| `output/design-preview.html` | Passo 1.5 (designer-quiz) | Passo 1.75 (revisor-design) |
| `output/quiz-design.md` | Passo 1.5 (designer-quiz) | Final (usuario) |

---

## Key Conventions

1. **Never invent data** — only use what the user provides; use `[INSERIR X]` for missing info
2. **Personalization with {{nome}}** — use the captured name variable in subsequent screens
3. **Conditional logic** — demographics screens split by gender (tela A for men, tela B for women)
4. **Psychological flow** — each stage must connect emotionally to the next
5. **Conversion focus** — every question directs the lead toward the purchase decision
6. **No emojis in quiz content** — keep professional and clean
7. **Continuous flow** — do NOT pause between steps unless a real decision is needed

---

## Output Format (quiz.md)

Each screen is documented as:

```
### Tela [N]: [Titulo da tela]
**Tipo**: [tipo]
**Etapa**: [nome da etapa]

**Conteudo**:
- Titulo: [texto]
- Subtitulo: [texto]
- Imagem: [descricao da imagem sugerida]

**Opcoes** (se aplicavel):
1. [opcao 1]
2. [opcao 2]

**Logica**: [condicional se houver]
**Objetivo psicologico**: [o que esta tela faz no lead]
```
