---
name: criador-quiz
description: Gera todas as telas do quiz seguindo o framework de 13 etapas (18 telas) de conversao
---

# Skill: Criador de Quiz

## Missao

Ler os dados coletados e gerar as 18 telas do quiz em ordem exata, seguindo o framework psicologico validado que transforma leads em compradores por impulso.

---

## Entrada (INPUT)

- `output/dados-quiz.json` (lido do arquivo)

---

## Saida (OUTPUT)

### Arquivos gerados
1. `output/quiz.json` — estrutura JSON completa de todas as telas
2. `output/quiz.md` — documento legivel para implementacao no Inlead

### Retorno ao Chat
```
Quiz gerado com sucesso!

  • Total de telas: 18 (20 contando 2A e 2B)

  • {{resposta}} dinamica: ativa na tela 14 (referencia tela 13)
  • Segmentacao por genero: [Sim/Nao]
  • Pagina de vendas interna: inclusa (tela 18)
```

**NAO emitir o conteudo completo no chat. NAO mencionar caminhos de arquivo como texto.**

Apos salvar os arquivos, usar a ferramenta `Read` em `output/quiz.md` e depois em `output/quiz.json` para que apareçam como cards clicaveis no chat.

---

## PASSO 0: Auto-Deteccao Antes de Gerar

Antes de gerar qualquer tela, ler `output/dados-quiz.json` e determinar automaticamente:

### A. Incluir segmentacao por genero? (`incluir_genero`)

**Regra**: Analisar `produto.tema` e `publico.descricao`.

`incluir_genero = false` se o tema/publico contiver indicadores de genero unico:
- Palavras que indicam so feminino: "gravidez", "parto", "amamentacao", "gestante", "maternidade", "menopausa", "ovario", "utero", "maes", "mulheres", "feminino", "feminina"
- Palavras que indicam so masculino: "prostata", "barba", "calvice", "paternidade", "pais", "masculino", "masculina", "homens"
- Qualquer descricao de publico que deixe claro que e apenas um genero

`incluir_genero = true` em todos os outros casos (temas universais como produtividade, leitura, emagrecimento, financas, negocio, ansiedade, etc.)

**Se `incluir_genero = false`**: A Tela 1 nao tem escolha de genero — apenas titulo, argumento e botao CTA. Nao existem Telas 2A/2B.

---

### B. Incluir pergunta de idade? (`incluir_idade`)

**Regra**: Analisar `produto.tema` e `publico.descricao`.

`incluir_idade = false` se:
- O publico e claramente de uma faixa etaria especifica: "adolescentes", "universitarios", "jovens", "criancas", "terceira idade", "idosos", "aposentados"
- O tema e B2B ou tecnico sem componente de idade: "automacoes", "programacao", "excel", "gestao empresarial", "RH", "contabilidade", "juridico"
- O tema nao tem nenhum ganho de personalizacao por idade

`incluir_idade = true` em produtos de desenvolvimento pessoal, saude, habitos, leitura, financas pessoais, produtividade — onde a foto de uma pessoa de faixa etaria similar aumenta identificacao.

**Se `incluir_idade = false` e `incluir_genero = true`**: A Tela 2 tem apenas a escolha de genero (sem opcoes de faixa etaria).
**Se `incluir_idade = false` e `incluir_genero = false`**: Pular Tela 2 completamente.

---

### C. Quantas telas de depoimentos? (`telas_depoimentos`)

Baseado em `depoimentos_quantidade` dos dados:
- `"0"` ou ausente → `telas_depoimentos = 0` (omitir Telas 11 e 12)
- `"1-2"` → `telas_depoimentos = 1` (gerar so Tela 11)
- `"3+"` → `telas_depoimentos = 2` (gerar Tela 11 + Tela 11B com mais depoimentos)

**Se `telas_depoimentos = 0`**: Pular Telas 11 e 12 inteiramente. A Tela 10 leva direto para a Tela 13 (agora renumerada).

---

### D. Registrar decisoes

Antes de gerar, registrar no `quiz.json` (campo `meta`):
```json
{
  "incluir_genero": true,
  "incluir_idade": true,
  "telas_depoimentos": 1,
  "motivo_genero": "tema universal — produtividade",
  "motivo_idade": "desenvolvimento pessoal — faixa etaria relevante"
}
```

---

## Mapa de Telas (Adaptavel)

O numero final de telas varia entre 14 e 20 dependendo das decisoes acima.

