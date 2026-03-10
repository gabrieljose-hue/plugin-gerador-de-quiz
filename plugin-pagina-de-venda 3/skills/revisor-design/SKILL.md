---
name: revisor-design
description: Designer sênior que revisa a página para garantir identidade visual alinhada, não parece IA, e está adequada ao nicho/público
---

# Skill: Revisor de Design (Designer Sênior)

## Missão

Revisar o design system gerado (DESIGN_TOKENS + design-preview.html) e validar se **não parece IA genérico**, está **alinhado com o nicho/produto**, e tem **identidade visual própria e profissional**.

---

## Entrada (INPUT)

**Leia os arquivos (não são fornecidos no chat)**:
- `output/design-tokens.json` — JSON de design system (Passo 4)
- `output/design-preview.html` — Preview visual do design (Passo 4)
- `output/copy-estruturada.json` — Copy (para validar alinhamento, Passo 3)

**Receba do chat**:
- `NOME_PRODUTO` e `COR_PRINCIPAL` — Contexto das escolhas
- Contexto de nicho/público

---

## Saída (OUTPUT)

**Retornar ao chat APENAS o status e lista de pontos** (não re-emita os arquivos):

**Opção A: APROVADO** ✅
```
✅ DESIGN APROVADO

Análise:
✓ Identidade visual clara e alinhada com o nicho
✓ Paleta de cores diferenciada (não genérica)
✓ Tipografia personalizada e legível
✓ Não parece template de IA
✓ Layout profissional e bem organizado
✓ 3i's do Marketing refletidos visualmente

Pronto para construir o HTML (Passo 5a)!
```

**Opção B: AJUSTES NECESSÁRIOS** 🔧
```
🔧 DESIGN REQUER AJUSTES

Problemas encontrados:

1. Paleta de cores é genérica...
   → Sugestão: [específica]

2. Tipografia muito padrão...
   → Sugestão: [específica]

[mais pontos...]

Volte para skill designer-html com essas sugestões.
```

**Opção C: REJEIÇÃO** ❌
```
❌ DESIGN REJEITADO

Problemas críticos:
1. [problema crítico 1]
2. [problema crítico 2]

Recomendo revisão completa do design system.
```

**NÃO re-emita os arquivos** (`design-preview.html` ou `design-tokens.json`) — eles já foram salvos em Passo 4.

---

## Checklist de Revisão (CRÍTICO)

### 0. ESTRUTURA BÁSICA DO HERO (CRÍTICO)

Verificar se o hero segue os padrões obrigatórios:

- [ ] ❌ **SEM navbar/header** — Página deve estar limpa e focada (não há navbar sticky)
- [ ] ✅ **Logo centralizada no topo** (se `LOGO_URL` preenchida)
  - Margem: 20px do topo
  - Max-width: ~150px desktop, ~120px mobile
  - **Se vazio**: Omitir (não mostrar placeholder)
- [ ] ✅ **Headline centralizada**
  - Alinhamento: `text-align: center`
  - Font: `titulo` com weight 900
  - Tamanho: 48px mobile, 72px desktop
- [ ] ✅ **Subheadline centralizada**
  - Alinhamento: `text-align: center`
  - Font: `subtitulo`
  - Tamanho: 20px mobile, 24px desktop
- [ ] ✅ **Urgência/Escassez** (se preenchida)
  - Centralizada abaixo da subheadline
  - Cor: `accent` ou destaque
  - **Se vazio**: Omitir
- [ ] ✅ **Vídeo de Vendas** (se `VIDEO_URL` preenchida)
  - **DEVE estar APÓS os bullet points** (não embaixo da subheadline)
  - Centralizado
  - Iframe 16:9 responsivo
  - Max-width: 800px desktop, 100% mobile
  - **Se vazio**: Omitir completamente (não mostrar placeholder)

**Status**: Correto / Precisa ajuste

---

### 1. 3i's DO MARKETING REFLETIDOS VISUALMENTE

Verificar se o design comunica visualmente os 3i's:

- [ ] **Identidade do Comunicador** (como ele quer ser visto):
  - Se "especialista confiável" → design sóbrio, autoridade visual?
  - Se "mentor amigável" → cores quentes, tipografia approachable?
  - Se "inovador" → design moderno, cores ousadas?

- [ ] **Identidade do Consumidor** (aspiracional, após transformação):
  - Visual transmite o estado que o cliente deseja alcançar?
  - Cores e tipografia comunicam a aspiração?

