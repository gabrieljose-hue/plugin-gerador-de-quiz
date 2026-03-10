---
description: Gera página HTML profissional a partir do copy de vendas
---

# Comando /criar-pagina

Orquestra o fluxo completo: converte copy → faz perguntas → extrai estrutura → gera HTML profissional.

## REGRA DE FLUXO CONTÍNUO

Cada passo deve fluir automaticamente para o próximo sem anunciar, pedir confirmação ou esperar o usuário dizer "continue". Só pausar quando houver uma decisão real do usuário (AskUserQuestion). Nunca emitir mensagens como "Pronto para...", "Agora vamos...", "Configurações salvas!", "Ótimo!".

---

## Fluxo (7 passos + 4 sub-passos para construção)

### PASSO 0: Decisão inicial (nova lógica)

**Objetivo**: Perguntar se o mentorado já tem um copy pronto ou precisa criar do zero.

**Usar `AskUserQuestion`** com 1 pergunta:
- Pergunta: "Você já tem um copy pronto para sua página de vendas?" (header: "Copy")
- Opções:
  - "Sim, tenho um copy pronto" (description: "Vou colar meu texto e seguir direto para a extração")
  - "Não, quero gerar um copy agora" (description: "O assistente vai me guiar com perguntas para criar o copy do zero")

**Bifurcação do fluxo**:

#### Opção A: Já tem copy
- Define `COPY_ORIGEM = 'própria'`
- Ir direto para **PASSO 1: Obter copy** (mentorado cola o texto)

