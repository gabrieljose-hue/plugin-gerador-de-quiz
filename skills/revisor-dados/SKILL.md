---
name: revisor-dados
description: Valida e ajusta os dados coletados (de qualquer origem) antes de gerar o quiz. Atua proativamente para preencher lacunas e garantir qualidade.
---

# Skill: Revisor de Dados

## Missao

Validar o `output/dados-quiz.json` — seja ele proveniente do coletor-dados (usuario respondeu perguntas) ou do extrator-dados (usuario colou copy). Ajustar proativamente o que for possivel sem perguntar ao usuario. **Sempre usar `AskUserQuestion`** quando precisar de input do usuario — nunca perguntar apenas no chat. Perguntar APENAS o que for critico e impossivel de inferir.

---

## Entrada (INPUT)

- `output/dados-quiz.json` (lido do arquivo)

---

## Saida (OUTPUT)

### Operacao Interna
- Validar cada campo obrigatorio
- Ajustar/completar campos que podem ser melhorados sem perguntar ao usuario
- Salvar versao revisada em `output/dados-quiz.json` (sobrescreve)

### Retorno ao Chat

**Se APROVADO sem ajustes**:
```
Dados validados e aprovados!

Todos os campos obrigatorios estao presentes e adequados.
```

**Se APROVADO com ajustes proativos**:
```
Dados validados com ajustes automaticos.

Ajustes realizados:
  [AJUSTE] Campo: [descricao do que foi ajustado e por que]
```

**Se precisa de input do usuario (so para campos criticos)**:
Usar `AskUserQuestion` com as perguntas minimas necessarias para completar os campos criticos.
Depois de receber as respostas, aprovar e seguir.

---

## Campos Obrigatorios e Criterios

### CRITICOS (sem esses, o quiz nao pode ser gerado)

| Campo | Criterio | Acao se ausente |
|-------|----------|-----------------|
| `produto.tema` | Presente e claro | AskUserQuestion |
| `produto.nome` | Presente | AskUserQuestion |
| `produto.transformacao_principal` | Presente e especifica | Inferir do nome + tema, se possivel. Se nao, AskUserQuestion |
| `dores.principal` | Presente | AskUserQuestion |
| `dores.consequencias` | Minimo 5 itens | Se < 5: completar com inferencias do contexto do tema |
| `desejos` | Minimo 4 itens | Se < 4: completar com inferencias do tema e transformacao |
| `produto.objetivos_especificos` | Exatamente 4 itens | Se < 4: inferir do produto; se vazio: AskUserQuestion |

### IMPORTANTES (afetam qualidade mas nao bloqueiam)

| Campo | Criterio | Acao se ausente |
|-------|----------|-----------------|
| `produto.preco` | Presente | Deixar como "[INSERIR PRECO]" — nao perguntar |
| `produto.link_checkout` | Presente | Deixar como "[INSERIR LINK]" — nao perguntar |
| `produto.logo_url` | URL da logo do produto | Se ausente: deixar como "" (vazio) — nao perguntar. A logo e opcional |
| `publico.numeros_sociais` | Presente | Inferir forma generica: "+centenas de alunos" — nao perguntar |
| `produto.amplificar_oportunidade` | Presente — frase curta que elimina a barreira para o resultado (ex: "15 minutos por dia", "sem experiencia anterior") | Inferir do contexto (ver Regra 4) — nao perguntar |
| `produto.entregaveis` | Presente (string com itens separados por `\|`) | Se ausente ou `"[INSERIR ENTREGAVEIS]"`: registrar em `campos_ausentes` mas nao perguntar ao usuario — nao inferir |
| `depoimentos_quantidade` | Presente ("0", "1-2" ou "3+") | Se ausente, definir como "0" — nao inferir |
| `depoimentos` | Array de `{ imagem_url }` condicionado por `depoimentos_quantidade` | Se quantidade != "0" mas array vazio: usar AskUserQuestion para pedir as URLs das imagens (ver Regra 8) |

---

## Regras de Ajuste Proativo

### Regra 1: Completar `dores.consequencias` se insuficiente

Se houver menos de 5 consequencias, inferir as faltantes a partir do `dores.principal` e `produto.tema`.

**Exemplo**:
- Tema: "Leitura rapida"
- Dor principal: "Se distrai durante a leitura"
- Consequencias ja existentes: 3
- Inferir mais 2: "A lista de leituras pendentes so aumenta" | "Sinto ansiedade por ter muito conteudo para ler e pouco tempo"

**Regra**: As consequencias inferidas devem ser:
- Especificas ao tema (nao genericas)
- Escritas em primeira pessoa
- Verossimeis para o publico descrito

---

### Regra 2: Completar `desejos` se insuficiente

Se houver menos de 4 desejos, inferir a partir da `transformacao_principal` e do contexto do tema.

**Exemplo**:
- Transformacao: "Ler 3x mais rapido em 30 dias"
- Desejos ja existentes: 2
- Inferir mais 2: "Aprender mais em menos tempo" | "Melhorar o desempenho no trabalho"

---

### Regra 3: Completar `produto.objetivos_especificos` se incompleto

Se houver menos de 4 objetivos especificos, completar a partir dos beneficios mencionados ou da transformacao principal.

Os 4 objetivos devem ser:
- Especificos e acionaveis (nao genericos)
- Diretamente ligados ao que o produto entrega
- Diferentes entre si (cada um e um resultado distinto)

**Exemplo** (tema: leitura):
- Se existir apenas 2: inferir os outros 2 como aspectos diferentes do mesmo tema
  - "Memorizar o que leu com mais facilidade" | "Reduzir o cansaco durante a leitura"

