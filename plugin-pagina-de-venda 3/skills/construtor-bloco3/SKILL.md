---
name: construtor-bloco3
description: Gera as seções 10-13 (BLOCO 3 Credibilidade) do HTML.
---

# Skill: Construtor de BLOCO 3 (Credibilidade)

## Missão

Gerar as **seções 10-13** (BLOCO 3: Credibilidade) do HTML da página de vendas.

---

## Entrada (INPUT)

**Leia os arquivos** (não são fornecidos no chat):
- `output/config-pagina.json` — Respostas do mentorado (Passo 2)
- `output/design-tokens.json` — Design system validado (Passo 4)
- `output/copy-estruturada.json` — Copy estruturado (Passo 3)

---

## Saída (OUTPUT)

Arquivo salvo em: **`output/bloco3.html`**

**Estrutura**:
- Apenas as 4 seções (sem tags de documento ou head)
- Usa variáveis CSS definidas pelo bloco1
- **NÃO inclui** `<!DOCTYPE>`, `<html>`, `<head>`, abertura/fechamento de `<body>`

**Retorno ao chat**:
- ✅ Confirmação: "Bloco 3 salvo em output/bloco3.html"
- Sem emitir o HTML no chat

---

## Seções (10-13) — Detalhes

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
- Botão CTA (repetição)

---

## Design System

Reutiliza variáveis CSS e classes definidas pelo bloco1:
- Variáveis: `--primary`, `--primary-dark`, `--primary-light`, `--accent`, `--bg-dark`, `--bg-dark-alt`, `--bg-light`, `--bg-light-alt`, `--text-dark`, `--text-muted`, `--text-light`, `--text-muted-light`
- Tipografia: `--titulo-family`, `--subtitulo-family`, `--corpo-family`
- Layout: `--secao-padding`, `--border-radius`, `.container`, `.container--narrow`, `.container--text`
- Grids: `.grid-2`, `.grid-3`
- Botões: `.btn-cta--[estilo]` (conforme DESIGN_TOKENS.elementos.botao_estilo)
- Cards: `.card--[estilo]` (conforme DESIGN_TOKENS.elementos.cards_estilo)
- Backgrounds: `.section--dark`, `.section--dark-alt`, `.section--light`, `.section--light-alt`, etc
- Utilitários: `.text-center`, `.mb-sm`, `.mb-md`, `.mb-lg`, `.mb-xl`, `.badge`, `.highlight`, `.quote`
- Accordion: `.accordion-item`, `.accordion-header`, `.accordion-icon`, `.accordion-body`, `.accordion-body-inner` (estilos no bloco1)

---

## CSS Específico do Bloco 3 (OBRIGATÓRIO)

**REGRA**: Incluir estes estilos via `<style>` tag no início do bloco3.html. São estilos específicos das seções 10-13 que complementam o CSS base do bloco1.

```html
<style>
/* ===========================
   BLOCO 3: CSS Específico
   =========================== */

/* === Seção 10: Expert Photo === */
.expert-photo {
  width: 100%;
  max-width: 320px;
  aspect-ratio: 3 / 4;
  object-fit: cover;
  border-radius: var(--border-radius);
  box-shadow: var(--shadow-lg);
}
.expert-photo-placeholder {
  width: 100%;
  max-width: 320px;
  aspect-ratio: 3 / 4;
  background: var(--bg-dark-alt);
  border-radius: var(--border-radius);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 4rem;
  color: var(--text-muted-light);
}
.expert-info h2 {
  margin-bottom: 8px;
}
.expert-info .expert-bio {
  font-size: 1.1rem;
  color: var(--text-muted-light);
  margin-bottom: 20px;
  line-height: 1.7;
}
.expert-resultado {
  background: var(--accent);
  color: #fff;
  padding: 20px 24px;
  border-radius: var(--border-radius);
  font-family: var(--subtitulo-family), sans-serif;
  font-weight: 600;
  font-size: 1.1rem;
  margin-top: 24px;
}

/* === Seção 10: Credenciais === */
.credential-list {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  list-style: none;
  padding: 0;
  margin: 16px 0;
}
.credential-item {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background: rgba(255,255,255,0.1);
  color: var(--text-light);
  padding: 6px 16px;
  border-radius: 50px;
  font-size: 0.85rem;
  font-weight: 600;
  font-family: var(--subtitulo-family), sans-serif;
}
.credential-item::before {
  content: '✦';
  color: var(--accent);
  font-size: 0.7rem;
}

/* === Seção 11: Objeções === */
.objection-item {
  padding: 24px 0;
  border-bottom: 1px solid rgba(0,0,0,0.06);
}
.objection-item:last-child { border-bottom: none; }
.objection-question {
  font-family: var(--subtitulo-family), sans-serif;
  font-weight: 700;
  font-size: 1.15rem;
  margin-bottom: 8px;
  color: var(--text-dark);
}
.objection-answer {
  font-size: 1.05rem;
  color: var(--text-muted);
  line-height: 1.7;
}

/* === Seção 13: Suporte === */
.support-channels {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  margin: 30px 0;
}
.support-channel {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  padding: 24px 20px;
  min-width: 140px;
  background: rgba(255,255,255,0.06);
  border-radius: var(--border-radius);
  transition: background var(--transition-base);
}
.support-channel:hover {
  background: rgba(255,255,255,0.1);
}
.support-channel-icon {
  font-size: 2rem;
}
.support-channel-name {
  font-family: var(--subtitulo-family), sans-serif;
  font-weight: 600;
  font-size: 0.95rem;
  color: var(--text-light);
}
</style>
```