| Tela | Condicao | Etapa | Tipo | Descricao |
|------|----------|-------|------|-----------|
| 1 | sempre | Abertura | apresentacao | Titulo + CTA (com ou sem escolha de genero) |
| 2A | se incluir_genero=true E incluir_idade=true | Dados Demograficos | escolha-unica | Genero masculino + Idade |
| 2B | se incluir_genero=true E incluir_idade=true | Dados Demograficos | escolha-unica | Genero feminino + Idade |
| 2 | se incluir_genero=true E incluir_idade=false | Dados Demograficos | escolha-unica | Genero (sem idade) |
| — | se incluir_genero=false E incluir_idade=false | — | — | Pular para Tela 3 |
| 3 | sempre | Prova | apresentacao | Numeros sociais + social proof |
| 4 | sempre | Diagnostico 1 | escolha-unica | Pergunta sobre a dor principal |
| 6 | sempre | Diagnostico 2 | escolha-unica | Segunda dimensao do problema |
| 7 | sempre | Diagnostico 3 | escolha-unica | Terceira dimensao do problema |
| 8 | sempre | Tocar na Ferida 1 | escolha-unica | "Sente-se culpado?" (so opcoes afirmativas) |
| 9 | sempre | Tocar na Ferida 2 | multipla-escolha | "Quais as consequencias negativas?" |
| 10 | sempre | Desejo Principal | multipla-escolha | "Qual seu maior objetivo?" |
| 11 | se telas_depoimentos >= 1 | Depoimentos | apresentacao | Depoimentos 1 (e 2 se quantidade permitir) |
| 11B | se telas_depoimentos = 2 | Depoimentos Extra | apresentacao | Mais depoimentos |
| 12 | se telas_depoimentos >= 1 | Nivel de Conhecimento | escolha-unica | "O que voce sabe sobre [tema]?" |
| 13 | sempre | Objetivo Especifico | escolha-unica | "Qual e o seu maior objetivo?" (usada na tela 14) |
| 14 | sempre | Promessa Personalizada | apresentacao | "Nos iremos te ajudar!" + {{resposta_tela_13}} |
| 15 | sempre | Micro-Compromisso | escolha-unica | Nivel de motivacao (so opcoes positivas) |
| 16 | sempre | Carregamento | carregamento | Analise de perfil |
| 17 | sempre | Resultado | apresentacao | Lead esta pronto + grafico |
| 18 | sempre | Pagina de Vendas | apresentacao | Oferta completa interna |

---

## Descricao Detalhada de Cada Tela

### TELA 1 — Abertura
**Etapa**: 1 — Abertura
**Tipo**: `apresentacao`

**Conteudo obrigatorio**:
- Titulo: Promessa direta e curta relacionada ao tema (o "quadro na parede" — o estado desejado)
  - Ex: "Leia mais rapido sem perder a compreensao"
  - Deve ser curto, impactante, claro
- Subtitulo: "Escolha seu genero para continuar (Questionario de 1min.)"
- Imagem sugerida: Visual impactante relacionado ao tema
- Opcoes (botoes): "Masculino" | "Feminino"
- Logica: Masculino → Tela 2A | Feminino → Tela 2B

**Tom**: A promessa deve gerar "eu quero saber mais" — nao revelar o metodo, so o resultado.

---

### TELA 2A — Dados Demograficos (Masculino)
**Etapa**: 2 — Dados Demograficos
**Tipo**: `escolha-unica`

**Conteudo obrigatorio**:
- Titulo: "Qual a sua idade?"
- Opcoes com fotos MASCULINAS de pessoas de cada faixa etaria:
  1. 18-28
  2. 29-38
  3. 39-49
  4. 50+
- Todas as opcoes levam para a Tela 3
- Imagem: Cada opcao tem foto de homem representando a faixa etaria

---

### TELA 2B — Dados Demograficos (Feminino)
**Etapa**: 2 — Dados Demograficos
**Tipo**: `escolha-unica`

**Conteudo obrigatorio**:
- Titulo: "Qual a sua idade?"
- Opcoes com fotos FEMININAS de pessoas de cada faixa etaria:
  1. 18-28
  2. 29-38
  3. 39-49
  4. 50+
- Todas as opcoes levam para a Tela 3
- Imagem: Cada opcao tem foto de mulher representando a faixa etaria

**Se `segmentar_genero = false`**: Gerar apenas 1 Tela 2 sem diferenciar fotos por genero.

---

### TELA 3 — Prova Social
**Etapa**: 3 — Prova
**Tipo**: `apresentacao`

**Conteudo obrigatorio**:
- Titulo: "[NUMEROS_SOCIAIS] utilizam o [PRODUTO]"
  - Ex: "+1000 alunos utilizam o Metodo MIL para leitura e memorizacao"
- Subtitulo: Resultado que essas pessoas alcancaram com o produto
  - Ex: "e recebemos depoimentos diariamente de pessoas que [TRANSFORMACAO_PRINCIPAL]"