#### Opção B: Quer gerar copy
- Define `COPY_ORIGEM = 'copywriter'`
- Skill também gera a **Premissa** (até 10 palavras — sacada criativa que gera curiosidade)
- Coletar contexto estratégico (os **3i's do Marketing**):
  - **Identidade do Comunicador**: Como você quer ser visto pelo seu público? (ex: especialista, mentor, guia)
  - **Identidade do Consumidor**: Como o seu cliente quer ser visto? Que transformação pessoal ele busca?
  - **Identidade do Produto**: O que o produto representa? Qual transformação ele promete?
- Chamar skill **copywriter** para criar copy estruturado
- Skill faz perguntas estruturadas para entender:
  - **Quadro**: qual é o resultado/transformação que o produto promete?
  - **Furadeira**: qual é o método/passo a passo para chegar lá?
  - **Decorado**: quais são os benefícios emocionais após atingir o quadro?
- Retorna `COPY_BRUTO` gerado pela skill (já aprovado pelo mentorado no step 9 da skill)
- **Nota**: Este copy gerado passará por revisão obrigatória no **Passo 3.5** (revisor-copy)

---

### PASSO 1: Obter copy do mentorado

**Objetivo**: Capturar o texto do copy do mentorado (vindo de Opção A ou do skill copywriter).

**Procedimento**:

Se vindo de **Opção A** (já tem copy):
1. Pedir: "Cole aqui o texto completo do seu copy:"
2. Aguardar mentorado colar
3. Texto colado → `COPY_BRUTO`

Se vindo de **Opção B** (gerado por copywriter):
1. Skill já retornou `COPY_BRUTO` aprovado pelo mentorado
2. Usar diretamente como `COPY_BRUTO`

Seguir para **PASSO 2** com `COPY_BRUTO` confirmado.

**Saída**: Variável `COPY_BRUTO` com todo o texto do copy.

---

### PASSO 2: 10 Perguntas interativas com AskUserQuestion + Salvar configurações

**Usar a tool `AskUserQuestion` para coletar todas as configurações de forma organizada.**

O fluxo será em 2 rodadas de perguntas (para não sobrecarregar com 10 perguntas de uma vez).

---

#### RODADA 2.1: Perguntas 0-4 (Vídeo, Cor, Nome, Preço, Link)

**Chamar AskUserQuestion com as 4 perguntas abaixo:**

**0. URL do vídeo de vendas** (opcional)
- Tipo: `other` (aceita qualquer URL ou deixa em branco)
- Exemplo: `https://youtube.com/watch?v=abc123` ou `https://vimeo.com/123456789` ou deixe em branco
- Header: "Vídeo de Vendas"
- Fallback: (vazio — não exibir seção de vídeo)

**1. Cor principal do site** (sugestão inicial para o designer)
- Tipo: `other` (campo de texto para código hex)
- Exemplo: `#FF6B00`
- Header: "Cor Principal"
- Fallback: `#FF6B00`

**2. Nome do produto** (para `<title>` da página)
- Tipo: `other` (campo de texto)
- Exemplo: `Python para Análise de Dados`
- Header: "Nome Produto"
- Fallback: `Página de Vendas`

**3. Preço do produto** (valor final, ex: "R$ 297" ou "R$ 1.997")
- Tipo: `other` (campo de texto)
- Exemplo: `R$ 297`
- Header: "Preço"
- Fallback: (deixar vazio, placeholder `[INSERIR PREÇO]`)

**4. Link da página de compra/checkout**
- Tipo: `other` (field para URL)
- Exemplo: `https://checkout.sua-empresa.com/produto123`
- Header: "Link Checkout"
- Fallback: `#` com placeholder `[INSERIR LINK DE CHECKOUT]`

---

#### RODADA 2.2: Perguntas 5-9 (Urgência, Tipo, Logo, Foto, Imagem)

**Chamar AskUserQuestion com as 5 perguntas abaixo:**

**5. Urgência/Escassez**
- Tipo: `other` (campo de texto)
- Exemplo: `10 vagas restantes` ou `Prazo até 31/03` ou deixe em branco
- Header: "Urgência"
- Fallback: (omitir se vazio)

**6. Tipo de depoimentos**
- Tipo: `other` com opções (dropdown-like)
  - Opção A: `texto` — depoimentos em texto/citações
  - Opção B: `video` — depoimentos em vídeo (iframes)
- Header: "Depoimentos"
- Fallback: `texto`

**7. URL da logo** (Cloudron ou CDN)
- Tipo: `other` (campo para URL)
- Exemplo: `https://cdn.seu-cloudron.com/logo.png` ou deixe em branco
- Header: "Logo URL"
- Fallback: (vazio — navbar mostra apenas nome do produto)

**8. URL da foto do expert** (Cloudron ou CDN)
- Tipo: `other` (campo para URL)
- Exemplo: `https://cdn.seu-cloudron.com/expert.jpg` ou deixe em branco
- Header: "Foto Expert"
- Fallback: (vazio — placeholder cinza com ícone 👤)

**9. URL da foto/imagem do produto** (se não tiver vídeo, aparece aqui no Hero)
- Tipo: `other` (campo para URL)
- Exemplo: `https://cdn.seu-cloudron.com/produto.jpg` ou deixe em branco
- Header: "Imagem Produto"
- Fallback: (vazio — placeholder genérico)

**Após coletar as 10 respostas** (Rodadas 2.1 + 2.2):

1. Consolidar todas as respostas
2. Salvar em `output/config-pagina.json` com:
```json
{
  "video_url": "string (vazio = sem vídeo)",
  "cor_principal": "#RRGGBB",
  "nome_produto": "string",
  "preco_produto": "string (vazio = usar placeholder)",
  "link_checkout": "string ou #",
  "urgencia_escassez": "string (vazio = omitir)",
  "tipo_depoimentos": "texto | video",
  "logo_url": "string (vazio = sem logo)",
  "expert_foto_url": "string (vazio = placeholder)",
  "produto_imagem_url": "string (vazio = placeholder)"
}
```

---

### PASSO 3: Chamar skill extrator-copy

Usar a skill **extrator-copy** para extrair JSON estruturado do copy bruto.

**Entrada**: `COPY_BRUTO`

**Saída**:
- Skill salva JSON em: `output/copy-estruturada.json`
- Chat recebe apenas: confirmação do arquivo + `campos_ausentes[]` resumido
- Exemplo: "✅ Copy estruturado salvo. Campos ausentes: expert.bio, depoimentos (apenas 1 encontrado)"

---

### PASSO 3.5: Chamar skill revisor-copy (NOVO)

**Condição**: Este passo é executado **apenas se** `COPY_ORIGEM = 'copywriter'` (gerada pela skill)

- Se `COPY_ORIGEM = 'própria'` (Opção A) → **PULAR** este passo, ir direto ao Passo 4
- Se `COPY_ORIGEM = 'copywriter'` (Opção B) → executar normalmente

---

Usar a skill **revisor-copy** para validar se a copy está profissional, persuasiva e pronta para vender.

**Entrada**:
- `COPY_BRUTO`
- `JSON_ESTRUTURADO` (do passo 3)
- Contexto de nicho/público

**Fluxo**:
1. Revisor analisa copy contra checklist:
   - Promessa clara e persuasiva
   - Estrutura QFD completa (Quadro → Furadeira → Decorado)
   - **Para quem é / Para quem não é**: segmentação clara do público-alvo
   - **Argumento Incontestável**: contém uma afirmação baseada em dados, pesquisa ou ciência que reforça a credibilidade
   - **❌ NÃO contém emojis típicos de IA**: 🎯 🚀 ⚡ 📈 🎥 e similares
   - Linguagem natural, sem "marcas de IA"
   - Alinhamento com público/nicho
2. Retorna resultado:
   - **APROVADA**: `COPY_VALIDADA` → segue para Passo 4
   - **REJEITADA**: feedback detalhado → mentorado refaz copy, volta ao Passo 3
   - **APROVADA COM SUGESTÕES**: mentorado decide se aceita sugestões ou segue

**Saída**: `COPY_VALIDADA` (aprovada para próximo passo)

---

### PASSO 4: Chamar skill designer-html

Usar a skill **designer-html** para pensar e definir o design system.

**Entrada**:
- `output/copy-estruturada.json` (lê do arquivo, Passo 3)
- `COR_PRINCIPAL` (resposta da pergunta 1)
- `NOME_PRODUTO` (resposta da pergunta 2)
- `PRECO_PRODUTO` (resposta da pergunta 3)

**Requisitos obrigatórios de layout**:
- ❌ **SEM navbar/header** — Página limpa
- **Logo** (se houver): centralizada no topo do hero
- **Headline**: centralizada, alinhamento de texto centralizado
- **Subheadline**: centralizada, alinhamento de texto centralizado
- **Bullet points**: centralizado na página, alinhamento do texto à esquerda
- **Vídeo** (se houver): centralizado, APÓS bullets, não logo abaixo da subheadline
- Manter hierarquia visual clara e proporcional

**Fluxo**:
1. Skill analisa nicho, preço e produto em voz alta
2. Se cor não combinar: questiona e oferece alternativas (aguardar resposta)
3. Gera `output/design-preview.html` — preview visual do design (respeitando requisitos acima)
4. Salva `output/design-tokens.json` — DESIGN_TOKENS como arquivo

5. Skill designer-html já pergunta ao mentorado se quer ajustes (internamente)
6. Quando aprovado, seguir direto para Passo 4.5

**Saída do Chat**:
- Card clicável do `output/design-preview.html` (gerado automaticamente pela tool Write)
- Resumo de 5 linhas: Estética | Paleta principal | Tipografia resumida
- Pergunta de aprovação via `AskUserQuestion` na mesma resposta
- **NÃO emitir o JSON completo do DESIGN_TOKENS** (será lido do arquivo)

**Nota**: Este design passará por revisão obrigatória no **Passo 4.5** (revisor-design)

---

### PASSO 4.5: Chamar skill revisor-design (NOVO)

Usar a skill **revisor-design** para validar se o design tem identidade visual própria, está alinhado com o nicho/público e não parece genérico de IA.

**Entrada**:
- `output/design-tokens.json` (lê do arquivo, Passo 4)
- `output/design-preview.html` (lê do arquivo, Passo 4)
- `output/copy-estruturada.json` (lê do arquivo, Passo 3)
- `NOME_PRODUTO` e nicho/público

**Fluxo**:
1. Revisor lê os arquivos (não recebe conteúdo no chat)
2. Analisa design contra checklist:
   - **❌ NÃO parece gerado por IA** (sem marca típica: sem 🎯 🚀 ⚡ 📈 🎥 etc)
   - ❌ **SEM navbar/header** — Página deve estar limpa (não há navbar)
   - ✅ **Logo centralizada no topo** (se houver)
   - ✅ **Headline centralizada** com alinhamento de texto centralizado
   - ✅ **Subheadline centralizada** com alinhamento de texto centralizado
   - ✅ **Bullet points centralizados** com alinhamento de texto à esquerda (DENTRO da mesma seção hero)
   - ✅ **Vídeo centralizado** APÓS os bullets (DENTRO da mesma seção hero)
   - ✅ **Botão CTA após vídeo** (marca final do hero, DENTRO da mesma seção)
   - ✅ **Seção Demonstração** separada (após hero)
   - ✅ **Seção Para Quem É** separada (após demonstração)
   - Identidade visual própria e profissional
   - Alinhamento com nicho e público-alvo
   - Paleta de cores consistente
   - Tipografia clara e legível
   - Hierarquia visual bem definida
   - Spacing e proporções adequadas
3. Retorna resultado:
   - **APROVADO**: status "Design aprovado para construção" → segue para Passo 5
   - **AJUSTES NECESSÁRIOS**: lista de ajustes → skill designer-html refaz, volta para Passo 4.5
   - **REJEIÇÃO**: feedback crítico → designer revisão completa, volta ao Passo 4

**Saída do Chat**:
- Status (APROVADO/AJUSTES/REJEIÇÃO) + lista de pontos (não emitir arquivos completos)

---

### Estrutura de 17 Seções da Página de Vendas (REFERÊNCIA)

**BLOCO 1: HERO / PERSUASÃO INICIAL (SEM NAVBAR — UMA ÚNICA SEÇÃO)**

**ESTRUTURA IMPORTANTE**: Os pontos 1-3 ficam TODOS dentro de `<section id="hero">`, mas com estrutura interna clara.

1. **Logo + Headline + Subheadline + Urgência** (dentro do `<section id="hero">`)
   - ❌ **SEM navbar/header** — Página limpa e focada
   - Logo: Centralizada no topo (se `LOGO_URL` preenchida)
   - Headline: Promessa principal, curta, impactante, centralizada
   - Subheadline: Contexto e clareza, centralizada
   - Urgência/Escassez: Se preenchida, centralizada abaixo
   - Objetivo: Chamar atenção imediata, gerar curiosidade

2. **Bullet Points / Chamada para Ação Textual** (dentro do `<section id="hero">`)
   - Bullet points mostrando resultados e benefícios
   - "O que você aprenderá/ganhará"
   - Centralizado na página, bullets alinhados à esquerda
   - Sem exageros, mantendo curiosidade ativa
   - Objetivo: Convencer sem entregar tudo

2.5. **Vídeo de Vendas** (dentro do `<section id="hero">`, após bullets)
   - ✅ **Se `VIDEO_URL` preenchida**: Aparece DEPOIS dos bullet points
   - YouTube: iframe 16:9 responsivo
   - Vimeo: iframe 16:9 responsivo
   - Max-width: 800px desktop, 100% mobile, centralizado
   - ❌ **Se `VIDEO_URL` vazio**: Omitir completamente (sem placeholder)

3. **Botão com CTA** (dentro do `<section id="hero">`)
   - Verbo de ação + decorado
   - Posição: após vídeo/bullets, bem visível, marca final do hero
   - Repetido nas seções posteriores

4. **Demonstração** (nova `<section id="demonstracao">` — seção separada)
   - Explicação prática de como o produto funciona
   - Quebra objeção: "Tá, mas isso funciona mesmo?"
   - Formatos: Vídeo, gráfico, print, passo a passo
   - Pode incluir trechos da Furadeira (método)

5. **Para Quem É** (nova `<section id="para-quem">` — seção separada)
   - Bullet points segmentados por perfil/situação/desafio
   - Cada ponto = cenário real que leitor se identifica
   - Objetivo: Eliminar dúvida "Será que funciona pra mim?"

---

**BLOCO 2: OFERTA / DETALHES**

6. **Explicação do Formato**
   - Tipo: Curso, comunidade, acompanhamento, acervo, etc.
   - Como funciona
   - Como será entregue (plataforma, lives, acesso imediato, passo a passo)

7. **Explicação da Oferta**
   - O que a pessoa recebe ao comprar
   - Valor acumulado de cada entrega
   - "Empacotar" a oferta com clareza

8. **Garantia**
   - Tirar o medo da compra
   - Evitar linguagem rebuscada ou jurídica
   - Promessa clara (ex: "7 dias de garantia 100%, sem perguntas")

9. **Provas (Depoimentos)**
   - Depoimentos em vídeo ou texto
   - Prints de resultado (vendas, DMs, comentários)
   - Estudos de caso breves
   - Resultados do próprio criador
   - Alinhados com o Quadro e Decorados

---

**BLOCO 3: CREDIBILIDADE**

10. **Autoridade**
    - Experiência
    - Resultados próprios
    - Histórias reais
    - Resultados de alunos (prova social + autoridade)
    - Objetivo: Criar confiança, reforçar por que confiar

11. **Objeções e Respostas**
    - Antecipa dúvidas que impedem a compra
    - Respostas claras, empáticas, persuasivas
    - Formatos: FAQ estilo, antes/depois, etc.

12. **FAQ (Perguntas Frequentes)**
    - Dúvidas operacionais, técnicas, estratégicas
    - Direto e prático
    - Dúvidas mais frequentes dos clientes

13. **Suporte**
    - Mostrar que a pessoa não estará sozinha
    - Canais de comunicação
    - Tempos de resposta
    - Tipos de suporte ofertado

---

**BLOCO 4: EMOÇÃO / CONEXÃO + LÓGICA**

14. **Argumentos Incontestáveis / Racionais**
    - Dados, pesquisas, lógica prática
    - Desarmam ceticismo com fatos objetivos
    - Números, analogias, validação de cenário
    - Exemplo: "97% dos alunos reportam melhora em 30 dias"

15. **Dores e Desejos**
    - Aprofunda conexão emocional
    - Pessoa se vê no problema que vive hoje
    - Pessoa se vê no desejo que quer realizar
    - Objetivo: Conexão visceral

17. **Comparação com Outras Soluções**
    - Por que sua solução é melhor
    - O que a pessoa já tentou vs. sua solução
    - Formatos: Tabela comparativa, "Com [produto]" vs. "Sem [produto]"
    - Visual claro e organizado

---

### PASSO 5: Gerar os 4 blocos sequencialmente

Em vez de gerar todas as 17 seções de uma vez, gerar 4 arquivos HTML em sequência (blocos 1-4) para reduzir uso de tokens no contexto.

---

### PASSO 5a: Chamar skill construtor-bloco1

Usar a skill **construtor-bloco1** para gerar BLOCO 1 (seções 1-5).

**Entrada**:
- `output/config-pagina.json` (respostas do mentorado)
- `output/design-tokens.json` (design aprovado)
- `output/copy-estruturada.json` (copy extraído)

**Estrutura do BLOCO 1 (Seções 1-5)**:
1. Cabeçalho (Headline + Subheadline)
2. Chamada para Ação (Texto)
3. Botão CTA
4. Demonstração
5. Para Quem É

**Saída**: `output/bloco1.html`
- Inclui: `<!DOCTYPE html>`, `<head>`, `<style>`, `<script>`, opening `<body>`
- Não fecha `</body></html>` (será adicionado pelo bloco4)

---

### PASSO 5b: Chamar skill construtor-bloco2

Usar a skill **construtor-bloco2** para gerar BLOCO 2 (seções 6-9).

**Entrada**:
- `output/config-pagina.json`
- `output/design-tokens.json`
- `output/copy-estruturada.json`

**Estrutura do BLOCO 2 (Seções 6-9)**:
6. Explicação do Formato
7. Explicação da Oferta + Preço + Garantia
8. Provas/Depoimentos
9. Paliativo (Entregável Rápido)

**Saída**: `output/bloco2.html`
- Sem tags de abertura/fechamento de documento

---

### PASSO 5c: Chamar skill construtor-bloco3

Usar a skill **construtor-bloco3** para gerar BLOCO 3 (seções 10-13).

**Entrada**:
- `output/config-pagina.json`
- `output/design-tokens.json`
- `output/copy-estruturada.json`

**Estrutura do BLOCO 3 (Seções 10-13)**:
10. Autoridade do Expert
11. Objeções e Respostas
12. FAQ (Accordion)
13. Suporte

**Saída**: `output/bloco3.html`
- Sem tags de abertura/fechamento de documento

---

### PASSO 5d: Chamar skill construtor-bloco4

Usar a skill **construtor-bloco4** para gerar BLOCO 4 (seções 14, 15 e 17) + fechar documento.

**Entrada**:
- `output/config-pagina.json`
- `output/design-tokens.json`
- `output/copy-estruturada.json`

**Estrutura do BLOCO 4 (Seções 14, 15, 17)**:
14. Argumentos Incontestáveis
15. Dores e Desejos
17. Comparação com Outras Soluções

**Saída**: `output/bloco4.html`
- Fecha com: `</body></html>`
- Inclui qualquer script deferido se necessário

---

### PASSO 6: Concatenar blocos → HTML final

Após os 4 blocos serem gerados (Passos 5a-5d):

1. Ler `output/bloco1.html` (contém `<!DOCTYPE>` até opening `<body>`)
2. Ler `output/bloco2.html` (append)
3. Ler `output/bloco3.html` (append)
4. Ler `output/bloco4.html` (contém seções + closing `</body></html>`)
5. Concatenar em ordem e salvar em `output/pagina-de-venda.html`

**Validações pós-concatenação**:
- ✅ HTML válido (sem erros de sintaxe)
- ✅ Todas as 17 seções presentes
- ✅ Tags abertas/fechadas corretamente
- ✅ CSS e JS inline

**Saída**: `output/pagina-de-venda.html` (página completa, pronta para publicar)

---

### PASSO 7: Confirmação ao mentorado

Exibir sucesso + lista de campos ausentes + arquivos gerados.

```
✅ Página de vendas gerada com sucesso!

📁 Arquivos salvos:
  • output/copy-estruturada.json — copy estruturado
  • output/design-tokens.json — design system
  • output/design-preview.html — preview visual do design
  • output/config-pagina.json — configurações do mentorado
  • output/bloco1.html, bloco2.html, bloco3.html, bloco4.html — blocos intermediários
  • output/pagina-de-venda.html — página final (PRONTA PARA PUBLICAR)

🎨 Design aprovado:
  • Estética: [estética escolhida]
  • Paleta: [cores principais]
  • Tipografia: [fontes escolhidas]

📋 Configurações:
  • Produto: [NOME]
  • Preço: [PRECO]
  • Link checkout: [LINK]
  • Urgência: [URGENCIA]
  • Depoimentos: [TIPO]
  • Logo URL: [URL OU não informada]
  • Foto expert URL: [URL OU não informada]

⚠️  Campos ausentes no copy (complete manualmente):
  • [lista de campos]
```

---

## Validações

- **Cor**: Formato hex `#RRGGBB` (ex: `#FF6B00`)
- **URL**: Começa com `http://` ou `https://`
- **Tipo depoimentos**: `texto` ou `video`

---

## Tratamento de erros

### Se copy não for fornecido
```
❌ Nenhum copy foi colado.

Por favor:
1. Cole o texto completo do seu copy aqui no chat
2. Ou escolha criar um novo copy seguindo a estrutura QFD
```

### Se JSON for inválido
```
Erro ao extrair o copy. Tentando novamente...
```

Retry até 2 vezes.

---

## Arquivos de Estado (novo fluxo file-based)

| Arquivo | Escrito por | Lido por |
|---------|-------------|----------|
| `output/config-pagina.json` | Passo 2 (respostas do mentorado) | Passos 5a-5d (construtores de blocos) |
| `output/copy-estruturada.json` | Passo 3 (skill extrator-copy) | Passos 3.5, 4, 4.5, 5a-5d |
| `output/design-tokens.json` | Passo 4 (skill designer-html) | Passos 4.5, 5a-5d |
| `output/design-preview.html` | Passo 4 (skill designer-html) | Passo 4.5 (skill revisor-design) |
| `output/bloco1.html` | Passo 5a (skill construtor-bloco1) | Passo 6 (concatenação) |
| `output/bloco2.html` | Passo 5b (skill construtor-bloco2) | Passo 6 (concatenação) |
| `output/bloco3.html` | Passo 5c (skill construtor-bloco3) | Passo 6 (concatenação) |
| `output/bloco4.html` | Passo 5d (skill construtor-bloco4) | Passo 6 (concatenação) |
| `output/pagina-de-venda.html` | Passo 6 (concatenação) | Final (publicação) |

---

## Referências

- **Skills (10 no total)**:
  - `copywriter`: Gera copy estruturado (Passo 0 — opcional)
  - `extrator-copy`: Extrai estrutura do copy (Passo 3) + salva em arquivo
  - `revisor-copy`: Valida copy (promessa, QFD, persuasão) (Passo 3.5)
  - `designer-html`: Define design system (Passo 4) + salva em arquivos
  - `revisor-design`: Valida design (não parece IA, alinhado com nicho) (Passo 4.5) + lê arquivos
  - `construtor-bloco1`: Gera seções 1-5 do HTML (Passo 5a) — **NOVO**
  - `construtor-bloco2`: Gera seções 6-9 do HTML (Passo 5b) — **NOVO**
  - `construtor-bloco3`: Gera seções 10-13 do HTML (Passo 5c) — **NOVO**
  - `construtor-bloco4`: Gera seções 14, 15 e 17 do HTML (Passo 5d) — **NOVO**

- **QFD Framework**: Quadro (resultado/transformação) → Furadeira (método/processo) → Decorado (benefícios emocionais)
- **3i's do Marketing**: Identidade do Comunicador + Identidade do Consumidor + Identidade do Produto
- **17 seções da página** (estrutura completa):
  - **BLOCO 1 (Hero/Persuasão)**: Cabeçalho | Chamada Ação | Botão CTA | Demonstração | Para Quem É
  - **BLOCO 2 (Oferta)**: Formato | Oferta | Garantia | Provas/Depoimentos
  - **BLOCO 3 (Credibilidade)**: Autoridade | Objeções & Respostas | FAQ | Suporte
  - **BLOCO 4 (Emoção/Lógica)**: Argumentos Incontestáveis | Dores & Desejos | Comparação vs. Soluções
- **Fluxo de validação**: Copy → Revisor Copy → Design → Revisor Design → HTML final
