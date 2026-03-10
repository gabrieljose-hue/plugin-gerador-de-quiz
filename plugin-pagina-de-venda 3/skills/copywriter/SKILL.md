---
name: copywriter
description: Guia o mentorado através da estrutura QFD para criar um copy profissional de vendas
---

# Skill: Copywriter

## Missão

Ajudar o mentorado a criar um **copy de vendas profissional e estruturado** seguindo o framework **QFD (Quadro/Furadeira/Decorado)**, que é a base esperada pelo plugin.

---

## Entrada (INPUT)

Nenhuma entrada prévia. A skill faz perguntas interativas.

---

## Saída (OUTPUT)

Um copy bem estruturado em português, pronto para ser passado ao skill `extrator-copy`.

**Formato**: Texto puro (markdown) com seções claramente identificadas.

---

## Conceitos-base do copy (obrigatório entender antes de gerar)

### PREMISSA
Sacada criativa de até 10 palavras que desperta curiosidade e faz a pessoa parar para ouvir. É diferente de headline, slogan ou promessa.

**Características obrigatórias**:
- Até 10 palavras
- Clara, sem enrolação
- Foco em um único benefício principal
- Memorável e impactante
- Gera curiosidade sem revelar tudo
- Criativa e distinta

**O que NÃO é premissa**:
- Não começa com verbo no imperativo
- Não tem interrogações ou exclamações
- Não usa "através de" ou "com" para sugerir o caminho
- Não usa conjunção "e" (foco em uma coisa só)
- Não é slogan motivacional nem explicação longa

**Exemplos de boas premissas**:
- "Tem gente cobrando R$800 por uma página feita com IA em 3 horas."
- "Guardar dinheiro é bom, mas fazer o dinheiro trabalhar por você é melhor."
- "Ansiedade não é normal."
- "Quem vende barato vende menos."
- "Você pode criar um produto digital lucrativo em poucas horas."

---

### ESTRUTURA QFD

### QUADRO (Q) — O Resultado / Transformação prometida
- O objetivo principal que o aluno vai alcançar COM o produto
- É o "quadro na parede" — o estado desejado após aplicar o método
- Exemplo: "Ter uma renda extra de R$ 5.000/mês trabalhando de casa"

### FURADEIRA (F) — O Método / Passo a passo
- O caminho que o aluno vai percorrer para chegar ao quadro
- É o meio, não a prova — é como o produto entrega a transformação
- Exemplo: "Método em 3 etapas: nicho → produto → venda diária"

### DECORADO (D) — Benefícios Emocionais pós-Quadro
- As consequências que acontecem APÓS o aluno atingir o quadro
- São mais emocionais e aspiracionais do que o quadro
- Exemplo: "Mais tempo com família, liberdade de horários, fim da insegurança financeira"

---

## Processo interativo

A skill faz perguntas organizadas em **5 blocos** para cobrir as 17 seções da página. Quando o mentorado responder um bloco, pergunte o próximo.

**OBRIGATÓRIO**: Todas as perguntas devem ser feitas usando a ferramenta `AskUserQuestion`. Como a ferramenta suporta no máximo 4 perguntas por chamada, o Bloco 1 (5 perguntas) deve ser dividido em 2 rodadas (Rodada 1.1: perguntas 1-4 | Rodada 1.2: pergunta 5). Os demais blocos (3 perguntas cada) cabem em uma única chamada.

Para cada pergunta, usar `multiSelect: false` e fornecer 2-3 opções de exemplo relevantes ao contexto. O mentorado pode selecionar um exemplo ou usar "Other" para responder livremente.

### BLOCO 1 — IDENTIDADE, PREMISSA E QFD

1. **Qual é o nome do seu produto?**
   - Exemplo: "Vendas Perpétuas Masterclass"

2. **Qual é a promessa principal do produto?** (QUADRO — o que ele faz?)
   - É a transformação/objetivo que o aluno atinge
   - Exemplo: "Ter uma renda digital de R$ 5.000/mês em vendas perpétuas"

3. **Qual é o método ou processo que ele usa para entregar isso?** (FURADEIRA — como ele faz?)
   - É o passo a passo, o caminho que o aluno percorre
   - Exemplo: "Método em 3 fases: posicionar, criar, perpetuar"