- Imagem sugerida: Colagem de fotos de muitas pessoas (alunos/clientes) ou print de depoimentos
- Botao: "Continuar"

**Tom**: Confiante e inclusivo. O lead sente que muitas pessoas ja passaram por isso.

---

### TELA 4 — Diagnostico 1
**Etapa**: 4 — Coleta de Informacoes de Diagnostico
**Tipo**: `escolha-unica`

**Conteudo obrigatorio**:
- Titulo: Pergunta direta sobre o comportamento relacionado a DOR PRINCIPAL
  - A pergunta deve revelar o nivel/frequencia do problema
  - Ex: "Voce se distrai facilmente durante a leitura?"
- Opcoes: 4 opcoes graduadas do mais grave ao menos grave (ou do "sempre" ao "nunca")
  - Ex: "Me distraio toda hora, se passar uma mosca perco a atencao" | "Ocasionalmente" | "Raramente perco a concentracao" | "Sou muito concentrado"
- Todas as opcoes levam para a Tela 6

**Geracao**: Criar pergunta e opcoes baseadas em `dores.principal` dos dados coletados.

---

### TELA 6 — Diagnostico 2
**Etapa**: 4 — Coleta de Informacoes de Diagnostico (continuacao)
**Tipo**: `escolha-unica`

**Conteudo obrigatorio**:
- Titulo: "[PERGUNTA SOBRE OUTRO COMPORTAMENTO DO PROBLEMA]?"
  - A pergunta deve ser sobre uma SEGUNDA DIMENSAO do problema (diferente da Tela 4)
  - Ex: "Voce deixa as coisas para ultima hora?"
- Opcoes: 4 opcoes de frequencia
  - Ex: "Nunca" | "Raramente" | "Muitas vezes" | "Sempre"
- Todas as opcoes levam para a Tela 7

**Geracao**: A pergunta deve explorar um comportamento diferente da Tela 4 mas que tambem revela o mesmo problema central.

---

### TELA 7 — Diagnostico 3
**Etapa**: 4 — Coleta de Informacoes de Diagnostico (continuacao)
**Tipo**: `escolha-unica`

**Conteudo obrigatorio**:
- Titulo: Pergunta sobre uma TERCEIRA DIMENSAO do problema (diferente das Telas 4 e 6)
  - Ex: "Voce pega no sono durante a leitura?"
- Opcoes: 4 opcoes de frequencia
  - Ex: "Sempre" | "Algumas vezes" | "Raramente" | "Nunca"
- Todas as opcoes levam para a Tela 8

**Geracao**: Criar pergunta explorando outro sintoma ou consequencia do mesmo problema central.

---

### TELA 8 — Tocar na Ferida 1 (Culpa/Validacao Emocional)
**Etapa**: 5 — Tocar na Ferida
**Tipo**: `escolha-unica`

**Conteudo obrigatorio**:
- Titulo: "Sente-se culpado ou mal por isso?"
  - (Adaptar conforme o tema: "Sente frustrado com isso?" | "Isso te incomoda?" | "Voce se sente mal com essa situacao?")
- Opcoes: APENAS 2 opcoes — ambas afirmativas, nao ha opcao negativa
  1. "Sim, e muito"
  2. "Sim"
- Ambas as opcoes levam para a Tela 9

**Objetivo psicologico**: O lead confirma emocionalmente que o problema o afeta. Nao existe opcao "Nao" — isso forca o reconhecimento e amplifica a urgencia de resolver.

---

### TELA 9 — Tocar na Ferida 2 (Consequencias)
**Etapa**: 5 — Tocar na Ferida (continuacao)
**Tipo**: `multipla-escolha`

**Conteudo obrigatorio**:
- Titulo: "Quando isso acontece, quais sao, na sua opiniao, as consequencias negativas?"
- Subtitulo: "Selecione uma ou mais opcoes para avancar"
- Opcoes: Usar as `dores.consequencias` coletadas (minimo 5, ideal 6-7)
  - Cada opcao em PRIMEIRA PESSOA: "Nao consigo...", "Perco tempo...", "Sinto..."
  - Devem ser especificas e reais — o lead deve se reconhecer em pelo menos 2-3
- Todas as opcoes levam para a Tela 10

**Objetivo psicologico**: O lead nomeia as consequencias do seu problema. Selecionar = confirmar que precisa resolver. Cria urgencia emocional concreta.

---

### TELA 10 — Desejo Principal
**Etapa**: 6 — Entender o Principal Desejo do Lead
**Tipo**: `multipla-escolha`