### JS Inline do Accordion FAQ (OBRIGATÓRIO)

**REGRA**: Incluir este script no final do bloco3.html. É o JS que faz o accordion do FAQ funcionar.

```html
<script>
(function() {
  var items = document.querySelectorAll('.accordion-item');
  items.forEach(function(item) {
    var header = item.querySelector('.accordion-header');
    if (!header) return;
    header.addEventListener('click', function() {
      var isActive = item.classList.contains('active');
      // Fechar todos
      items.forEach(function(other) {
        other.classList.remove('active');
        var body = other.querySelector('.accordion-body');
        if (body) body.style.maxHeight = null;
      });
      // Abrir o clicado (se não estava ativo)
      if (!isActive) {
        item.classList.add('active');
        var body = item.querySelector('.accordion-body');
        if (body) body.style.maxHeight = body.scrollHeight + 'px';
      }
    });
  });
})();
</script>
```

### Backgrounds por Seção (Bloco 3)

| Seção | Background | Motivo |
|-------|-----------|--------|
| 10. Autoridade | `section--dark` | Tom sério, credibilidade |
| 11. Objeções | `section--light-alt` | Contraste sutil com FAQ |
| 12. FAQ | `section--light` | Neutro, foco no conteúdo |
| 13. Suporte | `section--dark-alt` | Diferente do hero, acolhedor |

---

## Placeholders (quando faltar info)

| Campo | Placeholder |
|-------|------------|
| expert.nome | `[NOME DO EXPERT]` |
| expert.bio | `[BIO DO EXPERT]` |
| expert.credenciais[] | 2 credenciais genéricas |
| objecoes[] | 3 objeções genéricas com respostas |
| faq[] | 5 FAQs padrão |
| suporte.canais[] | WhatsApp, e-mail, comunidade |
| EXPERT_FOTO_URL | Placeholder cinza com ícone 👤 |
| LINK_CHECKOUT | `href="#"` + `[INSERIR LINK DE CHECKOUT]` |

---

## Validações

- ✅ 4 seções presentes e em ordem
- ✅ Cores aplicadas corretamente (usando variáveis CSS)
- ✅ Botões CTAs com links corretos
- ✅ Accordion FAQ funcional (JS inline)
- ✅ Responsive (mobile/tablet/desktop)
- ✅ JSON parseado corretamente
- ✅ Placeholders para campos faltando
- ✅ HTML válido (sem erros de sintaxe)
- ✅ Sem tags de documento (doctype, html, head, body)

---

## Procedimento

1. **Leia os 3 arquivos de entrada**
2. **Use variáveis CSS** já definidas
3. **Gere o HTML das 4 seções** em ordem
4. **Substitua placeholders** — Use valores do JSON ou defaults
5. **Aplique DESIGN_TOKENS** — cores, fontes, espaçamento
6. **Inclua JS para Accordion** — Abrir/fechar FAQs com JS inline
7. **Salve em** `output/bloco3.html`
8. **Retorne confirmação** — Arquivo criado, pronto para bloco4

---

## ⚠️ Restrições

- ❌ Não inclua `<!DOCTYPE>`, `<html>`, `<head>`, abertura/fechamento `<body>`
- ❌ Não repita definições de CSS que já estão em bloco1
- ❌ Não invente informações não presentes no JSON
- ✅ Use variáveis CSS definidas pelo bloco1
- ✅ Apenas as 4 seções (10-13)
- ✅ Accordion FAQ deve funcionar com JS inline (sem jQuery)