- [ ] **Identidade do Produto** (o que representa):
  - Design reflete o posicionamento do produto?
  - Cores/elementos conectam com a promessa?

**Status**: Alinhado / Parcialmente alinhado / Desalinhado

---

### 2. BIG IDEA VISUAL (NOVO!)

Verificar se existe um elemento visual único que encarne a sacada criativa:

- [ ] Design tem um elemento memorável/distintivo?
- [ ] Este elemento aparece repetido (coerência)?
- [ ] O elemento comunica a essência do produto?
- [ ] Não é genérico (ícone stock ou padrão)?

**Exemplos**:
- Elemento único que marca: gradiente exclusivo, padrão próprio, detalhe que só este site tem
- Não é bom: template genérico onde qualquer site poderia usar

**Status**: Tem Big Idea Visual clara / Fraco / Não tem

---

### 3. IDENTIDADE VISUAL vs. NICHO

Verificar se o design **combina com o tipo de produto**:

#### Se é curso/educação:
- [ ] Design transmite profissionalismo? (não é genérico)
- [ ] Cores sugerem confiança/crescimento?
- [ ] Tipografia é elegante e legível?
- [ ] Layout sugere estrutura/organização?
- **Exemplo**: Tech → azul/cinza | Mindfulness → verde/warm | Negócio → preto/ouro

#### Se é produto digital/SaaS:
- [ ] Design moderno mas não futurista demais?
- [ ] Cores neutras ou brand colors fortes?
- [ ] Interface parece intuitiva?
- [ ] Não parece sci-fi ou robótico?

#### Se é coaching/mentoria:
- [ ] Design humanizado (não corporativo demais)?
- [ ] Cores quentes ou inspiradoras?
- [ ] Tipografia amigável mas profissional?
- [ ] Espaço para fotos de pessoas?

#### Se é fitness/lifestyle:
- [ ] Design dinâmico e energético?
- [ ] Cores vibrantes alinhadas?
- [ ] Fotos/imagens sugerem movimento?
- [ ] Layout ativa a sensação de ação?

---

### 2. "NÃO PARECE IA" — Verificação de Genericidade

Marque como ❌ REJEITADO se o design:

- [ ] ❌ Parece um template genérico de Wix/Webflow?
- [ ] ❌ Usa padrão "hero + 3 cards + footer" sem personalização?
- [ ] ❌ Cores são combinação óbvia (azul + branco + cinza)?
- [ ] ❌ Tipografia usa apenas fontes padrão (Arial, Helvetica)?
- [ ] ❌ Ícones parecem stock icons genéricos (sem estilo)?
- [ ] ❌ Espaçamento é muito uniforme/robótico?
- [ ] ❌ Layout segue regra "um tamanho serve para todos"?
- [ ] ❌ Sem qualquer elemento único/memorável?
- [ ] ❌ Parece feito por ferramenta automática de IA?

### Sinais de que PARECE IA:
- Tipografia muito perfeita/sem personalidade
- Cores muito saturadas ou muito flat
- Elementos muito simétricos e geométricos
- Muitos espaços em branco (falta organicidade)
- Sem variação visual (tudo igual)
- Ícones muito minimalists/genéricos
- Layout muito grid-based e previsível

### Sinais de que NÃO parece IA:
- Tipografia tem personalidade (mistura de weights, kerning desigual)
- Cores têm profundidade (degradientes, variações)
- Elementos com detalhe humano (imperfeições controladas)
- Espaço em branco inteligente (não vazio)
- Variação visual (alguns elementos maiores, outros menores)
- Ícones customizados ou stilizados
- Layout quebra grid quando necessário

---

### 3. ALINHAMENTO COM O PÚBLICO

Verificar se o design **fala visual** com o público-alvo:

#### Público-alvo: Executivos / CEOs / Alto padrão
- ✅ Design deve ser: Minimalista, sofisticado, preto/ouro/azul
- ❌ Evitar: Muitas cores, emojis, estilo "startup"

#### Público-alvo: Empreendedores jovens / Startups
- ✅ Design deve ser: Moderno, dinâmico, cores vibrantes
- ❌ Evitar: Muito corporativo, cores muito escuras

#### Público-alvo: Mães/Mulheres de 30-50 anos
- ✅ Design deve ser: Acolhedor, cores quentes, tipografia legível
- ❌ Evitar: Muito minimalist, letras muito pequenas