---

### Regra 4: Inferir `amplificar_oportunidade` se ausente

Inferir baseado em padroes do tema:

| Tema / Contexto | Inferencia padrao |
|-----------------|-------------------|
| Metodo com passos | "sem precisar de muito tempo por dia" |
| Produto para iniciantes | "mesmo sem experiencia anterior" |
| Produto online/digital | "de qualquer lugar, no seu ritmo" |
| Produto de habitos | "sem precisar mudar toda a sua rotina" |
| Produto de saude/bem-estar | "sem dietas radicais ou exercicios intensos" |
| Generico | "sem complicacao" |

---

### Regra 5: Normalizar `numeros_sociais` se ausente

Se nao foi informado nenhum numero social, usar forma generica sem inventar:
- `"centenas de alunos"` ou `"uma comunidade crescente de alunos"`

Nunca inventar numeros especificos (ex: nao colocar "+500 alunos" se nao foi informado).

---

### Regra 6: `segmentar_genero` e `incluir_idade` sao determinados pelo criador-quiz

O revisor-dados NAO precisa definir esses campos — o criador-quiz faz a auto-deteccao no PASSO 0 antes de gerar as telas. O revisor-dados pode ignorar esses campos.

### Regra 7: Validar `depoimentos_quantidade` vs array `depoimentos`

Se `depoimentos_quantidade` esta ausente: definir como `"0"`.

Se `depoimentos_quantidade = "0"`: garantir que `depoimentos = []` (array vazio).

Se `depoimentos_quantidade` != `"0"` e `depoimentos = []` (array vazio): aplicar Regra 8.

### Regra 8: Coletar imagens de depoimentos quando ausentes

Se `depoimentos_quantidade` indica que existem depoimentos ("1-2" ou "3+") mas o array `depoimentos` esta vazio (caso tipico quando o usuario veio pelo caminho do extrator-dados):

Usar `AskUserQuestion` com 1 pergunta:
- Header: "Imagens dos Depoimentos"
- Descricao: "Seu produto tem depoimentos de clientes. Cole as URLs das imagens (prints de WhatsApp, Instagram, Google, etc.). Uma URL por linha. Deixe em branco para pular."
- Tipo: other

**EXCECAO — Colar URLs diretamente no chat**: Se o usuario responder algo como "vou colar abaixo", "cola aqui", "vou digitar" ou qualquer variacao indicando que vai colar o conteudo diretamente no chat, NAO usar `AskUserQuestion`. Em vez disso, responder apenas com uma frase curta no chat pedindo que cole as URLs (ex: "Pode colar as URLs aqui:") e aguardar a resposta do usuario. Apos receber as URLs no chat, voltar a usar `AskUserQuestion` normalmente para tudo.

**Se o usuario informar URLs**: salvar cada URL como `{ "imagem_url": "string" }` no array `depoimentos` e ajustar `depoimentos_quantidade` conforme a quantidade recebida.

**Se o usuario deixar em branco**: manter `depoimentos = []` e redefinir `depoimentos_quantidade = "0"`. O criador-quiz vai omitir as telas de depoimento.

**Esta pergunta conta como 1 das 3 perguntas criticas permitidas ao revisor.**

---

## Quando Perguntar ao Usuario

**Sempre** usar a ferramenta `AskUserQuestion` — nunca perguntar no texto do chat. Usar `AskUserQuestion` APENAS para os campos criticos que estao ausentes E nao podem ser inferidos com seguranca:

- `produto.tema`: Se completamente ausente
- `produto.nome`: Se completamente ausente — usar AskUserQuestion com sugestoes de exemplos de nomes de produto (ex: "Metodo MIL de Leitura", "Programa Corpo em Forma 30 Dias", "Curso Vendas do Zero ao Pro", "Imersao Mente de Alta Performance") — nunca usar "Curso", "Mentoria" ou "Metodo" isolados como sugestao, pois sao tipos e nao nomes
- `dores.principal`: Se completamente ausente

**Maximo de 3 perguntas** em uma unica chamada de `AskUserQuestion`. Nunca fazer mais de uma rodada de perguntas.

---

## Checklist Final de Aprovacao

Antes de aprovar, verificar:

- [ ] `produto.tema` preenchido
- [ ] `produto.nome` preenchido
- [ ] `produto.transformacao_principal` preenchida
- [ ] `dores.principal` preenchida
- [ ] `dores.consequencias` com 5 ou mais itens
- [ ] `desejos` com 4 ou mais itens
- [ ] `produto.objetivos_especificos` com exatamente 4 itens
- [ ] `produto.amplificar_oportunidade` preenchido
- [ ] `produto.entregaveis` presente (se ausente: registrar em `campos_ausentes`, nao bloquear aprovacao)
- [ ] `publico.segmentar_genero` definido (true/false)
- [ ] `publico.numeros_sociais` preenchido (pode ser generico)

Todos os checks passando → APROVADO, seguir para criador-quiz.

---

## Procedimento

1. Ler `output/dados-quiz.json`
2. Verificar cada campo do checklist
3. Para cada campo com problema: aplicar a regra de ajuste proativo correspondente
4. Se houver campo critico ausente e nao inferivel: usar `AskUserQuestion` (maximo 3 perguntas, 1 rodada)
5. Salvar o JSON revisado/ajustado em `output/dados-quiz.json`
6. Retornar ao chat: APROVADO + lista de ajustes feitos (se houver) — sem anunciar "Gerando quiz..." nem qualquer outra mensagem de transicao
7. Seguir automaticamente para o criador-quiz sem pausar nem aguardar input do usuario
