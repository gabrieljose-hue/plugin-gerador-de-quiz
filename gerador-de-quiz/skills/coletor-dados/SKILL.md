---
name: coletor-dados
description: Coleta todas as informacoes necessarias para gerar o quiz via perguntas interativas
---

# Skill: Coletor de Dados

## Missao

Coletar todas as informacoes sobre o produto, publico e estrategia de conversao para que o criador-quiz possa gerar um quiz personalizado e eficaz.

---

## Entrada (INPUT)

Nenhuma entrada previa. A skill faz perguntas interativas via AskUserQuestion.

---

## Saida (OUTPUT)

### Operacao Interna
Salvar JSON em: `output/dados-quiz.json`

### Retorno ao Chat
Resumo de confirmacao:
```
Dados coletados e salvos em output/dados-quiz.json

Resumo:
  • Tema: [TEMA]
  • Produto: [PRODUTO]
  • Publico: [PUBLICO]
  • Dores coletadas: [N]
  • Desejos coletados: [N]
  • Depoimentos: [N]
```

**NAO emitir o JSON completo no chat.**

---

## Processo Interativo (3 Rodadas)

**OBRIGATORIO**: Todas as perguntas usam a ferramenta `AskUserQuestion`. Maximo de 4 perguntas por chamada.

---

### RODADA 1: Produto e Oferta (4 perguntas)

**Chamar AskUserQuestion com 4 perguntas:**

**1. Qual e o tema do quiz?**
- Header: "Tema do Quiz"
- Descricao: O assunto central que vai atrair o lead. Ex: "Leitura rapida", "Emagrecimento", "Vendas online", "Ansiedade"
- Tipo: other (campo de texto livre)
- Exemplo: "Leitura rapida e memorização"

**2. Qual e o nome do produto que sera vendido no quiz?**
- Header: "Produto"
- Descricao: Nome exato do produto que aparecera na pagina de vendas interna
- Tipo: other (campo de texto livre)
- Exemplo: "Metodo MIL de Leitura"

**3. Qual e o preco do produto?**
- Header: "Preco"
- Descricao: Valor que sera exibido na pagina de vendas interna. Ex: "R$ 97", "R$ 297", "12x de R$ 19,90"
- Tipo: other (campo de texto livre)
- Exemplo: "R$ 197"

**4. Qual e o link do checkout (pagina de compra)?**
- Header: "Link Checkout"
- Descricao: URL para onde o botao de compra vai redirecionar. Pode ser deixado em branco se ainda nao tem.
- Tipo: other (campo de texto livre)
- Exemplo: "https://checkout.hotmart.com/produto123"

---

### RODADA 2: Publico e Prova Social (4 perguntas)

**Chamar AskUserQuestion com 4 perguntas:**

**5. Quem e o publico-alvo deste quiz?**
- Header: "Publico-Alvo"
- Descricao: Descreva o perfil da pessoa que vai responder o quiz. Idade, situacao atual, nivel de conhecimento, principal dor.
- Tipo: other (campo de texto livre)
- Exemplo: "Adultos de 25-50 anos que querem ler mais rapido mas se distraem facilmente. Ja tentaram metodos tradicionais sem sucesso."

**6. O quiz vai segmentar por genero?**
- Header: "Generos"
- Descricao: Se sim, a tela de abertura vai perguntar o genero e as proximas telas terao versoes masculina e feminina com fotos diferentes.
- Tipo: single select
- Opcoes:
  - "Sim, segmentar por genero" (description: "A tela de abertura pergunta o genero e personaliza as proximas telas")
  - "Nao, quiz unico para todos" (description: "Sem segmentacao por genero, perguntas iguais para todos")

**7. Quantos alunos, clientes ou usuarios o produto ja tem?**
- Header: "Numeros Sociais"
- Descricao: Numero que sera exibido na tela de prova social. Ex: "+1.000 alunos", "+500 clientes", "+2.000 usuarios"
- Tipo: other (campo de texto livre)
- Exemplo: "+1.000 alunos"

**8. Qual e a principal transformacao que o produto promete?**
- Header: "Transformacao Principal"
- Descricao: O resultado principal que o lead vai alcancar ao usar o produto. Vai aparecer em varias telas do quiz.
- Tipo: other (campo de texto livre)
- Exemplo: "Ler 3x mais rapido sem perder a compreensao em 30 dias"

---

### RODADA 3: Dores, Desejos e Depoimentos (3 chamadas separadas)

#### Rodada 3.1: Dores (4 perguntas de uma vez)

**Chamar AskUserQuestion com 4 perguntas para coletar as principais dores:**

**9. Qual e a dor/problema numero 1 do seu publico?**
- Header: "Dor Principal"
- Descricao: O maior problema que o lead sente hoje. O que o incomoda mais?
- Tipo: other
- Exemplo: "Se distrai facilmente durante a leitura e precisa reler varias vezes"

**10. Quais sao as consequencias negativas mais comuns desse problema?**
- Header: "Consequencias Negativas"
- Descricao: O que acontece de ruim por causa do problema? Liste de 3 a 6 consequencias separadas por virgula.
- Tipo: other
- Exemplo: "Nao consegue lembrar onde parou, perde tempo relendo, lista de leituras so aumenta, nao cresce na carreira, abandona livros pela metade, sente ansiedade"

