---
name: construtor-bloco2
description: Gera as seções 6-9 (BLOCO 2 Oferta/Detalhes) do HTML.
---

# Skill: Construtor de BLOCO 2 (Oferta/Detalhes)

## Missão

Gerar as **seções 6-9** (BLOCO 2: Oferta/Detalhes) do HTML da página de vendas.

---

## Entrada (INPUT)

**Leia os arquivos** (não são fornecidos no chat):
- `output/config-pagina.json` — Respostas do mentorado (Passo 2)
- `output/design-tokens.json` — Design system validado (Passo 4)
- `output/copy-estruturada.json` — Copy estruturado (Passo 3)

---

## Saída (OUTPUT)

Arquivo salvo em: **`output/bloco2.html`**

**Estrutura**:
- Apenas as 4 seções (sem tags de documento ou head)
- Usa variáveis CSS definidas pelo bloco1
- **NÃO inclui** `<!DOCTYPE>`, `<html>`, `<head>`, abertura/fechamento de `<body>`

**Retorno ao chat**:
- ✅ Confirmação: "Bloco 2 salvo em output/bloco2.html"
- Sem emitir o HTML no chat

---

## Seções (6-9) — Detalhes

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
- Botão CTA (repetição)
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

## Design System

Reutiliza variáveis CSS e classes definidas pelo bloco1:
- Variáveis: `--primary`, `--primary-dark`, `--primary-light`, `--accent`, `--bg-dark`, `--bg-dark-alt`, `--bg-light`, `--bg-light-alt`, `--text-dark`, `--text-muted`, `--text-light`, `--text-muted-light`
- Tipografia: `--titulo-family`, `--subtitulo-family`, `--corpo-family`
- Layout: `--secao-padding`, `--border-radius`, `.container`, `.container--narrow`, `.container--text`
- Grids: `.grid-2`, `.grid-3`
- Botões: `.btn-cta--[estilo]` (conforme DESIGN_TOKENS.elementos.botao_estilo)
- Cards: `.card--[estilo]` (conforme DESIGN_TOKENS.elementos.cards_estilo)
- Backgrounds: `.section--dark`, `.section--dark-gradient`, `.section--light`, `.section--primary-tint`, etc
- Utilitários: `.text-center`, `.mb-sm`, `.mb-md`, `.mb-lg`, `.mb-xl`, `.badge`, `.highlight`

---

## CSS Específico do Bloco 2 (OBRIGATÓRIO)

**REGRA**: Incluir estes estilos via `<style>` tag no início do bloco2.html. São estilos específicos das seções 6-9 que complementam o CSS base do bloco1.

