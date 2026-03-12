# CLAUDE.md

This file provides guidance to Claude Code when working with this plugin.

## Project Overview

**Plugin Gerador de Quiz RTG v1.0.0** is a Claude Code plugin that creates structured sales quizzes automatically, following a proven conversion framework (similar to Inlead quizzes).

The plugin orchestrates a pipeline of AI skills that:
1. Extract data from user-provided copy or briefing
2. Generate all quiz screens (13 stages) with questions, options and logic
3. Create visual design system and functional HTML

**Key Architecture**: Command (`/criar-quiz`) → Extrator de Dados → Revisor de Dados (somente Caminho A) → Criador de Quiz (com auto-validacao) → Designer de Quiz (com auto-validacao) → Integracao Sheets (opcional) → HTML Builder → Output

---

## Quiz Framework (13 Etapas)

The quiz follows a proven psychological conversion flow:

```
1.  Abertura          → Titulo + genero (apresentacao)
2.  Dados Demograficos → Idade com fotos (escolha-unica, condicional por genero)
3.  Prova Social       → Social proof de alunos (apresentacao)
4.  Diagnostico        → Perguntas diagnosticas
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
- `carregamento`: Tela de loading com progresso

---

## Architecture

### Command

- **`/criar-quiz`** (`commands/criar-quiz.md`)
  - Passo 0: Asks if user has copy or not → bifurcates
  - Passo 0.5: revisor-dados validates data (somente Caminho A)
  - Passo 1: criador-quiz generates 18 screens (com auto-validacao)
  - Passo 1.1: User reviews content
  - Passo 1.5: designer-quiz creates visual identity (com auto-validacao)
  - Passo 1.8: integracao-sheets (opcional)
  - Passo 2: html-builder generates functional HTML
  - Passo 3: Confirmation + open in Chrome

### Skills (5 total)

1. **extrator-dados** (`skills/extrator-dados/SKILL.md`)
   - Reads pasted copy/briefing and extracts structured data
   - Output: `output/dados-quiz.json`

2. **revisor-dados** (`skills/revisor-dados/SKILL.md`)
   - Validates and proactively adjusts data extracted from copy (Caminho A only)
   - Fills gaps by inference; only asks user for critical missing fields (max 3 questions)
   - Caminho B skips this skill (inference is done inline during data collection)
   - Output: validated `output/dados-quiz.json`

3. **criador-quiz** (`skills/criador-quiz/SKILL.md`)
   - Generates all 18 screens following the exact validated framework
   - Auto-detects gender/age segmentation and testimonial screen count
   - Includes auto-validation (11 checkpoints) before saving
   - Output: `output/quiz.md` + `output/quiz.json`

4. **designer-quiz** (`skills/designer-quiz/SKILL.md`)
   - Asks 1 question (brand color), auto-determines visual style by niche
   - Creates color palette (11 tokens), Inter typography, component styles
   - Generates interactive HTML preview with 10 sections for user approval
   - Includes auto-validation (9 checkpoints) before presenting to user
   - Output: `output/design-tokens.json` + `output/design-preview.html`

5. **integracao-sheets** (`skills/integracao-sheets/SKILL.md`)
   - Optional skill: asks if user wants Google Sheets tracking
   - If yes: generates Apps Script code (doPost upsert), JS snippet (sessionId + enviarParaSheets), and setup guide
   - Output: `output/apps-script.gs` + `output/sheets-tracking.js` + `output/instrucoes-sheets.md`

6. **html-builder** (`skills/html-builder/SKILL.md`)
   - Generates single autocontained HTML file with all screens, navigation, design system applied
   - Embeds sheets-tracking.js inline if available
   - Output: `output/quiz.html`

---

## File Flow

| File | Written by | Read by |
|------|-----------|---------|
| `output/dados-quiz.json` | Passo 0 + Passo 0.5 (se Caminho A) | Passo 1 (criador-quiz), Passo 1.5 (designer-quiz) |
| `output/quiz.json` | Passo 1 (criador-quiz) | Passo 1.5 (designer-quiz), Passo 2 (html-builder) |
| `output/quiz.md` | Passo 1 (criador-quiz) | Passo 1.1 (revisao) + Final (usuario) |
| `output/design-tokens.json` | Passo 1.5 (designer-quiz) | Passo 2 (html-builder) |
| `output/design-preview.html` | Passo 1.5 (designer-quiz) | Usuario (aprovacao visual) |
| `output/apps-script.gs` | Passo 1.8 (integracao-sheets, opcional) | Final (usuario) |
| `output/sheets-tracking.js` | Passo 1.8 (integracao-sheets, opcional) | Passo 2 (html-builder) |
| `output/instrucoes-sheets.md` | Passo 1.8 (integracao-sheets, opcional) | Final (usuario) |
| `output/quiz.html` | Passo 2 (html-builder) | Final (usuario) |

---

## Key Conventions

0. **SEMPRE usar AskUserQuestion para perguntas** — Em todos os casos, sempre que o plugin precisar de resposta, escolha ou input do usuario, usar a ferramenta `AskUserQuestion`. Nunca fazer perguntas apenas no texto do chat. Nenhuma exceção.

1. **Never invent data** — only use what the user provides; use `[INSERIR X]` for missing info
2. **Conditional logic** — demographics screens split by gender (tela A for men, tela B for women)
3. **Psychological flow** — each stage must connect emotionally to the next
4. **Conversion focus** — every question directs the lead toward the purchase decision
5. **Zero emojis anywhere** — proibido usar emojis no conteudo do quiz, no design, no HTML gerado e em qualquer output do plugin. Isso vale especialmente para emojis de template de IA: 🎯 ⚡ 📊 ✨ 🚀 💡 🔥 ⭐ e similares.
6. **Continuous flow** — do NOT pause between steps unless a real decision is needed

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