**Conteudo obrigatorio**:
- Titulo: "Qual seu maior objetivo para [TEMA DO QUIZ]?"
- Subtitulo: "Selecione uma ou mais opcoes para avancar"
- Opcoes: Usar os `desejos` coletados (minimo 4, ideal 5)
  - Escritas de forma aspiracional e positiva
  - Ex: "Aprender mais rapido" | "Ser mais produtivo" | "Melhorar no trabalho"
- Todas as opcoes levam para a Tela 11

**Objetivo psicologico**: O lead declara o que quer. Quem declara um desejo, esta mais proximo de agir para realizalo.

---

### TELA 11 — Depoimentos
**Etapa**: 7 — Depoimentos Provando que o Problema tem Solucao
**Tipo**: `apresentacao`

**Conteudo obrigatorio**:
- Titulo: "Hoje somos [NUMEROS_SOCIAIS] e recebemos depoimentos diariamente de pessoas com [TRANSFORMACAO_PRINCIPAL]"
- Imagem 1: `depoimentos[0].imagem_url` (ou `[INSERIR URL DA IMAGEM DO DEPOIMENTO 1]`)
- Imagem 2: `depoimentos[1].imagem_url` se existir (ou `[INSERIR URL DA IMAGEM DO DEPOIMENTO 2]`)
- Instrucao: Exibir cada imagem em full-width, empilhadas verticalmente
- Botao: "Continuar"

**Objetivo psicologico**: Prova social especifica logo apos o lead confirmar suas dores e desejos. "Outras pessoas como eu resolveram isso."

---

### TELA 12 — Nivel de Conhecimento
**Etapa**: 7 — Depoimentos (continuacao — qualificacao do lead)
**Tipo**: `escolha-unica`

**Conteudo obrigatorio**:
- Titulo: "O que voce sabe sobre [TEMA DO QUIZ]?"
  - Ex: "O que voce sabe sobre tecnicas de leitura e memorizacao?"
- Opcoes:
  1. "Nada"
  2. "Pouca coisa"
  3. "Muito"
- Todas as opcoes levam para a Tela 13

**Objetivo psicologico**: Coleta dado de qualificacao e cria curiosidade. O lead percebe que nao sabe o suficiente — o que prepara para a promessa da Tela 14.

---

### TELA 13 — Objetivo Especifico (Personalizacao da Promessa)
**Etapa**: 7 — Depoimentos (continuacao — personalizacao)
**Tipo**: `escolha-unica`

**Conteudo obrigatorio**:
- Titulo: "Qual e o seu maior objetivo para [TEMA]?"
- Subtitulo: "Selecione uma das opcoes"
- Opcoes: 4 opcoes ESPECIFICAS que representam os resultados concretos do produto
  - Devem ser mais especificas e acionaveis que os desejos gerais da Tela 10
  - Devem ser exatamente o que o produto entrega/resolve
  - Ex: "Multiplicar a velocidade de leitura" | "Aumentar o foco e compreensao" | "Memorizar qualquer conteudo" | "Reduzir o sono e cansaco durante a leitura"
  - Usar `produto.objetivos_especificos` dos dados coletados
- Todas as opcoes levam para a Tela 14
- **IMPORTANTE**: A resposta selecionada nesta tela e usada dinamicamente na Tela 14

**Objetivo psicologico**: O lead escolhe 1 resultado especifico. Isso personaliza completamente a promessa da proxima tela — o que aumenta muito a conversao.

---

### TELA 14 — Promessa Personalizada
**Etapa**: 9 — Prometer Ajuda para Esse Objetivo
**Tipo**: `apresentacao`

**Conteudo obrigatorio**:
- Titulo: "Nos iremos te ajudar!"
- Corpo: "Voce encontrara no [PRODUTO], exercicios para **{{resposta selecionada na Tela 13}}** sem ou com apenas [AMPLIFICAR_OPORTUNIDADE]"
  - Ex: "Voce encontrara no Metodo MIL, exercicios para **multiplicar a velocidade de leitura** sem precisar de muito tempo livre"
  - `{{resposta_tela_13}}` = a opcao que o lead selecionou na Tela 13
  - `[AMPLIFICAR_OPORTUNIDADE]` = usar `produto.amplificar_oportunidade` dos dados coletados
- Imagem sugerida: Imagem do produto ou representacao visual da transformacao
- Botao: "Ver meu plano personalizado"

**REGRA CRITICA**: A Tela 14 DEVE referenciar dinamicamente a resposta da Tela 13. Descrever no `quiz.md` qual variavel usar e como a plataforma Inlead deve configurar esta logica.

**Objetivo psicologico**: O lead sente que a promessa foi feita especificamente para ele, baseada na resposta que ele acabou de dar. Isso cria conexao emocional maxima antes do compromisso.

---