4. **Quais são os benefícios concretos e práticos que o produto causa?** (DECORADO — o que a pessoa sente, vive ou conquista?)
   - As consequências emocionais e aspiracionais APÓS atingir o quadro
   - Exemplo: "Mais tempo com família, liberdade de horário, fim da ansiedade financeira"

5. **Para quem é esse produto?** (Público frio, morno ou quente — iniciante, intermediário, avançado)
   - Descreva o perfil ideal: situação atual, nível de conhecimento, dor que sente
   - Exemplo: "Infoprodutores iniciantes que já têm produto mas vendem só em lançamento (público morno)"

**Síntese**: Gera a **Premissa** (até 10 palavras) baseada nas respostas + blocos de Quadro, Furadeira, Decorado e Segmentação ("Para quem é" / "Para quem NÃO é").

**Sobre a Premissa**: Após gerar, mostrar ao mentorado para validar antes de seguir.

**Extras capturados neste bloco**:
- Identidade do Comunicador (inferida da resposta Q5 + contexto)
- Identidade do Consumidor (inferida do QUADRO + DECORADO)
- Sacada criativa (inferida do diferencial implícito nas respostas)

---

### BLOCO 2 — FORMATO, OFERTA E GARANTIA

6. **Como funciona o produto?** (Formato: curso, mentoria, grupo, etc. O que o comprador recebe? Como é entregue?)
   - Separar: tipo do produto + plataforma + acesso + duração + módulos + frequência
   - Exemplo: "Curso online, 8 módulos gravados, acesso imediato na Hotmart, suporte por grupo fechado no Telegram"

7. **Qual é a sua oferta completa?** (O que está incluso, preço original, valor promocional, bônus etc.)
   - Liste TUDO: módulos, bônus, materiais extras, acesso a comunidade
   - Com valor percebido de cada item se possível
   - Exemplo: "Curso principal (R$ 997) + Bônus: Templates prontos (R$ 197) + Comunidade VIP (R$ 497) + Mentoria em grupo 1x/mês (R$ 997) = R$ 2.688, por R$ 497"

8. **Qual é a garantia oferecida?** (Número de dias e se é condicional ou incondicional)
   - Exemplo: "7 dias de garantia incondicional — devolvemos 100%, sem perguntas"

**Síntese**: Gera blocos de Formato (seção 6), Oferta + Preço (seção 7) e Garantia (parte da seção 7).

---

### BLOCO 3 — AUTORIDADE, PROVAS E OBJEÇÕES

9. **Você tem depoimentos de clientes?** (Inclua se tiver)
   - Se sim: nome, contexto/profissão e resultado específico (2-3 depoimentos)
   - Se não: sem problema, usamos credenciais e argumentos
   - Exemplo: "Marina, designer — 'Em 30 dias consegui minhas primeiras 3 vendas perpétuas'"

10. **Qual é a sua autoridade?** (Histórico, conquistas, por que você pode ensinar isso)
    - Credenciais, anos de experiência, número de alunos, resultados
    - Exemplo: "10 anos no mercado digital, 2.000 alunos, R$ 3M faturados, palestrante no Fire Festival"

11. **Quais são as principais objeções que os clientes costumam ter?**
    - As dúvidas que BLOQUEIAM a compra (não FAQ técnico)
    - Exemplo: "Não tenho tempo / Já tentei antes e não funcionou / Será que funciona pro meu nicho?"

**Síntese**: Gera blocos de Depoimentos (seção 8), Autoridade (seção 10) e Objeções Respondidas (seção 11).

---

### BLOCO 4 — FAQ, SUPORTE E ARGUMENTO RACIONAL

12. **Quais perguntas frequentes você recebe sobre o produto?**
    - Dúvidas operacionais, técnicas, de acesso (diferentes das objeções)
    - Exemplo: "Como acesso o curso? / Posso parcelar? / Tem suporte? / Funciona no celular? / Por quanto tempo tenho acesso?"
    - Se não tiver: informar que geramos 5 FAQs padrão