```html
<style>
/* ===========================
   BLOCO 2: CSS Específico
   =========================== */

/* === Seção 7: Preço === */
.price-block {
  text-align: center;
  padding: 40px 0;
}
.price-old {
  font-family: var(--subtitulo-family), sans-serif;
  font-size: 1.3rem;
  color: var(--text-muted-light);
  text-decoration: line-through;
  margin-bottom: 8px;
  display: block;
}
.price-current {
  font-family: var(--titulo-family), sans-serif;
  font-weight: 900;
  font-size: 3.5rem;
  color: var(--primary);
  line-height: 1.1;
}
@media (min-width: 768px) {
  .price-current { font-size: 4rem; }
}
@media (min-width: 1024px) {
  .price-current { font-size: 4.5rem; }
}
.price-parcela {
  font-size: 1.1rem;
  color: var(--text-muted-light);
  margin-top: 8px;
}

/* === Seção 7: Entregáveis === */
.entregavel {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  padding: 16px 0;
  border-bottom: 1px solid rgba(255,255,255,0.08);
  gap: 16px;
}
.entregavel:last-child { border-bottom: none; }
.entregavel-nome {
  font-family: var(--subtitulo-family), sans-serif;
  font-weight: 600;
  font-size: 1.05rem;
  color: var(--text-light);
  flex: 1;
}
.entregavel-valor {
  font-family: var(--subtitulo-family), sans-serif;
  font-size: 0.95rem;
  color: var(--text-muted-light);
  white-space: nowrap;
  text-align: right;
}
.entregavel-total {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  margin-top: 16px;
  border-top: 2px solid var(--primary);
  font-family: var(--titulo-family), sans-serif;
  font-weight: 700;
  font-size: 1.2rem;
  color: var(--text-light);
}

/* === Seção 8: Garantia === */
.garantia-block {
  border: 2px dashed var(--accent);
  border-radius: var(--border-radius);
  padding: 32px;
  background: rgba(255,255,255,0.03);
  text-align: center;
  max-width: 600px;
  margin: 40px auto;
}
.garantia-block h3 {
  margin-bottom: 12px;
}
.garantia-block p {
  color: var(--text-muted-light);
  font-size: 1.05rem;
}
.garantia-icon {
  font-size: 2.5rem;
  margin-bottom: 16px;
  display: block;
}

/* === Seção 8: Depoimentos (texto) === */
.testimonial-card {
  padding: 28px 24px;
  position: relative;
}
.testimonial-card::before {
  content: '"';
  font-family: var(--titulo-family), sans-serif;
  font-size: 4rem;
  color: var(--primary);
  opacity: 0.3;
  position: absolute;
  top: 8px;
  left: 16px;
  line-height: 1;
}
.testimonial-text {
  font-style: italic;
  font-size: 1.05rem;
  line-height: 1.7;
  margin-bottom: 16px;
  padding-top: 16px;
}
.testimonial-author {
  display: flex;
  align-items: center;
  gap: 12px;
}
.testimonial-avatar {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: var(--primary-light);
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 1.1rem;
  color: var(--primary);
  flex-shrink: 0;
}
.testimonial-name {
  font-family: var(--subtitulo-family), sans-serif;
  font-weight: 600;
  font-size: 0.95rem;
}
.testimonial-role {
  font-size: 0.85rem;
  color: var(--text-muted);
}

/* === Seção 9: Paliativo === */
.paliativo-card {
  border: 3px solid var(--primary);
  border-radius: var(--border-radius);
  padding: 40px 32px;
  text-align: center;
  max-width: 700px;
  margin: 0 auto;
  background: linear-gradient(135deg, var(--primary-light), transparent);
}
.paliativo-card h3 {
  margin-bottom: 12px;
}
.paliativo-card p {
  margin-bottom: 24px;
  font-size: 1.1rem;
}
</style>
```

### Backgrounds por Seção (Bloco 2)

| Seção | Background | Motivo |
|-------|-----------|--------|
| 6. Formato do Produto | `section--light` | Seção informativa, fundo neutro |
| 7. Oferta + Preço + Garantia | `section--dark-gradient` | Destaque máximo para o preço |
| 8. Depoimentos | `section--light` | Neutro para os cards brilharem |
| 9. Paliativo | `section--primary-tint` | Destaque especial para o bônus rápido |

---

## Placeholders (quando faltar info)

| Campo | Placeholder |
|-------|------------|
| produto.tipo | `[INSERIR TIPO DO PRODUTO]` |
| produto.formato_entrega | `[INSERIR COMO É ENTREGUE]` |
| beneficios[] | 3 cards genéricos |
| produto.preco | `[INSERIR PREÇO]` |
| depoimentos[] | 3 cards genéricos |
| bonus[] | "Bônus adicionais" |
| LINK_CHECKOUT | `href="#"` + `[INSERIR LINK DE CHECKOUT]` |

---

## Validações

- ✅ 4 seções presentes e em ordem
- ✅ Cores aplicadas corretamente (usando variáveis CSS)
- ✅ Botões CTAs com links corretos
- ✅ Responsive (mobile/tablet/desktop)
- ✅ JSON parseado corretamente
- ✅ Placeholders para campos faltando
- ✅ HTML válido (sem erros de sintaxe)
- ✅ Sem tags de documento (doctype, html, head, body)

---

## Procedimento

1. **Leia os 3 arquivos de entrada**
2. **Use variáveis CSS** já definidas (--primary, --titulo-family, etc)
3. **Gere o HTML das 4 seções** em ordem
4. **Substitua placeholders** — Use valores do JSON ou defaults
5. **Aplique DESIGN_TOKENS** — cores, fontes, espaçamento
6. **Salve em** `output/bloco2.html`
7. **Retorne confirmação** — Arquivo criado, pronto para bloco3

---

## ⚠️ Restrições

- ❌ Não inclua `<!DOCTYPE>`, `<html>`, `<head>`, abertura/fechamento `<body>`
- ❌ Não repita definições de CSS que já estão em bloco1
- ❌ Não invente informações não presentes no JSON
- ✅ Use variáveis CSS definidas pelo bloco1
- ✅ Apenas as 4 seções (6-9)