### TELA 15 — Micro-Compromisso
**Etapa**: 10 — Solicitar Micro-Compromisso do Lead
**Tipo**: `escolha-unica`

**Conteudo obrigatorio**:
- Titulo: "Qual o seu nivel de motivacao para resolver esse problema?"
- Opcoes: EXATAMENTE 3 opcoes — todas positivas, nenhuma leva o lead a desistir
  1. "Estou muito determinado! Quero resolver isso agora."
  2. "Quero criar novos habitos"
  3. "Quero muito, mas tenho medo de falhar novamente"
- Todas as opcoes levam para a Tela 16

**Objetivo psicologico**: O lead declara motivacao. Quem se compromete publicamente (mesmo que so para si) tem mais probabilidade de completar a acao. Todas as opcoes sao "positivas" — nao existe saida.

---

### TELA 16 — Carregamento
**Etapa**: 11 — Tela de Carregamento
**Tipo**: `carregamento`

**Conteudo obrigatorio**:
- Titulo: "Analisando o seu perfil..."
- Subtitulo: "Isso levara apenas alguns segundos"
- **Contador animado**: numero grande centralizado que sobe de 0% ate 100% (efeito de contagem continua, ex: 0... 12... 37... 68... 91... 100%)
- Itens de progresso (aparecem em sequencia como checklist animado, sincronizados com o contador):
  1. "Analisando suas respostas..."          — aparece ~0-25%
  2. "Identificando seu perfil de [TEMA]..."  — aparece ~25-50%
  3. "Calculando seu potencial de melhora..." — aparece ~50-75%
  4. "Criando seu plano personalizado..."     — aparece ~75-100%
- Barra de progresso: 0% ate 100% (sincronizada com o contador)
- Duracao sugerida: 5-8 segundos
- Leva automaticamente para Tela 17 ao completar

**Objetivo psicologico**: Cria antecipacao e sensacao de personalizacao real. O lead sente que o sistema esta "trabalhando para ele" — o que aumenta o valor percebido do resultado.

---

### TELA 17 — Resultado
**Etapa**: 12 — Mostrar que o Lead Esta Pronto
**Tipo**: `apresentacao`

**Conteudo obrigatorio**:
- Titulo: "Com base nas suas respostas, voce esta pronto para [TRANSFORMACAO_PRINCIPAL] nos proximos dias"
- Subtitulo: "[NOME DO PRODUTO]"

**Componente visual obrigatorio: Antes & Depois com Termometro**

Gerar 2 a 3 metricas baseadas nos `produto.objetivos_especificos` e `dores.principal`. Para cada metrica, definir:
- Nome da metrica (ex: "Velocidade de leitura", "Foco e Compreensao")
- Estado Antes: adjetivo negativo (ex: "Baixa", "Fraco") + percentual baixo (15% a 30%)
- Estado Depois: adjetivo positivo (ex: "Rapida", "Forte") + percentual alto (85% a 96%)

**Estrutura do componente (2 colunas lado a lado)**:

```
          ANTES              |           DEPOIS
[imagem: pessoa frustrada]   | [imagem: pessoa celebrando]
   badge: "Antes"            |    badge: "Depois"
                             |
[Metrica 1 — label]          | [Metrica 1 — label]
[adjetivo negativo]  [XX%]   | [adjetivo positivo]  [XX%]
[termometro: 1-2 segmentos   | [termometro: todos os 5
 preenchidos, resto vazio]   |  segmentos preenchidos]
                             |
[Metrica 2 — label]          | [Metrica 2 — label]
[adjetivo negativo]  [XX%]   | [adjetivo positivo]  [XX%]
[termometro parcial]         | [termometro completo]
```

**Termometro (barra gradiente por segmentos)**:
- 5 segmentos coloridos em sequencia: vermelho → laranja → amarelo → verde-claro → verde
- Antes: apenas 1 segmento preenchido (vermelho), restantes apagados/cinza
- Depois: todos os 5 segmentos preenchidos (gradiente completo)
- Cada segmento e uma pilula arredondada separada

**Imagens sugeridas para o par Antes/Depois**:
- Antes: pessoa cansada, desanimada, frustrada com o problema (estilo ilustracao ou foto)
- Depois: pessoa animada, celebrando, energizada com o resultado conquistado

**Descricao das metricas por tema** (adaptar ao produto):
- Tema de leitura/aprendizado: "Velocidade de leitura" e "Foco e Compreensao" (e "Retencao de conteudo" se 3 metricas)
- Tema de emagrecimento/saude: "Disposicao e Energia" e "Controle Alimentar" (e "Autoestima" se 3)
- Tema de financas: "Controle Financeiro" e "Capacidade de Investir" (e "Seguranca Financeira" se 3)
- Tema de produtividade: "Foco e Concentracao" e "Entrega de Resultados" (e "Gestao do Tempo" se 3)
- Outros temas: derivar 2-3 metricas dos `produto.objetivos_especificos`

