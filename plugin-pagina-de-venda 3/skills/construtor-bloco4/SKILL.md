---
name: construtor-bloco4
description: Gera as seções 14, 15 e 17 (BLOCO 4 Emoção/Lógica) do HTML e fecha o documento.
---

# Skill: Construtor de BLOCO 4 (Emoção/Lógica + Fechamento)

## Missão

Gerar as **seções 14, 15 e 17** (BLOCO 4: Emoção/Lógica) do HTML da página de vendas, e fechar o documento com `</body></html>`.

---

## Entrada (INPUT)

**Leia os arquivos** (não são fornecidos no chat):
- `output/config-pagina.json` — Respostas do mentorado (Passo 2)
- `output/design-tokens.json` — Design system validado (Passo 4)
- `output/copy-estruturada.json` — Copy estruturado (Passo 3)

---

## Saída (OUTPUT)

Arquivo salvo em: **`output/bloco4.html`**

**Estrutura**:
- As 3 seções (14, 15, 17)
- Scripts deferidos (se houver)
- Fecha com: `</body></html>`

**Retorno ao chat**:
- ✅ Confirmação: "Bloco 4 salvo em output/bloco4.html"
- Sem emitir o HTML no chat

---

## Seções (14, 15, 17) — Detalhes

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

### 17. COMPARAÇÃO COM OUTRAS SOLUÇÕES
- **Fundo**: `bg_light`
- Tabela comparativa ou dois blocos "Com [produto]" vs. "Sem [produto]"
- Colunas: outros métodos (✗) vs. seu produto (✓)
- Visual claro e organizado, sem denegrir concorrentes
- Objetivo: mostrar diferenciação de forma objetiva
- Botão CTA final (repetição, última aparição)

---

## Design System

Reutiliza variáveis CSS e classes definidas pelo bloco1:
- Variáveis: `--primary`, `--primary-dark`, `--primary-light`, `--accent`, `--bg-dark`, `--bg-dark-alt`, `--bg-light`, `--bg-light-alt`, `--text-dark`, `--text-muted`, `--text-light`, `--text-muted-light`
- Tipografia: `--titulo-family`, `--subtitulo-family`, `--corpo-family`
- Layout: `--secao-padding`, `--border-radius`, `.container`, `.container--narrow`, `.container--text`
- Grids: `.grid-2`, `.grid-3`
- Botões: `.btn-cta--[estilo]` (conforme DESIGN_TOKENS.elementos.botao_estilo)
- Cards: `.card--[estilo]` (conforme DESIGN_TOKENS.elementos.cards_estilo)
- Backgrounds: `.section--dark`, `.section--dark-gradient`, `.section--light`, `.section--light-alt`, `.section--primary-tint`, etc
- Padrões: `.stat-number`, `.highlight`, `.quote`, `.benefit-list`, `.comparison-table`, `.badge`
- Utilitários: `.text-center`, `.text-left`, `.mb-sm`, `.mb-md`, `.mb-lg`, `.mb-xl`

---

## CSS Específico do Bloco 4 (OBRIGATÓRIO)

**REGRA**: Incluir estes estilos via `<style>` tag no início do bloco4.html. São estilos específicos das seções 14, 15 e 17.

