---
name: revisor-quiz
description: Revisa o quiz gerado validando qualidade de conversao, fluxo psicologico e as 18 telas obrigatorias
---

# Skill: Revisor de Quiz

## Missao

Ler o quiz gerado e validar se ele esta correto estruturalmente e tem qualidade para converter leads em compradores. O revisor conhece o framework de 18 telas e verifica cada ponto critico.

---

## Entrada (INPUT)

- `output/quiz.json` (lido do arquivo)
- `output/dados-quiz.json` (lido do arquivo — para comparar com o que foi gerado)

---

## Saida (OUTPUT)

### Retorno ao Chat

**Se APROVADO**:
```
Quiz aprovado para implementacao!

Checklist:
  [OK] 18 telas presentes e em ordem correta
  [OK] Tela 8 apenas com opcoes afirmativas ("Sim e muito" / "Sim")
  [OK] Tela 13 com 4 objetivos especificos do produto
  [OK] Tela 14 referencia {{resposta_tela_13}} dinamicamente
  [OK] {{nome}} apenas a partir da Tela 6
  [OK] Tela 9 com minimo de 5 consequencias negativas
  [OK] Tela 10 com minimo de 4 desejos
  [OK] Tela 15 apenas com opcoes positivas (micro-compromisso)
  [OK] Tela 18 com os 8 blocos da pagina de vendas
  [OK] Tom progressivo correto

O quiz esta pronto! Siga as instrucoes em output/quiz.md para implementar no Inlead.
```

**Se AJUSTES NECESSARIOS**:
```
Quiz precisa de ajustes antes de ser implementado.

Problemas encontrados:
  [PROBLEMA] Tela X: [descricao do problema]
  [PROBLEMA] Tela Y: [descricao do problema]

Acoes necessarias:
  - [acao especifica para corrigir cada problema]

O criador-quiz vai corrigir as telas indicadas.
```

**Se REPROVADO**:
```
Quiz reprovado. Problemas criticos:

  [CRITICO] [descricao]

E necessario regerar o quiz completo.
```

---

## Checklist de Validacao (executar em ordem)

### 1. Completude Estrutural (critico)

Primeiro, ler o campo `meta` do `quiz.json` para saber quais telas sao esperadas:
- `meta.incluir_genero` — define se existem telas 2A/2B ou tela 2
- `meta.incluir_idade` — define se a tela 2 tem opcoes de faixa etaria
- `meta.telas_depoimentos` — define se existem telas 11 (e 11B)

**Telas SEMPRE obrigatorias (independente das configuracoes)**:
- [ ] Tela 1 existe (abertura)
- [ ] Tela 3 existe (prova social)
- [ ] Telas 4, 6, 7 existem (3 perguntas de diagnostico)
- [ ] Tela 5 existe (captura nome + email)
- [ ] **Tela 8 existe** (tocar na ferida 1 — "Sente-se culpado?")
- [ ] **Tela 9 existe** (tocar na ferida 2 — consequencias multipla-escolha)
- [ ] Tela 10 existe (desejo principal)
- [ ] **Tela 13 existe** (objetivo especifico — escolha-unica)
- [ ] **Tela 14 existe** (promessa com {{resposta_tela_13}})
- [ ] Tela 15 existe (micro-compromisso)
- [ ] Tela 16 existe (carregamento)
- [ ] Tela 17 existe (resultado com grafico)
- [ ] Tela 18 existe (pagina de vendas interna)

**Telas CONDICIONAIS** — validar de acordo com o `meta`:
- [ ] Se `incluir_genero = true` E `incluir_idade = true`: Telas 2A e 2B existem com opcoes de idade
- [ ] Se `incluir_genero = true` E `incluir_idade = false`: Tela 2 existe (so genero, sem idade)
- [ ] Se `incluir_genero = false`: Nao deve existir nenhuma tela 2 (e correto pular)
- [ ] Se `telas_depoimentos >= 1`: Telas 11 e 12 existem
- [ ] Se `telas_depoimentos = 2`: Tela 11B existe com depoimentos adicionais
- [ ] Se `telas_depoimentos = 0`: Telas 11 e 12 NAO existem (e correto pular — a Tela 10 leva direto para a Tela 13)

**Falha critica**: Qualquer tela OBRIGATORIA faltando → REPROVADO
**Falha critica**: Tela condicional presente quando deveria estar ausente (ex: tela de genero para produto especifico de um genero) → AJUSTE NECESSARIO

---

### 2. Tela 8 — Regra das Opcoes Apenas Afirmativas (critico)

- [ ] A tela 8 e do tipo `escolha-unica`
- [ ] Tem EXATAMENTE 2 opcoes
- [ ] Opcao 1 e "Sim, e muito" (ou variacao afirmativa de mesmo sentido)
- [ ] Opcao 2 e "Sim" (ou variacao afirmativa mais branda)
- [ ] **NAO existe opcao negativa** (sem "Nao", "Nunca", "Raramente")
- [ ] Ambas as opcoes levam para a Tela 9

**Falha critica**: Opcao negativa na Tela 8 → AJUSTE NECESSARIO

---

### 3. Tela 13 e 14 — Personalizacao Dinamica (critico)

- [ ] Tela 13 e do tipo `escolha-unica`
- [ ] Tela 13 tem exatamente 4 opcoes de objetivos especificos do produto
- [ ] As opcoes da Tela 13 sao especificas ao produto (nao genericas)
- [ ] Tela 14 referencia explicitamente `{{resposta_tela_13}}` no corpo do texto
- [ ] Tela 14 contem instrucao para configurar a variavel dinamica no Inlead
- [ ] Tela 14 usa {{nome}} no titulo