- Botao: "Ver meu plano"

**No quiz.md, documentar o componente como**:
```
Componente: Antes & Depois com Termometro
Metricas:
  1. [Nome] — Antes: [adjetivo] ([XX%]) | Depois: [adjetivo] ([XX%])
  2. [Nome] — Antes: [adjetivo] ([XX%]) | Depois: [adjetivo] ([XX%])
Imagem Antes: [descricao da imagem sugerida]
Imagem Depois: [descricao da imagem sugerida]
```

**Objetivo psicologico**: O lead ve visualmente a diferenca entre onde esta agora e onde vai estar. O termometro colorido torna a transformacao concreta, mensuravel e desejavel — o maior gatilho de conversao antes da oferta.

---

### TELA 18 — Pagina de Vendas Interna
**Etapa**: 13 — Pagina de Vendas Interna no Proprio Quiz
**Tipo**: `apresentacao` (longa)

**ESTRUTURA OBRIGATORIA (8 blocos em ordem)**:

**Bloco 1 — Comparativo Visual Antes vs Depois**:
- Titulo: Nao usa [PRODUTO] | Com [PRODUTO]
- Coluna "Quem NAO usa [PRODUTO]": lista de problemas/consequencias (usar `dores.consequencias`)
- Coluna "Quem USA [PRODUTO]": lista de beneficios/resultados (usar `desejos`)
- Visual: tabela comparativa clara, 2 colunas

**Bloco 2 — Headline Personalizada**:
- "O seu plano personalizado esta pronto!"
- Subtitulo: "Com base nas suas respostas, criamos o caminho mais rapido para voce [TRANSFORMACAO_PRINCIPAL]"

**Bloco 3 — O Que Voce Recebe** (ancoragem de preco):
- Se `produto.entregaveis` estiver preenchido (nao for `"[INSERIR ENTREGAVEIS]"` nem ausente):
  - Parsear o campo separando por `|` (pipe)
  - Renderizar cada item como linha: "Nome do item — De R$ [valor] por incluso" (preservar o texto exato do usuario, apenas formatar visualmente)
  - Total acumulado: "De R$ [TOTAL] por apenas R$ [PRECO_FINAL]" (calcular o total somando os valores se disponiveis, ou usar "[TOTAL]" se nao for possivel calcular)
- Se `produto.entregaveis` for `"[INSERIR ENTREGAVEIS]"` ou ausente:
  - Usar placeholders: [INSERIR MODULO X — De R$ X,00 por incluso]
  - Total acumulado: "De R$ [TOTAL] por apenas R$ [PRECO_FINAL]"

**Bloco 4 — Preco e CTA**:
- Preco em destaque: [PRECO do produto]
- Botao 1: "Quero comecar agora" → link para [LINK_CHECKOUT]
- Texto abaixo: "Acesso imediato apos a compra"

**Bloco 5 — Comparativo de Vida**:
- "Sua vida SEM o [PRODUTO]": descreve o futuro sem resolver o problema (usa `dores`)
- "Sua vida COM o [PRODUTO]": descreve o futuro com o produto resolvendo o problema (usa `desejos`)

**Bloco 6 — Depoimentos**:
- Imagem 1: `depoimentos[0].imagem_url` (ou `[INSERIR URL DA IMAGEM DO DEPOIMENTO 1]`)
- Imagem 2: `depoimentos[1].imagem_url` se existir (ou `[INSERIR URL DA IMAGEM DO DEPOIMENTO 2]`)
- Instrucao: Exibir cada imagem em full-width, empilhadas verticalmente

**Bloco 7 — CTA Final**:
- Botao 2: "Quero garantir meu acesso agora" → link para [LINK_CHECKOUT]

**Bloco 8 — Garantia**:
- "Garantia de 7 dias: Se por qualquer motivo voce nao estiver satisfeito, devolvemos 100% do seu investimento, sem perguntas."
- (Usar 7 dias como padrao se nao foi especificado pelo usuario)

---

## Calculo de Progresso por Tela

Calcular o percentual da barra de progresso para cada tela com base na posicao sequencial real no quiz (ignorando ramificacoes 2A/2B como duplicadas — ambas tem o mesmo progresso).

**Formula**: `progresso = round((posicao_sequencial / total_telas) * 100)`

- Telas 2A e 2B tem o mesmo valor de progresso (posicao 2)
- Tela 1 (Abertura): 0% — barra ainda nao exibe progresso (tela de entrada)
- Ultima tela (Pagina de Vendas): 100%