13. **Qual é o canal de suporte ao cliente?** (WhatsApp, e-mail, comunidade etc.)
    - Incluir: canais disponíveis, tempo de resposta, horário de atendimento
    - Exemplo: "WhatsApp (resposta em até 2h, seg-sex 9h-18h) + e-mail suporte@exemplo.com + grupo VIP no Telegram"

14. **Tem algum argumento racional forte que sustenta o produto?** (Dados, pesquisas, lógica, notícias, fatos)
    - Número, pesquisa ou dado objetivo que desarma ceticismo
    - Exemplo: "92% dos alunos relatam aumento de vendas nos primeiros 30 dias (pesquisa interna com 500 alunos)"

**Síntese**: Gera blocos de FAQ (seção 12), Suporte (seção 13) e Argumento Incontestável (seção 14).

---

### BLOCO 5 — EMOÇÃO, HISTÓRIA E COMPARAÇÃO

15. **Quais são as principais dores e desejos do seu público?**
    - **Dores** (hoje): o que a pessoa sente/vive e quer sair disso
    - **Desejos** (depois): o estado que quer alcançar
    - Exemplo: Dores: "Insegurança financeira, depende de lançamentos, trabalha 12h/dia" / Desejos: "Renda previsível, mais tempo livre, negócio que funciona sem ele"

16. **Fale brevemente sobre você, o autor.** (História, propósito do produto, diferencial)
    - Mini jornada de herói: ponto de virada + vulnerabilidade + conquista
    - Por que criou este produto
    - Exemplo: "Fui demitido em 2018, passei 6 meses sem renda, aprendi sozinho a vender online e hoje fatura 6 dígitos. Criei este curso porque ninguém me ensinou isso quando precisei."

17. **Seu produto pode ser comparado com alguma outra solução?** (Concorrentes, métodos tradicionais etc.)
    - O que diferencia dos outros — sem denegrir
    - Exemplo: "Cursos tradicionais ensinam teoria de lançamento. O meu ensina a vender todo dia, na prática, sem depender de lives."

**Síntese**: Gera blocos de Dores & Desejos (seção 15), Sobre o Autor (seção 16) e Comparação (seção 17).

---

## Estrutura final do copy entregue (17 seções em 4 blocos)

O copy gerado deve ter **esta ordem exata**, cobrindo as 17 seções da página:

```
PREMISSA
[Até 10 palavras — sacada criativa que gera curiosidade]

--- BLOCO 1: HERO / PERSUASÃO ---

[HEADLINE PRINCIPAL]
[SUBHEADLINE]

ASSISTA AO VÍDEO E DESCUBRA:
- [Bullet 1: resultado prático sem entregar tudo]
- [Bullet 2: curiosidade prática]
- [Bullet 3: benefício real]

BOTÃO CTA: [Verbo de ação + decorado]

DEMONSTRAÇÃO — Como funciona na prática:
[Explicação prática do método / passo a passo visual]

PARA QUEM É ESTE PRODUTO:
- [Perfil/situação 1 que se identifica]
- [Perfil/situação 2]
- [Perfil/situação 3]

PARA QUEM NÃO É:
- [Situação que não se encaixa 1]
- [Situação 2]

--- BLOCO 2: OFERTA ---

COMO FUNCIONA / FORMATO:
[Tipo do produto, plataforma, como é entregue, acesso]

O QUE VOCÊ RECEBE:
- [Entregável 1 + valor percebido]
- [Entregável 2 + valor percebido]
- [Bônus + valor]
Total: R$ [valor acumulado] — Hoje por R$ [preço]

GARANTIA:
[Texto direto e humano, sem linguagem jurídica]

DEPOIMENTOS:
[Nome] — [Função]: "[Resultado específico com o produto]"
[Nome] — [Função]: "[Resultado específico]"

PALIATIVO (ENTREGÁVEL RÁPIDO):
[O que é + por que é útil imediatamente]

--- BLOCO 3: CREDIBILIDADE ---

AUTORIDADE DO EXPERT:
[Credenciais + resultados de alunos + por que confiar]

OBJEÇÕES RESPONDIDAS:
Objeção: [Dúvida que bloqueia a compra]
Resposta: [Resposta empática e direta]

PERGUNTAS FREQUENTES:
P: [Pergunta real que o mentorado recebe — operacional/técnica]
R: [Resposta direta e clara]
P: [Pergunta 2]
R: [Resposta 2]
(mínimo 5 perguntas — usar as reais do mentorado + completar com padrão se necessário)

SUPORTE:
Canais: [WhatsApp / e-mail / comunidade — dados reais do mentorado]
Tempo de resposta: [ex: até 2h em dias úteis]
Horário: [ex: seg-sex, 9h-18h]
"Você não estará sozinho(a). Nossa equipe está aqui para ajudar."

--- BLOCO 4: EMOÇÃO / LÓGICA ---

ARGUMENTO INCONTESTÁVEL:
[Dado, pesquisa ou lógica objetiva com número em destaque]
Fonte: [pesquisa X / dado de Y]

DORES (HOJE):
- [Dor real 1 que o público sente]
- [Dor real 2]

DESEJOS (DEPOIS):
- [Estado aspiracional 1]
- [Estado aspiracional 2]

SOBRE O AUTOR:
[Mini jornada: ponto de virada → vulnerabilidade → conquista]
[Por que criou este produto]

COMPARAÇÃO COM OUTRAS SOLUÇÕES:
Sem [produto]: [consequência negativa]
Com [produto]: [consequência positiva]
```