**11. Quais sao os principais objetivos/desejos do seu publico?**
- Header: "Desejos Principais"
- Descricao: O que o lead mais quer conquistar? Liste de 3 a 5 desejos separados por virgula.
- Tipo: other
- Exemplo: "Aprender mais rapido, ser mais inteligente, passar em concursos, melhorar no trabalho, aprender novo idioma"

**12. Voce tem depoimentos de clientes ou alunos?**
- Header: "Depoimentos"
- Descricao: Selecione quantos depoimentos voce tem disponiveis agora.
- Tipo: single select
- Opcoes:
  - "Sim, tenho 3 ou mais" (description: "Tenho varios resultados de clientes/alunos para mostrar")
  - "Sim, tenho 1 ou 2" (description: "Tenho alguns resultados iniciais")
  - "Ainda nao tenho" (description: "Produto novo ou ainda sem depoimentos coletados")

**Se respondeu "Sim, tenho 3 ou mais" ou "Sim, tenho 1 ou 2":**
Chamar AskUserQuestion com 1 pergunta adicional:
- Header: "Texto dos Depoimentos"
- Descricao: Cole os textos dos seus depoimentos. Formato: [Nome] — [Resultado especifico]. Um por linha.
- Tipo: other
- Exemplo: "Maria S. — Comecei a ler 2x mais rapido em 3 semanas.\nJoao R. — Finalmente consegui terminar os livros que comecei."

**Se respondeu "Ainda nao tenho":**
Registrar `depoimentos_quantidade = 0` e `depoimentos = []`. O criador-quiz vai omitir as telas de depoimento.

---

**[NOVA] Rodada 3.2: Personalizacao e objetivos especificos (2 perguntas)**

**Chamar AskUserQuestion com 2 perguntas:**

**13. Quais sao os 4 resultados especificos que o seu produto entrega?**
- Header: "Objetivos Especificos do Produto"
- Descricao: Esses 4 resultados serao as opcoes da Tela 13 do quiz (a que personaliza a promessa final). Devem ser especificos ao que o produto faz — mais concretos que os desejos gerais. Separe por | (pipe).
- Tipo: other
- Exemplo: "Multiplicar a velocidade de leitura | Aumentar o foco e compreensao | Memorizar qualquer conteudo | Reduzir o sono e cansaco durante a leitura"

**14. Como o seu produto facilita o resultado para o lead — qual e a barreira que ele elimina?**
- Header: "Ampliar Oportunidade"
- Descricao: Uma frase curta que mostra que e facil/acessivel conseguir o resultado. Aparece na promessa personalizada da Tela 14: "Voce encontrara no [produto] exercicios para [objetivo escolhido] SEM OU COM APENAS [o que voce escrever aqui]".
- Tipo: other
- Exemplo: "15 minutos por dia" | "experiencia anterior" | "muito tempo livre" | "equipamentos caros"

---

## Estrutura obrigatoria do JSON salvo

```json
{
  "produto": {
    "tema": "string",
    "nome": "string",
    "preco": "string",
    "link_checkout": "string",
    "transformacao_principal": "string"
  },
  "publico": {
    "descricao": "string",
    "segmentar_genero": true,
    "numeros_sociais": "string"
  },
  "dores": {
    "principal": "string",
    "consequencias": ["array de strings — consequencias negativas do problema"]
  },
  "desejos": ["array de strings — principais objetivos do lead"],
  "depoimentos_quantidade": "string — '3+', '1-2' ou '0'",
  "depoimentos": [
    {
      "autor": "string",
      "texto": "string"
    }
  ],
  "campos_ausentes": ["array de campos nao preenchidos"]
}
```

---

## Regra: Nunca inventar

Se o usuario deixar um campo em branco:
- Salvar como string vazia `""` ou array vazio `[]`
- Adicionar ao array `campos_ausentes[]`
- O criador-quiz usara `[INSERIR X]` como placeholder

---

## Validacoes antes de salvar

- [ ] Tema preenchido
- [ ] Nome do produto preenchido
- [ ] Pelo menos 1 dor coletada
- [ ] Pelo menos 2 consequencias negativas coletadas
- [ ] Pelo menos 2 desejos coletados
- [ ] Transformacao principal preenchida

Se alguma validacao falhar, usar AskUserQuestion para pedir o campo faltante antes de salvar.

---

## Procedimento

1. Chamar AskUserQuestion — Rodada 1 (4 perguntas sobre produto e oferta)
2. Aguardar respostas
3. Chamar AskUserQuestion — Rodada 2 (4 perguntas sobre publico e prova social)
4. Aguardar respostas
5. Chamar AskUserQuestion — Rodada 3.1 (4 perguntas sobre dores, desejos e depoimentos)
6. Aguardar respostas
7. Montar JSON com todos os dados coletados
8. Validar campos obrigatorios (se faltou algo, pedir via AskUserQuestion antes de salvar)
9. Salvar em `output/dados-quiz.json`
10. Retornar resumo ao chat (3-5 linhas, sem JSON completo)