**Exemplo com 18 telas**:
| Tela | Progresso |
|------|-----------|
| 1    | 0%        |
| 2A/2B | 11%      |
| 3    | 17%       |
| 4    | 22%       |
| 5    | 28%       |
| 6    | 33%       |
| 7    | 39%       |
| 8    | 44%       |
| 9    | 50%       |
| 10   | 56%       |
| 11   | 61%       |
| 12   | 67%       |
| 13   | 72%       |
| 14   | 78%       |
| 15   | 83%       |
| 16   | 89%       |
| 17   | 94%       |
| 18   | 100%      |

---

## Formato de Saida: quiz.md

Cada tela deve ser documentada no seguinte formato:

```markdown
### Tela [N][A/B se aplicavel]: [Nome]

**Tipo**: [apresentacao | escolha-unica | multipla-escolha | carregamento]
**Etapa**: [numero e nome da etapa do framework]
**Progresso**: [XX%] — barra de progresso no topo da tela

**Conteudo**:
- Titulo: [texto exato]
- Subtitulo: [texto exato, se houver]
- Corpo: [texto completo, se houver]
- Imagem: [descricao da imagem sugerida para o usuario adicionar]
- Botao: [texto do botao]

**Opcoes** (se tipo for escolha):
1. [opcao 1]
2. [opcao 2]
3. [opcao 3]
4. [opcao 4]

**Logica de navegacao**:
- [para onde cada opcao leva — especificar condicional quando houver]
- [ou: "Todas as opcoes levam para a Tela [N+1]"]

**Objetivo psicologico**:
[O que essa tela faz no emocional do lead — 1 a 2 frases diretas]
```

---

## Formato de Saida: quiz.json

```json
{
  "meta": {
    "tema": "string",
    "produto": "string",
    "total_telas": 18,
    "tem_segmentacao_genero": true,
    "tela_resposta_dinamica": 13,
    "tela_usa_resposta_dinamica": 14
  },
  "telas": [
    {
      "id": "1",
      "nome": "Abertura",
      "etapa": "abertura",
      "tipo": "apresentacao",
      "progresso": 0,
      "titulo": "string",
      "subtitulo": "string",
      "imagem_sugerida": "string",
      "botao": "string",
      "opcoes": [
        { "texto": "Masculino", "vai_para": "2A" },
        { "texto": "Feminino", "vai_para": "2B" }
      ],
      "logica": "condicional por genero",
      "objetivo_psicologico": "string"
    }
  ]
}
```

---

## Regras Criticas de Geracao

1. **NUNCA inventar dados** — usar apenas o que esta em `dados-quiz.json`. Para campos ausentes, usar `[INSERIR X]`
2. **Ordem das 18 telas e obrigatoria** — nao pular, nao reordenar
3. **Tela 8 tem APENAS opcoes afirmativas** — "Sim, e muito" e "Sim". Nunca colocar opcao negativa
5. **Tela 13 e a chave da personalizacao** — a resposta escolhida DEVE ser referenciada na Tela 14
6. **Tela 14 usa `{{resposta_tela_13}}`** — descrever no quiz.md como configurar esta variavel no Inlead
7. **2A e 2B sao obrigatorias** se `segmentar_genero = true`
8. **Tela 18 tem todos os 8 blocos** mesmo que com placeholders [INSERIR X]
9. **Tom progressivo**: neutro → empatico → urgente → aspiracional → oferta
10. **Sem emojis** no conteudo das telas

---

## REGRA DE PERGUNTAS (OBRIGATORIO)

**Sempre usar a ferramenta `AskUserQuestion`** para qualquer pergunta, escolha ou coleta de input do usuario. Em todos os casos, sem excecao. Nunca fazer perguntas apenas no texto do chat.

---

## Procedimento

1. Ler `output/dados-quiz.json`
2. Identificar campos disponiveis vs ausentes
3. Gerar as 18 telas em ordem (2A e 2B se segmentacao ativada)
4. Para cada tela: escrever titulo/subtitulo/corpo, criar opcoes, definir logica de navegacao, objetivo psicologico
5. Montar `output/quiz.md` com todas as telas no formato documentado
6. Montar `output/quiz.json` com a estrutura completa
7. Executar auto-validacao (secao abaixo) — se falhar, corrigir e re-validar (max 2 tentativas)
8. Salvar ambos os arquivos
9. Retornar resumo de 5 linhas ao chat
10. Usar `Read` em `output/quiz.md` e `output/quiz.json` para exibi-los como cards clicaveis — nunca mencionar os caminhos como texto

---

## Auto-Validacao (antes de salvar)