---

## Procedimento da skill

1. **Bloco 1 — Rodada 1.1** (usar `AskUserQuestion` com 4 perguntas):
   - Pergunta 1: Nome do produto (header: "Produto")
   - Pergunta 2: Promessa principal / QUADRO (header: "Quadro")
   - Pergunta 3: Método / FURADEIRA (header: "Furadeira")
   - Pergunta 4: Benefícios emocionais / DECORADO (header: "Decorado")
   - Aguarda respostas

2. **Bloco 1 — Rodada 1.2** (usar `AskUserQuestion` com 1 pergunta):
   - Pergunta 5: Para quem é o produto (header: "Público")
   - Aguarda resposta
   - Gera a Premissa (até 10 palavras) — mostrar ao mentorado
   - Usar `AskUserQuestion` com 1 pergunta para validar:
     - Pergunta: "Essa é a premissa gerada: '[PREMISSA]'. O que acha?" (header: "Premissa")
     - Opções:
       - "Aprovada, seguir em frente" (description: "A premissa está boa, continuar para o próximo bloco")
       - "Quero ajustar" (description: "Vou sugerir mudanças na premissa")
   - Se aprovar: seguir para Bloco 2
   - Se ajustar: refazer premissa e perguntar novamente

3. **Bloco 2** (usar `AskUserQuestion` com 3 perguntas):
   - Pergunta 6: Formato do produto (header: "Formato")
   - Pergunta 7: Oferta completa (header: "Oferta")
   - Pergunta 8: Garantia (header: "Garantia")
   - Aguarda respostas → sintetiza internamente blocos de Formato, Oferta e Garantia (sem emitir ao chat — seguir direto para próximo bloco)

4. **Bloco 3** (usar `AskUserQuestion` com 3 perguntas):
   - Pergunta 9: Depoimentos (header: "Provas")
   - Pergunta 10: Autoridade (header: "Autoridade")
   - Pergunta 11: Objeções (header: "Objeções")
   - Aguarda respostas → sintetiza internamente blocos de Depoimentos, Autoridade e Objeções (sem emitir ao chat — seguir direto para próximo bloco)

5. **Bloco 4** (usar `AskUserQuestion` com 3 perguntas):
   - Pergunta 12: FAQ (header: "FAQ")
   - Pergunta 13: Suporte (header: "Suporte")
   - Pergunta 14: Argumento racional (header: "Argumento")
   - Aguarda respostas → sintetiza internamente blocos de FAQ, Suporte e Argumento Incontestável (sem emitir ao chat — seguir direto para próximo bloco)

6. **Bloco 5** (usar `AskUserQuestion` com 3 perguntas):
   - Pergunta 15: Dores e desejos (header: "Dores")
   - Pergunta 16: Sobre o autor (header: "Autor")
   - Pergunta 17: Comparação (header: "Comparação")
   - Aguarda respostas → sintetiza internamente blocos de Dores/Desejos, Sobre o Autor e Comparação (sem emitir ao chat — seguir direto para montagem)

