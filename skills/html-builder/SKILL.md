---
name: html-builder
description: Gera o arquivo quiz.html — um quiz funcional e completo em HTML puro com todas as telas conectadas, navegacao, variaveis dinamicas e design system aplicado.
---

# Skill: HTML Builder

## Missao

Ler os arquivos de saida do pipeline e gerar um unico arquivo `output/quiz.html` autocontido e funcional. O HTML deve ter todas as telas do quiz conectadas, logica de navegacao condicional, variavel dinamica {{resposta_tela_13}}, design system aplicado e interacao completa — pronto para abrir no navegador e usar.

---

## REGRA DE PERGUNTAS (OBRIGATORIO)

**Sempre usar a ferramenta `AskUserQuestion`** para qualquer pergunta, escolha ou coleta de input do usuario. Em todos os casos, sem excecao. Nunca fazer perguntas apenas no texto do chat.

---

## Entrada (INPUT)

- `output/quiz.json` — estrutura de todas as telas
- `output/design-tokens.json` — paleta, tipografia, componentes
- `output/imagens-quiz.json` (opcional) — URLs de imagens do Pexels por tela

---

## Saida (OUTPUT)

- `output/quiz.html` — arquivo HTML unico, autocontido e funcional

---

## Arquitetura do HTML

O arquivo gerado segue esta estrutura:

```
<head>
  Google Fonts + CSS variables do design system + estilos base
</head>
<body>
  <div id="progresso-wrapper"> <!-- barra de progresso fixa no topo -->
  <div id="tela-1" class="tela ativa"> ... </div>
  <div id="tela-2a" class="tela"> ... </div>
  <div id="tela-2b" class="tela"> ... </div>
  ... demais telas ...
  <script> logica de navegacao + state + variaveis dinamicas </script>
</body>
```

**Regra**: cada tela e um `<div class="tela">` com `display: none` por padrao. So a tela ativa tem `display: flex`. A navegacao e feita por JavaScript puro — sem frameworks.

---

## PASSO 1: Ler os dados

1. Ler `output/quiz.json` — extrair `meta` e array `telas`
2. Ler `output/design-tokens.json` — extrair `paleta`, `tipografia`, `componentes`
3. Verificar se `output/sheets-tracking.js` existe — se sim, ler o conteudo completo e guardar para embedar inline no HTML (variavel interna: `conteudo_tracking`). Se nao existir, `conteudo_tracking = null`

---