Antes de salvar os arquivos, executar os seguintes checks internamente. Se qualquer check critico falhar, corrigir a tela afetada e validar novamente (maximo 2 tentativas). Nunca salvar um quiz que nao passe nos checks criticos.

### Checks Criticos (bloqueiam se falhar)

1. **Completude**: Todas as telas obrigatorias estao presentes e na ordem correta (consultar Mapa de Telas)
2. **Tela 8**: Tipo `escolha-unica` com EXATAMENTE 2 opcoes afirmativas ("Sim, e muito" / "Sim"). Nenhuma opcao negativa
3. **Tela 13**: Tipo `escolha-unica` com exatamente 4 opcoes de objetivos especificos do produto
4. **Tela 14**: Corpo referencia `{{resposta_tela_13}}` explicitamente
5. **Tela 15**: Tipo `escolha-unica` com exatamente 3 opcoes positivas. Nenhuma opcao de desistencia
6. **Tela 18**: Os 8 blocos da pagina de vendas estao presentes (comparativo, headline, entregaveis, preco+CTA, comparativo de vida, depoimentos, CTA final, garantia)
7. **Sequencia psicologica**: Diagnostico (4,6,7) → Ferida (8,9) → Desejo (10) → Depoimentos (11) → Objetivo (13) → Promessa (14) → Compromisso (15) → Loading (16) → Resultado (17) → Vendas (18)

### Checks de Qualidade (corrigir se possivel)

8. **Tela 9**: Tipo `multipla-escolha` com minimo 5 consequencias em primeira pessoa
9. **Tela 10**: Tipo `multipla-escolha` com minimo 4 desejos em tom positivo
10. **Diagnostico (4, 6, 7)**: As 3 perguntas sao DIFERENTES entre si (dimensoes distintas do problema)
11. **Tom**: Sem emojis em nenhuma tela. Tom progressivo (neutro → empatico → urgente → aspiracional → oferta)

Se todos os checks passam: salvar os arquivos e continuar.
Se algum check critico falha: corrigir internamente e re-validar. Nao pedir input do usuario para correcoes estruturais.

---

## Exemplos de Telas Criticas

### Exemplo: Tela 8 (Tocar na Ferida 1)

```markdown
### Tela 8: Tocar na Ferida 1

**Tipo**: escolha-unica
**Etapa**: 5 — Tocar na Ferida

**Conteudo**:
- Titulo: "Sente-se culpado ou mal por isso?"
- Imagem: Expressao de frustacao ou preocupacao

**Opcoes**:
1. "Sim, e muito"
2. "Sim"

**Logica de navegacao**:
- Ambas as opcoes levam para a Tela 9

**Objetivo psicologico**:
O lead confirma emocionalmente que o problema o afeta. A ausencia de opcao negativa forca o reconhecimento da culpa, que e um gatilho emocional poderoso para a urgencia de resolver.
```

---

### Exemplo: Tela 13 + Tela 14 (Objetivo Especifico + Promessa Dinamica)

```markdown
### Tela 13: Objetivo Especifico

**Tipo**: escolha-unica
**Etapa**: 7 — Depoimentos (personalizacao)

**Conteudo**:
- Titulo: "Qual e o seu maior objetivo na leitura?"
- Subtitulo: "Selecione uma das opcoes"

**Opcoes**:
1. "Multiplicar a velocidade de leitura"
2. "Aumentar o foco e compreensao"
3. "Memorizar qualquer conteudo"
4. "Reduzir o sono e cansaco durante a leitura"

**Logica de navegacao**:
- Todas as opcoes levam para a Tela 14
- IMPORTANTE: Salvar a resposta como variavel {{resposta_tela_13}} para uso na Tela 14

**Objetivo psicologico**:
O lead escolhe 1 resultado especifico. Essa escolha sera espelhada de volta na proxima tela como uma promessa personalizada — o que cria o maior nivel de conexao emocional do quiz.

---

### Tela 14: Promessa Personalizada

**Tipo**: apresentacao
**Etapa**: 9 — Prometer Ajuda

**Conteudo**:
- Titulo: "Nos iremos te ajudar!"
- Corpo: "Voce encontrara no Metodo MIL, exercicios para **{{resposta_tela_13}}** sem precisar de mais de 15 minutos por dia"
  - Nota de implementacao: Configurar no Inlead para que {{resposta_tela_13}} seja substituida pela opcao escolhida na Tela 13
- Imagem: Imagem do produto ou representacao visual da transformacao
- Botao: "Ver meu plano personalizado"

**Logica de navegacao**:
- Botao leva para a Tela 15

**Objetivo psicologico**:
A promessa usa exatamente o resultado que o lead disse querer na tela anterior. Isso cria a sensacao de que o produto foi feito especificamente para ele — o maior gatilho de conversao do quiz.
```