#### Público-alvo: Geeks / Desenvolvedores
- ✅ Design deve ser: Funcional, dark mode, tipografia monospace para código
- ❌ Evitar: Skeuomorphism, muita decoração

---

### 11. PALETA DE CORES

Verificar se cores estão **bem utilizadas**:

- [ ] Cor principal é visível e diferenciada?
- [ ] Cores não rivalizam entre si (hierarquia clara)?
- [ ] Contraste suficiente para acessibilidade?
- [ ] Cores têm significado (não aleatórias)?
- [ ] Paleta tem 3-5 cores máximo (não poluído)?
- [ ] Cores combinam com psicologia do nicho?
  - Azul = confiança/tecnologia
  - Verde = crescimento/saúde
  - Vermelho = urgência/ação
  - Ouro = premium/luxo
  - Laranja = energia/criatividade
  - Roxo = criatividade/transformação

**Status**: Excelente / Bom / Fraco / Péssimo

---

### 12. TIPOGRAFIA

Verificar se fontes estão **bem escolhidas**:

- [ ] Máximo 2-3 famílias de fontes?
- [ ] Títulos (h1, h2) são legíveis e distintos?
- [ ] Corpo do texto é confortável para ler (16px+)?
- [ ] Fontes combinam com estilo do nicho?
- [ ] Não usa apenas fontes padrão do sistema?
- [ ] Line-height adequado (1.5 mínimo)?
- [ ] Contraste suficiente (texto escuro em fundo claro)?

**Exemplo bom**: Montserrat (títulos, bold) + Open Sans (corpo)
**Exemplo ruim**: Arial (tudo) ou 5 fontes diferentes

---

### 13. LAYOUT E ESPACIAMENTO

Verificar se estrutura visual é **profissional**:

- [ ] Alinhamentos consistentes (não desalinhado)?
- [ ] Espaçamento segue hierarquia (não uniforme)?
- [ ] Margens e paddings proporcionais?
- [ ] Elementos importantes têm peso visual (tamanho/cor)?
- [ ] Layout breathing room (não apertado)?
- [ ] Grid invisível mas presente?
- [ ] Responsividade apropriada (mobile/desktop)?

**Status**: Excelente / Bom / Fraco

---

### 14. ELEMENTOS VISUAIS

Verificar se decorativos estão **alinhados**:

- [ ] Ícones têm estilo consistente (não misturados)?
- [ ] Imagens (se houver) são profissionais?
- [ ] Sem imagens genéricas de stock photo excessivas?
- [ ] Botões têm design consistente?
- [ ] Separadores (linhas, formas) têm propósito?
- [ ] Nenhum elemento desnecessário (poluição visual)?

---

### 8. SEÇÃO PALIATIVO (ENTREGÁVEL RÁPIDO) DESTACADA (NOVO!)

Verificar se o paliativo tem destaque visual apropriado:

- [ ] Paliativo tem fundo/cor diferenciado (destaque)?
- [ ] Border ou elemento visual chama atenção?
- [ ] Ícone especial diferencia do resto?
- [ ] Texto é claro: "Ganho imediato" ou similar?
- [ ] CTA secundário facilita acesso (download/preview)?
- [ ] Não é confundido com outras seções?

**Motivo**: Paliativo é o "ísca" que convence visitor a ficar. Deve ter peso visual!

**Status**: Bem destacado / Razoável / Pouco destacado

---

### 9. ARGUMENTO INCONTESTÁVEL COM DESTAQUE VISUAL (NOVO!)

Verificar se o argumento (baseado em dados) está destacado:

- [ ] Número/estatística em tamanho grande (96px+)?
- [ ] Cor primária para destaque?
- [ ] Fonte da informação citada (credibilidade)?
- [ ] Texto apoiando o número (legível)?
- [ ] Visual diferencia esta seção das outras?

**Exemplo bom**: "97%" em verde grande, com fonte citada em baixo
**Exemplo ruim**: "97% de sucesso" em texto comum, sem destaque

**Status**: Excelente destaque / Bom / Fraco

---

### 10. DIFERENCIAÇÃO VISUAL

Verificar se design é **memorável**:

- [ ] Tem algo único/distinto (não é cópia de outro site)?
- [ ] Cor principal é diferente de competitors?
- [ ] Tipografia tem personalidade?
- [ ] Um detalhe visual que "marca"?
- [ ] Não parece que foi feito pelo mesmo template que 1000 sites?