## PASSO 2: Montar o `<head>`

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
  <title>[produto.nome] — Quiz</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="[tipografia.google_fonts_url]" rel="stylesheet">
  <style>
    /* === DESIGN TOKENS === */
    :root {
      --cor-primaria: [paleta.cor_primaria];
      --cor-primaria-dark: [paleta.cor_primaria_dark];
      --cor-primaria-light: [paleta.cor_primaria_light];
      --cor-acento: [paleta.cor_acento];
      --cor-fundo: [paleta.cor_fundo];
      --cor-fundo-card: [paleta.cor_fundo_card];
      --cor-borda: [paleta.cor_borda];
      --cor-borda-ativa: [paleta.cor_borda_ativa];
      --cor-texto: [paleta.cor_texto];
      --cor-texto-suave: [paleta.cor_texto_suave];
      --cor-progresso-bg: [paleta.cor_progresso_bg];
      --fonte-titulo: '[tipografia.fonte_titulo.family]', sans-serif;
      --fonte-corpo: '[tipografia.fonte_corpo.family]', sans-serif;
      --botao-raio: [componentes.botao_raio];
      --card-raio: [componentes.card_raio];
      --input-raio: [componentes.input_raio];
      --progresso-raio: [componentes.progresso_raio];
      --progresso-altura: [componentes.progresso_altura];
    }

    /* === RESET === */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html, body { height: 100%; }
    body {
      font-family: var(--fonte-corpo);
      background: var(--cor-fundo);
      color: var(--cor-texto);
      -webkit-font-smoothing: antialiased;
    }

    /* === BARRA DE PROGRESSO === */
    #progresso-wrapper {
      position: fixed;
      top: 0; left: 0;
      width: 100%;
      height: var(--progresso-altura);
      background: var(--cor-progresso-bg);
      z-index: 1000;
    }
    #progresso-fill {
      height: 100%;
      width: 0%;
      background: var(--cor-primaria);
      border-radius: var(--progresso-raio);
      transition: width 0.4s ease;
    }

    /* === TELAS === */
    .tela {
      display: none;
      flex-direction: column;
      min-height: 100vh;
      padding: calc(var(--progresso-altura) + 20px) 20px 100px;
      max-width: 480px;
      margin: 0 auto;
    }
    .tela.ativa { display: flex; }

    /* === TIPOGRAFIA === */
    .quiz-titulo {
      font-family: var(--fonte-titulo);
      font-size: 22px;
      font-weight: [tipografia.fonte_titulo.weight_bold];
      color: var(--cor-texto);
      line-height: 1.3;
      margin-bottom: 8px;
    }
    .quiz-subtitulo {
      font-size: 15px;
      color: var(--cor-texto-suave);
      line-height: 1.5;
      margin-bottom: 24px;
    }
    .quiz-corpo {
      font-size: 15px;
      color: var(--cor-texto);
      line-height: 1.6;
      margin-bottom: 24px;
    }

    /* === IMAGEM === */
    .quiz-imagem {
      width: 100%;
      border-radius: var(--card-raio);
      margin-bottom: 20px;
      object-fit: cover;
      max-height: 240px;
    }
    .quiz-imagem-full {
      width: 100%;
      border-radius: var(--card-raio);
      margin-bottom: 12px;
      object-fit: cover;
    }

    /* === CARDS DE OPCAO (escolha-unica e multipla-escolha) === */
    .opcoes { display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px; }

    .card-opcao {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 14px 16px;
      background: var(--cor-fundo-card);
      border: 2px solid var(--cor-borda);
      border-radius: var(--card-raio);
      cursor: pointer;
      transition: border-color 0.15s, background 0.15s;
      -webkit-tap-highlight-color: transparent;
    }
    .card-opcao:hover { border-color: var(--cor-primaria); }
    .card-opcao.selecionado {
      background: var(--cor-primaria-light);
      border-color: var(--cor-borda-ativa);
    }

    .card-letra {
      min-width: 28px;
      height: 28px;
      border-radius: 50%;
      background: var(--cor-primaria-light);
      color: var(--cor-primaria);
      font-family: var(--fonte-titulo);
      font-weight: 700;
      font-size: 13px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .card-opcao.selecionado .card-letra {
      background: var(--cor-primaria);
      color: white;
    }
    .card-texto { font-size: 15px; color: var(--cor-texto); line-height: 1.4; }

    /* Checkbox para multipla-escolha */
    .card-checkbox {
      min-width: 20px;
      height: 20px;
      border-radius: 5px;
      border: 2px solid var(--cor-borda);
      display: flex;
      align-items: center;
      justify-content: center;
      transition: border-color 0.15s, background 0.15s;
    }
    .card-opcao.selecionado .card-checkbox {
      background: var(--cor-primaria);
      border-color: var(--cor-primaria);
    }
    .card-checkbox::after {
      content: '';
      display: none;
      width: 5px;
      height: 9px;
      border: 2px solid white;
      border-top: none;
      border-left: none;
      transform: rotate(45deg) translateY(-1px);
    }
    .card-opcao.selecionado .card-checkbox::after { display: block; }

    /* === BOTAO CTA === */
    .btn-continuar {
      width: 100%;
      padding: 16px;
      background: var(--cor-primaria);
      color: white;
      border: none;
      border-radius: var(--botao-raio);
      font-family: var(--fonte-titulo);
      font-size: 16px;
      font-weight: 700;
      cursor: pointer;
      transition: background 0.15s, opacity 0.15s;
      position: fixed;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%);
      max-width: 480px;
      z-index: 100;
    }
    .btn-continuar:hover { background: var(--cor-primaria-dark); }
    .btn-continuar:disabled { opacity: 0.4; cursor: not-allowed; }

    /* Botao inline (para telas de apresentacao sem fixed) */
    .btn-inline {
      width: 100%;
      padding: 16px;
      background: var(--cor-primaria);
      color: white;
      border: none;
      border-radius: var(--botao-raio);
      font-family: var(--fonte-titulo);
      font-size: 16px;
      font-weight: 700;
      cursor: pointer;
      transition: background 0.15s;
      margin-top: auto;
    }
    .btn-inline:hover { background: var(--cor-primaria-dark); }

    /* === GENERO (tela 1 — escolha de genero) === */
    .genero-opcoes { display: flex; gap: 12px; margin-bottom: 24px; }
    .genero-card {
      flex: 1;
      padding: 20px 12px;
      background: var(--cor-fundo-card);
      border: 2px solid var(--cor-borda);
      border-radius: var(--card-raio);
      cursor: pointer;
      text-align: center;
      font-family: var(--fonte-titulo);
      font-weight: 700;
      font-size: 15px;
      transition: border-color 0.15s, background 0.15s;
    }
    .genero-card:hover {
      border-color: var(--cor-primaria);
      background: var(--cor-primaria-light);
    }

    /* === LOADING (tela 16) === */
    .loading-items { display: flex; flex-direction: column; gap: 16px; margin: 24px 0; }
    .loading-item {
      display: flex;
      align-items: center;
      gap: 12px;
      font-size: 15px;
      color: var(--cor-texto-suave);
      transition: color 0.3s;
    }
    .loading-item.concluido { color: var(--cor-texto); }
    .loading-icone {
      width: 24px;
      height: 24px;
      border-radius: 50%;
      border: 2px solid var(--cor-borda);
      display: flex;
      align-items: center;
      justify-content: center;
      flex-shrink: 0;
      transition: all 0.3s;
    }
    .loading-item.concluido .loading-icone {
      background: var(--cor-primaria);
      border-color: var(--cor-primaria);
    }
    .loading-item.concluido .loading-icone::after {
      content: '';
      width: 5px;
      height: 9px;
      border: 2px solid white;
      border-top: none;
      border-left: none;
      transform: rotate(45deg) translateY(-1px);
    }
    .loading-item.em-andamento .loading-icone {
      border-color: var(--cor-primaria);
      animation: pulsar 1s infinite;
    }
    @keyframes pulsar {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.4; }
    }
    .loading-barra-wrapper {
      width: 100%;
      height: 8px;
      background: var(--cor-progresso-bg);
      border-radius: 50px;
      margin-top: 16px;
      overflow: hidden;
    }
    .loading-barra-fill {
      height: 100%;
      width: 0%;
      background: var(--cor-primaria);
      border-radius: 50px;
      transition: width 0.3s ease;
    }

    /* === ANTES & DEPOIS COM TERMOMETRO (tela 17) === */
    .antes-depois { display: flex; gap: 16px; margin: 24px 0; }
    .antes-depois-col { flex: 1; text-align: center; }
    .antes-depois-badge {
      display: inline-block;
      background: #222;
      color: #fff;
      font-size: 12px;
      font-weight: 700;
      padding: 4px 12px;
      border-radius: 50px;
      margin-bottom: 8px;
    }
    .antes-depois-img {
      width: 100%;
      aspect-ratio: 1;
      object-fit: cover;
      border-radius: var(--card-raio);
      margin-bottom: 12px;
    }
    .metrica { margin-bottom: 14px; text-align: left; }
    .metrica-nome { font-weight: 700; font-size: 14px; color: var(--cor-texto); }
    .metrica-row { display: flex; justify-content: space-between; align-items: center; margin-top: 2px; }
    .metrica-adj { font-size: 13px; color: var(--cor-texto-suave); }
    .metrica-pct { font-size: 14px; font-weight: 700; color: var(--cor-texto); }
    .termometro { display: flex; gap: 4px; margin-top: 6px; }
    .termometro-seg {
      flex: 1;
      height: 10px;
      border-radius: 50px;
      background: #E0E0E0;
    }
    .termometro-seg.t1 { background: #E63946; }
    .termometro-seg.t2 { background: #F4720B; }
    .termometro-seg.t3 { background: #F4C430; }
    .termometro-seg.t4 { background: #90BE6D; }
    .termometro-seg.t5 { background: #43AA8B; }

    /* === PAGINA DE VENDAS (tela 18) === */
    .vendas-bloco { margin-bottom: 28px; }
    .vendas-titulo { font-family: var(--fonte-titulo); font-size: 18px; font-weight: 700; margin-bottom: 12px; }
    .comparativo { display: flex; gap: 12px; margin-bottom: 20px; }
    .comparativo-col {
      flex: 1;
      padding: 14px;
      border-radius: var(--card-raio);
      font-size: 13px;
      line-height: 1.6;
    }
    .comparativo-col.sem { background: #FFF0F0; border: 1px solid #FFCCCC; }
    .comparativo-col.com { background: #F0FFF4; border: 1px solid #CCEECC; }
    .comparativo-col strong { display: block; font-size: 13px; font-weight: 700; margin-bottom: 8px; }
    .comparativo-col li { margin-left: 14px; margin-bottom: 4px; }
    .preco-destaque {
      text-align: center;
      font-family: var(--fonte-titulo);
      font-size: 32px;
      font-weight: 900;
      color: var(--cor-primaria);
      margin: 16px 0;
    }
    .garantia {
      background: var(--cor-fundo-card);
      border: 1px solid var(--cor-borda);
      border-radius: var(--card-raio);
      padding: 16px;
      font-size: 14px;
      color: var(--cor-texto-suave);
      text-align: center;
      margin-top: 20px;
    }
  </style>
  [se conteudo_tracking != null]
  <script>[conteudo completo do sheets-tracking.js aqui inline — copiar exatamente o conteudo lido no PASSO 1]</script>
</head>
```

---

## PASSO 3: Gerar cada tela como `<div class="tela">`

Para cada tela em `quiz.json.telas`, gerar o HTML correspondente com base no `tipo`:

---

### Tipo: `apresentacao`

```html
<div id="tela-[ID]" class="tela [primeiro ? 'ativa' : '']">
  [se tiver imagem]
  <img src="[imagem_url ou imagem_sugerida como alt]" alt="[descricao]" class="quiz-imagem">

  <h1 class="quiz-titulo">[titulo]</h1>
  [se tiver subtitulo]
  <p class="quiz-subtitulo">[subtitulo]</p>
  [se tiver corpo]
  <div class="quiz-corpo">[corpo em HTML — listas viram <ul><li>, negrito vira <strong>]</div>

  <button class="btn-inline" onclick="if(window.enviarParaSheets)enviarParaSheets({tela:'[ID]',evento:'avancou'}); irPara('[proxima_tela]')">[texto_botao]</button>
</div>
```

**Excecao — Tela 1 com escolha de genero** (quando `meta.incluir_genero = true`):
```html
<div id="tela-1" class="tela ativa">
  [imagem se houver]
  <h1 class="quiz-titulo">[titulo]</h1>
  <p class="quiz-subtitulo">[subtitulo]</p>
  <div class="genero-opcoes">
    <div class="genero-card" onclick="if(window.enviarParaSheets)enviarParaSheets({tela:'1',resposta:'Masculino'}); irPara('2a')">Masculino</div>
    <div class="genero-card" onclick="if(window.enviarParaSheets)enviarParaSheets({tela:'1',resposta:'Feminino'}); irPara('2b')">Feminino</div>
  </div>
</div>
```

---

### Tipo: `escolha-unica`

```html
<div id="tela-[ID]" class="tela">
  <h1 class="quiz-titulo">[titulo]</h1>
  [subtitulo se houver]
  <div class="opcoes" id="opcoes-[ID]">
    <div class="card-opcao" onclick="selecionarUnica(this, '[ID]', 'A', '[proxima_tela]')">
      <div class="card-letra">A</div>
      <span class="card-texto">[opcao_1]</span>
    </div>
    <div class="card-opcao" onclick="selecionarUnica(this, '[ID]', 'B', '[proxima_tela]')">
      <div class="card-letra">B</div>
      <span class="card-texto">[opcao_2]</span>
    </div>
    <!-- ... demais opcoes -->
  </div>
</div>
```

**Regra**: `selecionarUnica` marca a opcao e navega automaticamente para a proxima tela apos 300ms.

**Excecao — Tela 13** (objetivo especifico que alimenta tela 14):
```html
onclick="selecionarObjetivo(this, '[texto_opcao]')"
```

---

### Tipo: `multipla-escolha`

```html
<div id="tela-[ID]" class="tela">
  <h1 class="quiz-titulo">[titulo]</h1>
  <p class="quiz-subtitulo">[subtitulo]</p>
  <div class="opcoes" id="opcoes-[ID]">
    <div class="card-opcao" onclick="toggleMultipla(this, '[ID]')">
      <div class="card-checkbox"></div>
      <span class="card-texto">[opcao_1]</span>
    </div>
    <!-- ... demais opcoes -->
  </div>
  <button class="btn-continuar" id="btn-[ID]" onclick="irParaMultipla('[ID]','[proxima_tela]')" disabled>Continuar</button>
</div>
```

**Regra**: botao "Continuar" comeca desabilitado. `toggleMultipla` habilita o botao quando pelo menos 1 opcao e selecionada.

---

### Tipo: `carregamento`

```html
<div id="tela-16" class="tela">
  <h1 class="quiz-titulo">[titulo]</h1>
  <p class="quiz-subtitulo">[subtitulo]</p>
  <div class="loading-items">
    <div class="loading-item" id="li-0">
      <div class="loading-icone"></div>
      <span>[item_1]</span>
    </div>
    <div class="loading-item" id="li-1">
      <div class="loading-icone"></div>
      <span>[item_2]</span>
    </div>
    <div class="loading-item" id="li-2">
      <div class="loading-icone"></div>
      <span>[item_3]</span>
    </div>
    <div class="loading-item" id="li-3">
      <div class="loading-icone"></div>
      <span>[item_4]</span>
    </div>
  </div>
  <div class="loading-barra-wrapper">
    <div class="loading-barra-fill" id="loading-barra"></div>
  </div>
</div>
```

---

### Tela 14 — Promessa Personalizada

O corpo da tela 14 deve ter um `<span id="resposta-tela-13"></span>` onde o texto dinamico sera injetado:

```html
<div id="tela-14" class="tela">
  <h1 class="quiz-titulo">Nos iremos te ajudar!</h1>
  <p class="quiz-corpo">
    Voce encontrara no [PRODUTO], exercicios para
    <strong id="resposta-tela-13"></strong>
    sem [AMPLIFICAR_OPORTUNIDADE]
  </p>
  [imagem se houver]
  <button class="btn-inline" onclick="if(window.enviarParaSheets)enviarParaSheets({tela:'14',evento:'avancou'}); irPara('15')">Ver meu plano personalizado</button>
</div>
```

---

### Tela 17 — Resultado com Antes & Depois (OBRIGATORIO)

A Tela 17 DEVE incluir o componente Antes & Depois com termometro de 5 segmentos. Este componente e obrigatorio e nao pode ser substituido por texto simples.

As metricas devem ser baseadas no tema do produto (extraidas de `dados-quiz.json`). Gerar 2 metricas relevantes ao nicho com valores baixos no "Antes" e altos no "Depois".

```html
<div id="tela-17" class="tela">
  <h1 class="quiz-titulo">[titulo — ex: Seu perfil esta pronto!]</h1>
  <p class="quiz-subtitulo">[subtitulo — ex: Veja como voce pode evoluir com o [PRODUTO]]</p>

  <div class="antes-depois">
    <!-- Coluna Antes -->
    <div class="antes-depois-col">
      <span class="antes-depois-badge">Antes</span>
      <img src="[URL_imagem_antes ou vazio]" alt="Antes" class="antes-depois-img" style="[display:none se sem URL]">
      <div class="metrica">
        <div class="metrica-nome">[Metrica 1 — ex: Velocidade de leitura]</div>
        <div class="metrica-row">
          <span class="metrica-adj">[adjetivo baixo — ex: Baixa]</span>
          <span class="metrica-pct">[valor baixo — ex: 22%]</span>
        </div>
        <div class="termometro">
          <div class="termometro-seg t1"></div>
          <div class="termometro-seg"></div>
          <div class="termometro-seg"></div>
          <div class="termometro-seg"></div>
          <div class="termometro-seg"></div>
        </div>
      </div>
      <div class="metrica">
        <div class="metrica-nome">[Metrica 2 — ex: Foco e Compreensao]</div>
        <div class="metrica-row">
          <span class="metrica-adj">[adjetivo baixo — ex: Fraco]</span>
          <span class="metrica-pct">[valor baixo — ex: 24%]</span>
        </div>
        <div class="termometro">
          <div class="termometro-seg t1"></div>
          <div class="termometro-seg"></div>
          <div class="termometro-seg"></div>
          <div class="termometro-seg"></div>
          <div class="termometro-seg"></div>
        </div>
      </div>
    </div>

    <!-- Coluna Depois -->
    <div class="antes-depois-col">
      <span class="antes-depois-badge">Depois</span>
      <img src="[URL_imagem_depois ou vazio]" alt="Depois" class="antes-depois-img" style="[display:none se sem URL]">
      <div class="metrica">
        <div class="metrica-nome">[Metrica 1 — mesma do Antes]</div>
        <div class="metrica-row">
          <span class="metrica-adj">[adjetivo alto — ex: Rapida]</span>
          <span class="metrica-pct">[valor alto — ex: 96%]</span>
        </div>
        <div class="termometro">
          <div class="termometro-seg t1"></div>
          <div class="termometro-seg t2"></div>
          <div class="termometro-seg t3"></div>
          <div class="termometro-seg t4"></div>
          <div class="termometro-seg t5"></div>
        </div>
      </div>
      <div class="metrica">
        <div class="metrica-nome">[Metrica 2 — mesma do Antes]</div>
        <div class="metrica-row">
          <span class="metrica-adj">[adjetivo alto — ex: Forte]</span>
          <span class="metrica-pct">[valor alto — ex: 92%]</span>
        </div>
        <div class="termometro">
          <div class="termometro-seg t1"></div>
          <div class="termometro-seg t2"></div>
          <div class="termometro-seg t3"></div>
          <div class="termometro-seg t4"></div>
          <div class="termometro-seg t5"></div>
        </div>
      </div>
    </div>
  </div>

  <button class="btn-inline" onclick="if(window.enviarParaSheets)enviarParaSheets({tela:'17',evento:'avancou'}); irPara('18')">[texto_botao — ex: Ver meu plano completo]</button>
</div>
```

**Regras do termometro**:
- **Antes**: apenas 1 segmento preenchido (classe `t1` = vermelho), os 4 restantes sem classe (cinza `#E0E0E0`)
- **Depois**: todos os 5 segmentos preenchidos (`t1` a `t5` = vermelho → laranja → amarelo → verde claro → verde)
- As metricas devem ser relevantes ao tema do produto (nao usar exemplos de leitura rapida se o tema for outro)
- Valores do "Antes": entre 18% e 28% | Valores do "Depois": entre 88% e 98%

---

### Tela 18 — Pagina de Vendas

Gerar os 8 blocos em HTML usando classes `.vendas-bloco`:

```html
<div id="tela-18" class="tela">
  <!-- Bloco 1: Comparativo -->
  <div class="vendas-bloco">
    <div class="comparativo">
      <div class="comparativo-col sem">
        <strong>Sem [PRODUTO]</strong>
        <ul>[lista das dores/consequencias]</ul>
      </div>
      <div class="comparativo-col com">
        <strong>Com [PRODUTO]</strong>
        <ul>[lista dos desejos/resultados]</ul>
      </div>
    </div>
  </div>

  <!-- Bloco 2: Headline -->
  <div class="vendas-bloco">
    <h1 class="quiz-titulo">O seu plano personalizado esta pronto!</h1>
    <p class="quiz-subtitulo">Com base nas suas respostas, criamos o caminho mais rapido para voce [TRANSFORMACAO_PRINCIPAL]</p>
  </div>

  <!-- Bloco 3: O que voce recebe -->
  <div class="vendas-bloco">
    <p class="vendas-titulo">O que voce recebe:</p>
    [lista de modulos/entregaveis com valores]
    <p style="font-weight:700; margin-top:12px;">Total: [VALOR_TOTAL] — Apenas:</p>
  </div>

  <!-- Bloco 4: Preco + CTA -->
  <div class="vendas-bloco" style="text-align:center">
    <div class="preco-destaque">[PRECO]</div>
    <a href="[LINK_CHECKOUT]" style="text-decoration:none" onclick="if(window.enviarParaSheets)enviarParaSheets({evento:'clicou_checkout'})">
      <button class="btn-inline">Quero comecar agora</button>
    </a>
    <p style="font-size:13px; color:var(--cor-texto-suave); margin-top:8px">&#128274; Compra 100% segura</p>
    <p style="font-size:13px; color:var(--cor-texto-suave); margin-top:4px">Acesso imediato apos a compra</p>
  </div>

  <!-- Bloco 5: Comparativo de vida -->
  <div class="vendas-bloco">
    [texto descritivo: vida sem vs com o produto]
  </div>

  <!-- Bloco 6: Depoimentos -->
  <div class="vendas-bloco">
    [para cada depoimento com imagem_url: <img src="[url]" class="quiz-imagem-full" alt="Depoimento">]
    [para cada depoimento sem url: placeholder [INSERIR IMAGEM DO DEPOIMENTO]]
  </div>

  <!-- Bloco 7: CTA final -->
  <div class="vendas-bloco" style="text-align:center">
    <a href="[LINK_CHECKOUT]" style="text-decoration:none" onclick="if(window.enviarParaSheets)enviarParaSheets({evento:'clicou_checkout'})">
      <button class="btn-inline">Quero garantir meu acesso agora</button>
    </a>
    <p style="font-size:13px; color:var(--cor-texto-suave); margin-top:8px">&#128274; Compra 100% segura</p>
  </div>

  <!-- Bloco 8: Garantia -->
  <div class="garantia">
    Garantia de 7 dias: Se por qualquer motivo voce nao estiver satisfeito, devolvemos 100% do seu investimento, sem perguntas.
  </div>
</div>
```

---

## PASSO 4: Gerar o `<script>`

O script deve conter toda a logica de navegacao, estado e variaveis dinamicas:

```javascript
<script>
// === STATE ===
const quiz = {
  respostaTela13: '',
  progressos: {
    // preencher com os valores de progresso de cada tela do quiz.json
    '1': 0,
    '2a': [N], '2b': [N],
    '3': [N],
    // ... valor calculado para cada tela
  }
};

// === NAVEGACAO ===
function irPara(telaId) {
  const atual = document.querySelector('.tela.ativa');
  if (atual) atual.classList.remove('ativa');

  const proxima = document.getElementById('tela-' + telaId);
  if (!proxima) return;
  proxima.classList.add('ativa');

  // Registrar chegada na tela (tracking universal)
  // Para telas interativas, o valor 'visualizou' sera sobrescrito pela resposta real do usuario
  if (window.enviarParaSheets) enviarParaSheets({ tela: telaId, evento: 'visualizou' });

  // Atualizar tela_atual na planilha
  // (ja feito pelo servidor ao receber qualquer evento com campo 'tela')

  // Atualizar barra de progresso
  const pct = quiz.progressos[telaId] || 0;
  document.getElementById('progresso-fill').style.width = pct + '%';

  // Rolar para o topo
  window.scrollTo({ top: 0, behavior: 'smooth' });

  // Disparar loading se for tela 16
  if (telaId === '16') iniciarLoading();
}

// === ESCOLHA UNICA ===
function selecionarUnica(card, telaId, letra, proximaTelaId) {
  document.querySelectorAll('#tela-' + telaId + ' .card-opcao').forEach(c => c.classList.remove('selecionado'));
  card.classList.add('selecionado');
  const texto = card.querySelector('.card-texto')?.textContent || letra;
  if (window.enviarParaSheets) enviarParaSheets({ tela: telaId, resposta: texto });
  setTimeout(() => irPara(proximaTelaId), 350);
}

// === TELA 13: SALVAR OBJETIVO ESPECIFICO ===
function selecionarObjetivo(card, texto) {
  quiz.respostaTela13 = texto;
  const span = document.getElementById('resposta-tela-13');
  if (span) span.textContent = texto;
  document.querySelectorAll('#tela-13 .card-opcao').forEach(c => c.classList.remove('selecionado'));
  card.classList.add('selecionado');
  if (window.enviarParaSheets) enviarParaSheets({ tela: '13', resposta: texto });
  setTimeout(() => irPara('14'), 350);
}

// === MULTIPLA ESCOLHA: NAVEGAR E ENVIAR RESPOSTAS ===
function irParaMultipla(telaId, proximaTelaId) {
  const selecionados = Array.from(
    document.querySelectorAll('#tela-' + telaId + ' .card-opcao.selecionado .card-texto')
  ).map(el => el.textContent);
  if (window.enviarParaSheets) enviarParaSheets({ tela: telaId, respostas: selecionados });
  irPara(proximaTelaId);
}

// === MULTIPLA ESCOLHA: TOGGLE ===
function toggleMultipla(card, telaId) {
  card.classList.toggle('selecionado');
  // Habilitar/desabilitar botao
  const algumSelecionado = document.querySelectorAll('#tela-' + telaId + ' .card-opcao.selecionado').length > 0;
  const btn = document.getElementById('btn-' + telaId);
  if (btn) btn.disabled = !algumSelecionado;
}

// === LOADING SCREEN ===
function iniciarLoading() {
  const items = document.querySelectorAll('#tela-16 .loading-item');
  const barra = document.getElementById('loading-barra');
  let step = 0;
  let progresso = 0;

  // Animacao dos itens do checklist
  const itemInterval = setInterval(() => {
    if (step > 0) items[step - 1].classList.remove('em-andamento');
    if (step < items.length) {
      items[step].classList.add('em-andamento');
      step++;
    } else {
      clearInterval(itemInterval);
      // Marcar todos como concluidos
      items.forEach(i => { i.classList.remove('em-andamento'); i.classList.add('concluido'); });
    }
  }, 1400);

  // Animacao da barra
  const barraInterval = setInterval(() => {
    progresso += 2;
    if (barra) barra.style.width = Math.min(progresso, 100) + '%';
    if (progresso >= 100) {
      clearInterval(barraInterval);
      if (window.enviarParaSheets) enviarParaSheets({ tela: '16', evento: 'avancou' });
      setTimeout(() => irPara('[proxima_tela_apos_loading]'), 500); // normalmente tela 17
    }
  }, 110);
}

// === INIT: mostrar tela 1 ===
document.addEventListener('DOMContentLoaded', () => {
  irPara('1');
});
</script>
```

---

## PASSO 5: Regras de Geracao

### R1: IDs das telas
- Usar IDs em lowercase e com hifen: `tela-1`, `tela-2a`, `tela-2b`, `tela-3`, ...
- Telas condicionais (11B): `tela-11b`

### R2: Imagens
- Se `imagens-quiz.json` tiver URL para a tela: usar no `src` da tag `<img>`
- Se nao tiver: gerar `<img src="" alt="[descricao da imagem sugerida]" class="quiz-imagem" style="display:none">` com `display:none` — o usuario adiciona a URL depois
- Nunca quebrar o layout por imagem faltando

### R3: Links de checkout
- Se `link_checkout` estiver preenchido: usar no `href` dos botoes da tela 18
- Se for `[INSERIR LINK]`: usar `href="#"` com comentario HTML `<!-- INSERIR LINK DE CHECKOUT AQUI -->`

### R4: Telas condicionais
- Se `meta.incluir_genero = false`: nao gerar telas 2A/2B, tela 1 nao tem escolha de genero
- Se `meta.telas_depoimentos = 0`: nao gerar telas 11 e 12
- O mapa de progressos no JavaScript deve refletir apenas as telas existentes

### R5: Texto longo (corpo da tela 18)
- Quebrar paragrafos em `<p>` separados
- Listas viram `<ul><li>`
- **negrito** vira `<strong>`

### R6: Responsividade
- `max-width: 480px` centrado na tela — funciona bem em mobile e desktop
- Botao CTA `position: fixed; bottom: 0` para telas de escolha
- Botao inline `.btn-inline` para telas de apresentacao

### R7: Zero emojis
- Proibido qualquer emoji no HTML gerado — nem em titulos, nem em opcoes, nem como icones decorativos
- Isso inclui especialmente os emojis classicos de IA: 🎯 ⚡ 📊 ✨ 🚀 💡 🔥 ⭐ e similares
- Usar apenas tipografia, cores e layout para criar hierarquia visual

### R8: Nao incluir analytics de terceiros, cookies ou dependencias externas
- Apenas Google Fonts e o proprio HTML/CSS/JS
- Excecao: o `sheets-tracking.js` gerado pelo plugin deve ser embedado inline se existir (`conteudo_tracking != null`)
- Arquivo autocontido — funciona ao abrir localmente no navegador

---

## PASSO 6: Salvar e retornar

Salvar o HTML completo em `output/quiz.html`.

Seguir imediatamente para o PASSO 3 do comando `/criar-quiz` — o card do `output/quiz.html` e a oferta de abrir no Chrome sao exibidos la.

---

## Procedimento

1. Ler `output/quiz.json`, `output/design-tokens.json`, `output/imagens-quiz.json`
2. Montar o `<head>` com CSS variables e todos os estilos
3. Gerar cada tela como `<div class="tela">` baseado no tipo
4. Gerar o `<script>` com toda a logica
5. Fechar `</body></html>`
6. Salvar em `output/quiz.html`
7. Retornar resumo ao chat