```html
<style>
/* ===========================
   BLOCO 4: CSS Específico
   =========================== */

/* === Seção 14: Argumentos Incontestáveis === */
.stat-section {
  text-align: center;
  padding: 20px 0;
}
.stat-source {
  font-family: var(--subtitulo-family), sans-serif;
  font-size: 0.9rem;
  color: var(--text-muted-light);
  font-style: italic;
  margin-top: 12px;
  display: block;
}
.stat-text {
  font-size: 1.2rem;
  color: var(--text-muted-light);
  margin-top: 8px;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}
@media (min-width: 768px) {
  .stat-text { font-size: 1.35rem; }
}

/* === Seção 15: Dores & Desejos === */
.dores-column {
  background: rgba(239, 68, 68, 0.06);
  border-left: 4px solid #EF4444;
  border-radius: 0 var(--border-radius) var(--border-radius) 0;
  padding: 32px 28px;
}
.dores-column h3 {
  color: #EF4444;
  margin-bottom: 16px;
}
.dores-column li {
  padding: 8px 0;
  font-size: 1.05rem;
  line-height: 1.6;
  list-style: none;
  position: relative;
  padding-left: 24px;
}
.dores-column li::before {
  content: '✗';
  position: absolute;
  left: 0;
  color: #EF4444;
  font-weight: 700;
}

.desejos-column {
  background: rgba(34, 197, 94, 0.06);
  border-left: 4px solid #22C55E;
  border-radius: 0 var(--border-radius) var(--border-radius) 0;
  padding: 32px 28px;
}
.desejos-column h3 {
  color: #22C55E;
  margin-bottom: 16px;
}
.desejos-column li {
  padding: 8px 0;
  font-size: 1.05rem;
  line-height: 1.6;
  list-style: none;
  position: relative;
  padding-left: 24px;
}
.desejos-column li::before {
  content: '✓';
  position: absolute;
  left: 0;
  color: #22C55E;
  font-weight: 700;
}

/* === Seção 17: Comparação === */
/* .comparison-table já está definida no bloco1 CSS Reference */
/* Classes adicionais para layout alternativo (blocos lado a lado) */
.comparison-block {
  padding: 32px 28px;
  border-radius: var(--border-radius);
}
.comparison-block--sem {
  background: rgba(239, 68, 68, 0.05);
  border: 1px solid rgba(239, 68, 68, 0.2);
}
.comparison-block--com {
  background: rgba(34, 197, 94, 0.05);
  border: 1px solid rgba(34, 197, 94, 0.2);
}
.comparison-block h3 {
  margin-bottom: 16px;
}
.comparison-block--sem h3 { color: #EF4444; }
.comparison-block--com h3 { color: #22C55E; }
.comparison-block li {
  padding: 8px 0;
  font-size: 1.05rem;
  list-style: none;
  position: relative;
  padding-left: 28px;
}
.comparison-block--sem li::before {
  content: '✗';
  position: absolute;
  left: 0;
  color: #EF4444;
  font-weight: 700;
}
.comparison-block--com li::before {
  content: '✓';
  position: absolute;
  left: 0;
  color: #22C55E;
  font-weight: 700;
}

/* Footer mínimo */
.footer {
  text-align: center;
  padding: 40px 20px;
  background: var(--bg-dark);
  color: var(--text-muted-light);
  font-size: 0.85rem;
}
.footer a {
  color: var(--primary);
  text-decoration: underline;
}
</style>
```

### Backgrounds por Seção (Bloco 4)

| Seção | Background | Motivo |
|-------|-----------|--------|
| 14. Argumentos Incontestáveis | `section--dark-gradient` | Impacto visual para estatísticas |
| 15. Dores & Desejos | `section--light` | Neutro para as colunas coloridas brilharem |
| 17. Comparação + CTA Final | `section--light-alt` | Encerramento limpo |

---

## Fechamento do Documento

Após a seção 17:

```html
  <!-- Scripts deferidos (se houver) -->
  <script>
    // Scripts que precisam ser carregados depois do HTML
  </script>
</body>
</html>
```

**IMPORTANTE**: Fechar com `</body></html>` para completar o documento iniciado pelo bloco1.

---

## Placeholders (quando faltar info)

| Campo | Placeholder |
|-------|------------|
| argumento_incontestavel | `[INSERIR ARGUMENTO COM DADOS]` |
| argumento_fonte | `[INSERIR FONTE DO DADO]` |
| dores[] | 3 dores genéricas |
| desejos[] | 3 desejos genéricos |
| comparacao.sem_produto[] | 3 pontos genéricos |
| comparacao.com_produto[] | 3 pontos genéricos |
| LINK_CHECKOUT | `href="#"` + `[INSERIR LINK DE CHECKOUT]` |

---

## Validações

- ✅ 3 seções presentes e em ordem (14, 15, 17)
- ✅ Cores aplicadas corretamente (usando variáveis CSS)
- ✅ Botão CTA final com link correto
- ✅ Responsive (mobile/tablet/desktop)
- ✅ JSON parseado corretamente
- ✅ Placeholders para campos faltando
- ✅ HTML válido (com fechamento correto)
- ✅ Fecha com `</body></html>`

---

## Procedimento

1. **Leia os 3 arquivos de entrada**
2. **Use variáveis CSS** já definidas
3. **Gere o HTML das 3 seções** (14, 15, 17) em ordem
4. **Substitua placeholders** — Use valores do JSON ou defaults
5. **Aplique DESIGN_TOKENS** — cores, fontes, espaçamento
6. **Inclua scripts deferidos** — Se houver (ex: countdown timer)
7. **Feche o documento** com `</body></html>`
8. **Salve em** `output/bloco4.html`
9. **Retorne confirmação** — Arquivo criado, pronto para concatenação (Passo 6)

---

## ⚠️ Restrições

- ❌ Não repita definições de CSS que já estão em bloco1
- ❌ Não invente informações não presentes no JSON
- ✅ Use variáveis CSS definidas pelo bloco1
- ✅ Apenas as 3 seções (14, 15, 17)
- ✅ Feche corretamente com `</body></html>`