**Exemplos de diferenciação**:
- Uma cor ousada bem usada
- Tipografia customizada
- Detalhe decorativo único (não usado em templates)
- Padrão ou textura própria
- Animação ou interação única

---

### 15. CONFORMIDADE COM EXPECTATIVAS

Verificar se design **atende ao briefing**:

- [ ] Cor principal foi usada corretamente?
- [ ] Design reflete a promessa do copy?
- [ ] Não discrepa muito da expectativa inicial?
- [ ] Responde ao público-alvo esperado?

---

## Processo de Revisão

### Passo 1: Análise Visual Rápida
- Ler e analisar `output/design-preview.html` (arquivo do Passo 4)
- Impressão geral: parece bom ou parece IA/genérico?
- Reação emocional: qual sensação gera?
- **Não emita o HTML no chat** — apenas a análise

### Passo 2: Checklist Detalhado
- Passar por cada categoria acima
- Marcar aprovado/fraco/rejeitado
- Anotar exemplos específicos

### Passo 3: Teste do "Parece IA?"
```
Perguntar-se:
- Se eu visse este design em 10 sites diferentes,
  pareceria que são do mesmo template?
- Este design é memorável ou esquecível?
- A cor/tipografia/estilo é distintivo ou genérico?
```

### Passo 4: Decisão

**CENÁRIO A: APROVAÇÃO** ✅
- Todos os items críticos estão ok
- Design tem identidade própria
- Alinhado com nicho/público

```
✅ DESIGN APROVADO

Análise:
✓ Identidade visual clara e alinhada com o nicho
✓ Paleta de cores diferenciada (não genérica)
✓ Tipografia personalizada e legível
✓ Não parece template de IA
✓ Layout profissional e bem organizado
✓ Elementos visuais coerentes

Pontos fortes:
+ Cor principal (laranja) diferencia bem
+ Tipografia Montserrat + Open Sans dá personalidade
+ Espaçamento breathing room profissional
+ Ícones customizados, não genéricos

Status: Pronto para construir o HTML!
```

**CENÁRIO B: AJUSTES NECESSÁRIOS** 🔧
- Alguns items fracos, viável refazer
- Não é rejeição total

```
🔧 DESIGN REQUER AJUSTES

Problemas encontrados:

1. Paleta de cores é genérica (azul + branco padrão)
   → Sugestão: Adicionar cor secundária mais diferenciada

2. Tipografia muito padrão (Arial + sem variação)
   → Sugestão: Usar Montserrat para títulos, Open Sans para corpo

3. Layout segue template genérico (hero + 3 cards + footer)
   → Sugestão: Adicionar elemento decorativo único, quebrar grid em alguns pontos

4. Ícones parecem stock (muito minimalists)
   → Sugestão: Estilos customizados ou variações para dar personalidade

Você quer que o designer refaça com essas sugestões?
Ou prefere seguir como está?
```

**CENÁRIO C: REJEIÇÃO** ❌
- Problemas críticos (parece muito IA, não alinhado)
- Recomenda revisão completa

```
❌ DESIGN REJEITADO

Problemas críticos:

1. Parece template genérico (muito comum em 1000 sites)
2. Paleta de cores extremamente genérica (azul + branco)
3. Tipografia não personalizada (Arial, padrão do sistema)
4. Layout segue padrão automático (sem diferenciação)
5. Nenhum elemento visual memorável

Este design não transmite a identidade do seu produto.
Recomendo: Revisão completa do design system com foco em diferenciação.

Voltar para skill designer-html com feedback detalhado.
```

---

## Critérios de Aprovação (INCREMENTADO!)

### APROVADO se:
1. ✅ Design é alinhado com o nicho/público
2. ✅ NÃO parece template genérico de IA
3. ✅ Paleta de cores diferenciada (não azul+branco padrão)
4. ✅ Tipografia personalizada (não só Arial/Helvetica)
5. ✅ Tem elemento visual memorável (não tudo igual)
6. ✅ Layout tem variação (não tudo uniforme/robótico)
7. ✅ Cor principal bem utilizada
8. ✅ **3i's do Marketing refletidos visualmente** (Comunicador, Consumidor, Produto)
9. ✅ **Big Idea Visual clara e distintiva** (elemento memorável próprio)
10. ✅ **Paliativo destacado visualmente** (fundo/cor diferenciado, CTA claro)
11. ✅ **Argumento Incontestável em destaque** (número grande, fonte citada)
12. ✅ Nenhum emoji típico de IA (🎯 🚀 ⚡ 📈 🎥)

