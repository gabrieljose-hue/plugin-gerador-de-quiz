---
name: extrator-dados
description: Le o copy/briefing colado pelo usuario e extrai os dados estruturados necessarios para gerar o quiz
---

# Skill: Extrator de Dados

## Missao

Ler o copy ou briefing colado pelo usuario e extrair todos os campos necessarios para gerar o quiz, populando o `output/dados-quiz.json` com o maximo de informacao possivel a partir do texto fornecido.

---

## Entrada (INPUT)

- `COPY_BRUTO`: Texto colado pelo usuario no chat (copy de vendas, briefing do produto, descricao, landing page, roteiro, etc.)

---

## Saida (OUTPUT)

### Operacao Interna
Salvar JSON em: `output/dados-quiz.json`

### Retorno ao Chat
```
Dados extraidos do copy e salvos em output/dados-quiz.json

Resumo da extracao:
  • Produto: [nome identificado ou nao encontrado]
  • Tema: [tema identificado ou nao encontrado]
  • Dores encontradas: [N]
  • Desejos encontrados: [N]
  • Depoimentos encontrados: [N]
  • Campos ausentes: [lista resumida]

Passando para revisao dos dados...
```

**NAO emitir o JSON completo no chat.**

---

## Campos que devem ser extraidos do copy

Ler o texto com atencao e identificar:

### Do produto
- `tema`: O assunto central do quiz/produto (ex: "Leitura rapida", "Emagrecimento")
- `nome`: Nome do produto, metodo, curso, ferramenta
- `preco`: Preco mencionado (ex: "R$ 197", "12x de R$ 19,90")
- `link_checkout`: URL de compra, se mencionado
- `transformacao_principal`: O resultado/promessa principal que o produto entrega
- `logo_url`: URL da logo do produto, se mencionada no copy (nao inferir — adicionar a `campos_ausentes` se nao encontrada)

### Do publico
- `descricao`: Perfil do publico-alvo (quem e, situacao atual, nivel de conhecimento)
- `segmentar_genero`: Inferir se o produto e direcionado a um genero ou ambos — `true` se for ambos ou nao especificado
- `numeros_sociais`: Numero de alunos/clientes/usuarios mencionado (ex: "+1000 alunos")

### Das dores
- `principal`: A maior dor/problema do publico descrita no copy
- `consequencias`: Lista de consequencias negativas do problema mencionadas no copy

### Dos desejos
- Lista de objetivos/desejos do publico mencionados no copy

### Dos depoimentos
- Detectar se ha depoimentos mencionados no copy (ex: "mais de 1000 alunos", citacoes de clientes)
- **Nunca extrair texto de depoimentos** — depoimentos usam imagens (prints), nao texto
- Se o copy menciona depoimentos existentes: definir `depoimentos_quantidade` conforme a quantidade aparente e adicionar `"depoimentos_imagens"` ao `campos_ausentes`
- As URLs das imagens serao coletadas pelo revisor-dados

### Personalizacao da promessa
- `objetivos_especificos`: Os 4 resultados concretos que o produto entrega (para a Tela 13)
  - Se o copy nao lista 4 resultados especificos, inferir a partir da transformacao principal e beneficios descritos
- `amplificar_oportunidade`: Como o produto reduz a barreira de resultado (ex: "sem precisar de muito tempo", "mesmo sem experiencia")
  - Se nao mencionado explicitamente, inferir a partir do tom do copy (ex: se fala "metodo simples" → "sem complicacao")
- `entregaveis`: Lista de modulos, bonus e materiais que o produto entrega, com valores se mencionados
  - Preservar exatamente como descrito no copy (nao resumir)
  - Se nao encontrado no copy → `"[INSERIR ENTREGAVEIS]"`
  - Adicionar `"entregaveis"` em `campos_ausentes[]` se nao encontrado

---

## Regras de Extracao

### Regra 1: Extrair o que esta explicito
Se a informacao esta no texto, extrair fielmente — nao alterar, nao resumir demais.

### Regra 2: Inferir com contexto quando razoavel
Se um campo nao esta explicito mas e claramente inferivel do contexto, inferir e marcar como `"inferido: true"` no JSON interno.

Exemplos de inferencia valida:
- Copy sobre "emagrecimento feminino sem academia" → `segmentar_genero: true` (feminino)
- Copy diz "metodo de 3 passos rapidos" → `amplificar_oportunidade: "sem precisar de muito tempo"`
- Copy lista 5 beneficios especificos → usar os 4 mais relevantes como `objetivos_especificos`

### Regra 3: Nunca inventar dados criticos
Os seguintes campos NAO podem ser inferidos — so extrair se estiverem no texto:
- `preco`: Nunca inventar preco
- `link_checkout`: Nunca inventar link
- `numeros_sociais`: Nunca inventar numeros
- `depoimentos`: Nunca inventar depoimentos

Se ausentes, deixar vazios e adicionar ao `campos_ausentes`.

---

## Estrutura do JSON gerado

```json
{
  "origem": "copy",
  "produto": {
    "tema": "string",
    "nome": "string",
    "preco": "string",
    "link_checkout": "string",
    "transformacao_principal": "string",
    "logo_url": "string",
    "objetivos_especificos": ["string", "string", "string", "string"],
    "amplificar_oportunidade": "string",
    "entregaveis": "string"
  },
  "publico": {
    "descricao": "string",
    "segmentar_genero": true,
    "numeros_sociais": "string"
  },
  "dores": {
    "principal": "string",
    "consequencias": ["string", "string", "string"]
  },
  "desejos": ["string", "string", "string"],
  "depoimentos": [],
  "depoimentos_quantidade": "string — '3+', '1-2' ou '0' (inferido do copy; imagens serao coletadas pelo revisor-dados)",
  "imagens_quiz": [],
  "campos_ausentes": ["lista de campos nao encontrados nem inferidos"]
}
```

---

## REGRA DE PERGUNTAS (OBRIGATORIO)

**Sempre usar a ferramenta `AskUserQuestion`** para qualquer pergunta, escolha ou coleta de input do usuario. Em todos os casos, sem excecao. Nunca fazer perguntas apenas no texto do chat.

---

## Procedimento

1. Ler o `COPY_BRUTO` completo
2. Extrair cada campo seguindo as 3 regras acima
3. Para `objetivos_especificos`: garantir exatamente 4 itens — se o copy tiver mais, escolher os 4 mais especificos e acionaveis; se tiver menos, inferir a partir dos beneficios e transformacao principal
4. Montar o JSON completo
5. Listar todos os campos que ficaram vazios (nao extraidos nem inferidos) em `campos_ausentes`
6. Salvar em `output/dados-quiz.json`
7. Retornar resumo ao chat