**Falha critica**: Tela 14 sem `{{resposta_tela_13}}` → AJUSTE NECESSARIO

---

### 4. Uso Correto de {{nome}}

- [ ] {{nome}} NAO aparece nas Telas 1, 2A, 2B, 3, 4, 5
- [ ] {{nome}} aparece OBRIGATORIAMENTE na Tela 6 (titulo: "Ola {{nome}}, ...")
- [ ] {{nome}} aparece na Tela 12
- [ ] {{nome}} aparece na Tela 14
- [ ] {{nome}} aparece na Tela 18 (pagina de vendas)

**Falha**: {{nome}} antes da Tela 5 → AJUSTE NECESSARIO

---

### 5. Tela 9 — Qualidade das Consequencias

- [ ] Tipo e `multipla-escolha`
- [ ] Tem minimo de 5 opcoes de consequencias
- [ ] Todas as opcoes estao em PRIMEIRA PESSOA ("Nao consigo...", "Perco...", "Sinto...")
- [ ] As consequencias vieram de `dores.consequencias` dos dados coletados (nao inventadas)
- [ ] Tom e empatico, nao julgador

**Falha**: Menos de 5 opcoes → AJUSTE NECESSARIO

---

### 6. Tela 10 — Qualidade dos Desejos

- [ ] Tipo e `multipla-escolha`
- [ ] Tem minimo de 4 opcoes de desejos
- [ ] Escritas de forma POSITIVA e aspiracional
- [ ] Os desejos vieram de `desejos` dos dados coletados (nao inventados)

**Falha**: Menos de 4 opcoes ou tom negativo → AJUSTE NECESSARIO

---

### 7. Tela 15 — Micro-Compromisso sem Opcao Negativa

- [ ] Tipo e `escolha-unica`
- [ ] Tem exatamente 3 opcoes
- [ ] Todas as 3 opcoes mostram motivacao positiva
- [ ] **NAO existe opcao que leva o lead a desistir** ("Nao tenho interesse", "Talvez outro dia")
- [ ] Todas as opcoes levam para a Tela 16

**Falha critica**: Opcao de desistencia no micro-compromisso → AJUSTE NECESSARIO

---

### 8. Tela 18 — Pagina de Vendas Completa

Verificar os 8 blocos obrigatorios:
- [ ] Bloco 1: Comparativo visual Antes (sem produto) vs Depois (com produto)
- [ ] Bloco 2: Headline "{{nome}}, o seu plano personalizado esta pronto!"
- [ ] Bloco 3: Lista de entregaveis com ancoragem de preco
- [ ] Bloco 4: Preco visivel + botao CTA com link de checkout
- [ ] Bloco 5: Comparativo de vida (sem vs com produto)
- [ ] Bloco 6: Depoimentos (ou placeholders)
- [ ] Bloco 7: Segundo botao CTA
- [ ] Bloco 8: Garantia de reembolso

**Falha critica**: Sem preco ou sem botao de compra → AJUSTE NECESSARIO

---

### 9. Fluxo Psicologico e Sequencia das Etapas

Validar que a progressao emocional esta correta:

- [ ] Diagnostico (telas 4, 6, 7) antes de Tocar na Ferida (telas 8, 9)
- [ ] Desejo (tela 10) logo APOS a Ferida (tela 9) — do problema para a solucao
- [ ] Depoimentos (tela 11) APOS o desejo — "outras pessoas resolveram"
- [ ] Objetivo especifico (tela 13) e ANTES da Promessa (tela 14)
- [ ] Micro-compromisso (tela 15) e ANTES do Carregamento (tela 16)
- [ ] Resultado (tela 17) e ANTES da Pagina de Vendas (tela 18)

**Falha critica**: Sequencia incorreta → REPROVADO

---

### 10. Perguntas de Diagnostico (Telas 4, 6, 7)

- [ ] Tela 4: pergunta sobre a dor principal
- [ ] Tela 6: pergunta DIFERENTE da Tela 4 (outra dimensao do problema) + usa {{nome}}
- [ ] Tela 7: pergunta DIFERENTE das Telas 4 e 6 (terceira dimensao)
- [ ] Cada pergunta tem 4 opcoes de frequencia/intensidade
- [ ] Nenhuma pergunta se repete

**Falha**: Perguntas repetidas → AJUSTE NECESSARIO

---

### 11. Tom e Linguagem

- [ ] Sem emojis no conteudo das telas
- [ ] Lingua portuguesa natural, sem jargoes
- [ ] Tom neutro nas telas 1-5
- [ ] Tom empatico nas telas 6-11
- [ ] Tom motivador/urgente nas telas 12-15
- [ ] Tom otimista na tela 17
- [ ] Tom persuasivo na tela 18

---

## Classificacao Final

### APROVADO
- Todos os checks criticos passaram
- Maximo de 2 checks menores falharam

### AJUSTES NECESSARIOS
- 1 a 2 checks criticos falharam mas sao corrigiveis sem refazer tudo
- Lista exatamente as telas e o que corrigir

### REPROVADO
- 3 ou mais checks criticos falharam
- Sequencia das etapas incorreta
- Tela 8, 13, 14 ou 18 ausentes ou fundamentalmente erradas
- Mais de 4 telas faltando

---

## Procedimento

1. Ler `output/quiz.json`
2. Ler `output/dados-quiz.json`
3. Executar os 11 checks em ordem, registrando cada resultado
4. Classificar: APROVADO / AJUSTES / REPROVADO
5. Retornar ao chat com resultado + lista de problemas detalhada (tela por tela)
6. Se AJUSTES: especificar exatamente quais telas corrigir e o que mudar
7. Se REPROVADO: explicar os problemas criticos e pedir para regerar