### REJEITADO se:
1. ❌ Parece que foi gerado por ferramenta automática
2. ❌ Paleta genérica (azul + branco obrigatório)
3. ❌ Tipografia 100% padrão do sistema
4. ❌ Nenhuma diferenciação visual
5. ❌ Layout extremamente previsível/template-like
6. ❌ 3i's não refletidos (design não comunica identidades)
7. ❌ Sem Big Idea Visual (nenhum elemento memorável único)
8. ❌ Paliativo sem destaque apropriado
9. ❌ Argumento Incontestável invisível ou pouco destacado
10. ❌ Contém emojis típicos de IA

---

## Exemplos: Design Fraco vs. Design Forte

### ❌ DESIGN FRACO (seria rejeitado)

```
Paleta: Azul + Branco + Cinza (genérica)
Tipografia: Arial (padrão do sistema)
Layout: Hero | 3 Cards | Footer (padrão)
Ícones: Stock icons minimalistas
Espaçamento: Muito uniforme
Diferenciação: Nenhuma
```

**Problema**: Parece um template de Wix que 5.000 pessoas usam.

---

### ✅ DESIGN FORTE (seria aprovado)

```
Paleta: Laranja #FF6B00 + Cinza #222 + Verde #2ECC40 (diferenciada)
Tipografia: Montserrat 900 (títulos, bold) + Open Sans 400 (corpo)
Layout: Hero assimétrico | Benefícios em zig-zag | CTA destacada
Ícones: Customizados com estilo próprio (não stock)
Espaçamento: Breathing room inteligente (não uniforme)
Detalhe único: Gradient no fundo do hero, padrão na seção de benefícios
Diferenciação: Laranja chamativa diferencia bem, tipo não é genérica
```

**Análise**:
- Cores: Laranja é ousada e diferencia (não azul padrão)
- Tipografia: Montserrat tem personalidade, não é Arial
- Layout: Zig-zag quebra padrão grid (menos robótico)
- Ícones: Customizados (não parecem 1000 sites iguais)
- Espaçamento: Variado (há áreas apertadas, áreas soltas)
- Resultado: Memorável, próprio, não parece IA

---

## Red Flags: Sinais de "Parece IA"

🚩 Muita simetria perfeita
🚩 Cores 100% saturadas (sem depth)
🚩 Tipografia sem peso visual
🚩 Espaçamento muito uniforme
🚩 Sem texturas ou padrões
🚩 Ícones muito minimalists/simplistas
🚩 Layout grid-perfeito (previsível)
🚩 Nenhum elemento "quebra" o padrão
🚩 Cores azul + branco (Template padrão)
🚩 Nada é memorável
🚩 **Emojis típicos de IA**: 🎯 🚀 ⚡ 📈 🎥 (indicam falta de originalidade)

---

## Green Flags: Sinais de Bom Design

🟢 Tipografia tem personalidade (mistura de weights/estilos)
🟢 Cores têm profundidade (gradientes, variações)
🟢 Elementos com detalhe humanizado
🟢 Espaçamento inteligente (não uniforme)
🟢 Variação visual clara (nem tudo igual)
🟢 Ícones com estilo próprio
🟢 Um detalhe visual que marca (memorável)
🟢 Layout quebra grid quando apropriado
🟢 Alinhado visualmente com o nicho
🟢 Se eu visse em 5 sites, reconheceria qual é qual

---

## Resumo: O Revisor de Design

A skill **revisor-design** é um **designer sênior profissional** que valida:

✅ Design não parece genérico/IA
✅ Alinhamento com nicho/público
✅ Identidade visual própria
✅ Profissionalismo e qualidade
✅ Diferenciação visual
✅ Conformidade com briefing

**Sem revisor design**: Copy bom, design genérico = pouca conversão
**Com revisor design**: Copy bom + design memorável = vendas! 🚀

---

## Nota Final

O revisor de design não está aqui para censurar ou forçar mudanças desnecessárias. Está aqui para garantir que o design:

1. **Não pareça automático** (key goal: evitar "parece IA")
2. **Comunique visualmente** o que o copy promete
3. **Seja memorável** e diferenciado
4. **Funcione** para o nicho/público específico

Se o design passa nessas 4 coisas, é aprovado. 🎉