7. **Monta o copy completo**
   - Combina todos os 5 blocos nas 17 seções
   - Formata como markdown em ordem de blocos
   - Sem emojis típicos de IA (sem 🎯 🚀 ⚡ 📈 🎥)
   - Inferir Identidade do Comunicador e Consumidor das respostas (para uso posterior pelo designer)

8. **Pergunta ao mentorado** (usar `AskUserQuestion` com 1 pergunta):
   - Pergunta: "Seu copy está pronto! Quer ajustar alguma seção?" (header: "Revisão")
   - Opções: "Está ótimo, seguir em frente" | "Quero ajustar uma seção"
   - Se ajustar: refaz a seção específica
   - Se aprovado: retorna copy final para o fluxo

---

## Tom e estilo

- **Claro e direto**: Sem jargão, linguagem simples
- **Específico**: Números, nomes, datas (não genérico)
- **Emocional**: Conecta com sentimentos do cliente
- **Persuasivo**: Usa argumentos psicológicos (urgência, FOMO, autoridade)
- **Confiável**: Baseado em evidências reais

---

## Validações

- ✅ Premissa gerada (até 10 palavras, criativa, gera curiosidade)
- ✅ Todas as 3 partes do QFD presentes com conceitos corretos
- ✅ Tem depoimentos com nomes e resultados específicos
- ✅ Tem argumento incontestável (dado ou lógica objetiva)
- ✅ Tem dores (hoje) e desejos (depois) distintos
- ✅ Tem demonstração prática
- ✅ Tem "para quem é" e "para quem não é"
- ✅ Tem objeções respondidas
- ✅ Tem comparação com outras soluções
- ✅ Tem história pessoal do autor (jornada de herói)
- ✅ Linguagem natural em português, sem jargão
- ✅ Sem emojis típicos de IA (🎯 🚀 ⚡ 📈 🎥)
- ✅ Sem mentiras ou promessas ilegais

---

## Exemplo de copy gerado (resumido)

```
PYTHON PARA ANÁLISE DE DADOS
Domine análise de dados em 90 dias. Ganhe R$ 5.000/mês.

Você está cansado de trabalhar 12 horas por dia em tarefas manuais.
Seu chefe não reconhece seu trabalho. Você quer crescer, mas não sabe por onde começar.
A maioria das soluções no mercado é cara e desconectada da realidade.

Mas e se você pudesse dominar análise de dados de verdade?
João ganhou R$ 5.000/mês em 90 dias.
Marina reduziu horas de trabalho em 40%.
Certificado Google Cloud. 10 anos de experiência em Fortune 500.
500+ alunos satisfeitos.

Apenas 10 vagas restantes. Preço sobe dia 31 de março.
Esta é sua chance de mudar de vida.

O QUE VOCÊ VAI APRENDER:
- Python do zero
- Análise com Pandas e NumPy
- Visualização com Matplotlib
- Dashboard interativo

SOBRE MIM:
Sou João Silva. Certificado Google Cloud. Trabalhei 10 anos na Netflix analisando dados.
Criei este método para ensinar análise DE VERDADE, não teoria.

DEPOIMENTOS:
"Achei que Python era impossível. Em 30 dias consegui criar meu primeiro dashboard!" - Marina
"Meu chefe pediu aumento depois que viu minha análise." - Carlos
"Agora ganho R$ 5.000 consultando dados." - Ana

BÔNUS:
- Template Excel de análise
- Script Python pronto
- Acesso ao grupo privado
Valor: R$ 500 (GRÁTIS para quem comprar hoje)

PREÇO:
Investimento: R$ 297
Garantia: 30 dias de reembolso integral (sem pergunta)

FAQ:
P: Preciso de experiência?
R: Não! Do zero é possível.

...

Não perca essa chance.
10 vagas apenas. Preço sobe amanhã.
Clique em Comprar agora e mude sua vida.
```

---

## Restrições

- ❌ Não inventar resultados falsos
- ❌ Não fazer promessas ilegais ("ganhe R$ 10.000 garantido")
- ❌ Não copiar de outros sites
- ✅ Usar dados reais do mentorado
- ✅ Ser honesto em números
- ✅ Enfatizar valor real do produto
