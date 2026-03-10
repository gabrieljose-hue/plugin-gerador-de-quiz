---
name: extrator-copy
description: Extrai e estrutura o copy usando framework QFD. Retorna JSON entre tags <COPY_ESTRUTURADA>.
---

# Skill: Extrator de Copy

## Missão

Ler o copy de vendas bruto e extrair um JSON estruturado seguindo o **framework QFD** (Quadro/Furadeira/Decorado).

---

## Entrada (INPUT)

Você receberá:
- `COPY_BRUTO`: Texto completo do copy (pode estar incompleto)
- `PRECO_PRODUTO`: Preço respondido pelo mentorado (ex: "R$ 297")

Use o `PRECO_PRODUTO` se encontrar no copy, caso contrário use a resposta do mentorado.

---

## Saída (OUTPUT)

### Operação Interna
1. **Gere o JSON estruturado** seguindo a estrutura obrigatória abaixo
2. **Salve em arquivo**: `output/copy-estruturada.json`

### Retorno ao Chat
Retorne **APENAS** um resumo de 3-5 linhas:
```
✅ Copy estruturado salvo em output/copy-estruturada.json

📋 Resumo:
  • Produto: [NOME_PRODUTO]
  • Estrutura QFD: ✓ Quadro | ✓ Furadeira | ✓ Decorado
  • Campos ausentes: [lista de campos ou "nenhum"]
```

**NÃO emita o JSON completo no chat** — foi salvo em arquivo para evitar spam de tokens.

---

## Estrutura obrigatória do JSON

```json
{
  "produto": {
    "nome": "string",
    "tipo": "string (curso | comunidade | mentoria | acervo | outro)",
    "formato_entrega": "string (como é entregue: plataforma, lives, acesso imediato)",
    "preco": "string",
    "preco_antigo": "string (se houver preço riscado)",
    "garantia": "string",
    "link_checkout": "string"
  },
  "expert": {
    "nome": "string",
    "bio": "string",
    "credenciais": ["array de strings"],
    "resultados_alunos": "string",
    "historia_pessoal": "string (jornada de herói: vulnerabilidade + conquista)"
  },
  "copy_estruturada": {
    "premissa": "string (até 10 palavras — sacada criativa que gera curiosidade)",
    "headline": "string",
    "subheadline": "string",
    "bullets_cta": ["array de strings (o que vai aprender/ganhar assistindo ao vídeo)"],
    "texto_botao_cta": "string (verbo de ação + decorado)",
    "quadro": "string (resultado/transformação prometida)",
    "furadeira": "string (método/passo a passo)",
    "decorado": "string (benefícios emocionais após atingir o quadro)",
    "dores": ["array de strings (problemas reais que o público sente hoje)"],
    "desejos": ["array de strings (estados aspiracionais que o público quer alcançar)"],
    "demonstracao": "string (como o produto funciona na prática)",
    "argumento_incontestavel": "string (dado, pesquisa ou lógica objetiva)",
    "argumento_fonte": "string (fonte do dado: pesquisa X, estudo Y)"
  },
  "para_quem": {
    "e_para": ["array de strings (perfis/situações que se beneficiam)"],
    "nao_e_para": ["array de strings (perfis/situações que não se encaixam)"]
  },
  "comparacao": {
    "sem_produto": ["array de strings (o que acontece sem o produto)"],
    "com_produto": ["array de strings (o que acontece com o produto)"]
  },
  "objecoes": [
    {
      "objecao": "string",
      "resposta": "string"
    }
  ],
  "suporte": {
    "canais": ["array de strings (WhatsApp, email, comunidade)"],
    "descricao": "string"
  },
  "beneficios": [
    {
      "titulo": "string",
      "descricao": "string"
    }
  ],
  "depoimentos": [
    {
      "autor": "string",
      "texto": "string",
      "funcao": "string",
      "tipo": "texto|video",
      "url_video": "string"
    }
  ],
  "bonus": [
    {
      "titulo": "string",
      "descricao": "string",
      "preco_valor": "string"
    }
  ],
  "faq": [
    {
      "pergunta": "string",
      "resposta": "string"
    }
  ],
  "urgencia_escassez": {
    "tipo": "vagas|prazo|preco|outro",
    "texto": "string"
  },
  "campos_ausentes": ["array de nomes de campos"]
}
```

---

## Regra 1: Nunca inventar

Se uma informação **NÃO aparecer explicitamente no copy**:
- Deixe o campo vazio: `""`
- **Adicione ao array** `campos_ausentes[]`

**✅ CORRETO:**
```json
"expert": {
  "nome": "João Silva",
  "bio": "",
  "credenciais": [],
  "resultados_alunos": ""
},
"campos_ausentes": ["expert.bio", "expert.credenciais"]
```

**❌ ERRADO** (nunca faça):
```json
"expert": {
  "nome": "João Silva",
  "bio": "Especialista com muita experiência.",  // ← INVENTADO!
  "credenciais": ["Certificação"]  // ← INVENTADO!
}
```

---

## Regra 2: Framework QFD

Ao ler o copy, identifique as 3 camadas:

### QUADRO (Q) — O Resultado / Transformação
- O objetivo principal que o aluno vai alcançar com o produto
- É o "quadro na parede" — o estado desejado após aplicar o método
- Exemplo: "Ter uma renda extra de R$ 5.000/mês trabalhando de casa"

### FURADEIRA (F) — O Método / Processo
- O passo a passo que o aluno vai seguir para chegar ao quadro
- É o meio, não a prova — é como o produto entrega a transformação
- Exemplo: "Método em 3 fases: descobrir nicho → criar produto → vender todo dia"

### DECORADO (D) — Benefícios Emocionais pós-Quadro
- As consequências que acontecem APÓS o aluno atingir o quadro
- São mais emocionais e aspiracionais
- Exemplo: "Mais tempo com família, liberdade de horários, fim da insegurança financeira"

---

## Regra 3: Seções vazias

Se não houver benefícios/depoimentos/bonus/faq no copy:
- Deixe o array **vazio**: `[]`
- O agente construtor-html gerará placeholders automaticamente

---

## Exemplo completo

**COPY_BRUTO:**
```
Python para Análise de Dados

Aprenda Python em 30 dias e consiga seu emprego em Data Science.
Sem experiência anterior necessária. Método prático comprovado.

João Silva é programador com 10 anos de experiência.
Certificado Google Cloud.
500+ alunos formados, 85% conseguiram emprego em 6 meses.

Benefícios:
- Aulas práticas ao vivo
- Suporte em comunidade privada
- Certificado reconhecido

Depoimento: "Maria Santos, Analista de Dados na Google:
Entrei sem saber nada e consegui meu emprego em 4 meses!"

Preço: R$ 297 (antes R$ 497)
Garantia: 30 dias ou devolução 100%

10 vagas restantes. Preço sobe amanhã.
```

**JSON_ESTRUTURADO:**
```json
<COPY_ESTRUTURADA>
{
  "produto": {
    "nome": "Python para Análise de Dados",
    "tipo": "Curso online",
    "preco": "R$ 297",
    "garantia": "30 dias de garantia ou devolução 100%",
    "link_checkout": ""
  },
  "expert": {
    "nome": "João Silva",
    "bio": "Programador com 10 anos de experiência.",
    "credenciais": ["Certificado Google Cloud"],
    "resultados_alunos": "500+ alunos formados, 85% conseguiram emprego em 6 meses"
  },
  "copy_estruturada": {
    "headline": "Aprenda Python em 30 dias e consiga seu emprego em Data Science",
    "subheadline": "Sem experiência anterior necessária. Método prático comprovado.",
    "contexto_problema": "Mercado de Data Science cresce, mas faltam profissionais qualificados.",
    "quadro": "Você sente que o mercado cresce mas não consegue acompanhar.",
    "furadeira": "500+ alunos formados, 85% conseguiram emprego em 6 meses com nosso método.",
    "decorado": "10 vagas restantes. Preço sobe amanhã. Garanta agora por R$ 297."
  },
  "beneficios": [
    {
      "titulo": "Aulas práticas ao vivo",
      "descricao": "Aprenda com projetos reais que caem em entrevistas."
    },
    {
      "titulo": "Suporte em comunidade privada",
      "descricao": "Faça suas dúvidas e receba respostas em até 24 horas."
    },
    {
      "titulo": "Certificado reconhecido",
      "descricao": "Valide seu conhecimento com certificado emitido."
    }
  ],
  "depoimentos": [
    {
      "autor": "Maria Santos",
      "texto": "Entrei sem saber nada e consegui meu emprego em 4 meses!",
      "funcao": "Analista de Dados - Google",
      "tipo": "texto",
      "url_video": ""
    }
  ],
  "bonus": [],
  "faq": [],
  "urgencia_escassez": {
    "tipo": "vagas",
    "texto": "10 vagas restantes. Preço sobe amanhã."
  },
  "campos_ausentes": [
    "produto.link_checkout",
    "depoimentos (apenas 1 encontrado)",
    "bonus",
    "faq"
  ]
}
</COPY_ESTRUTURADA>
```

---

## Procedimento

1. **Leia o COPY_BRUTO** com atenção
2. **Identifique cada seção** (nome, preço, benefícios, etc)
3. **Preencha o JSON** seguindo a estrutura obrigatória
4. **Marque campos ausentes** em `campos_ausentes[]`
5. **Valide JSON** mentalmente (sem erros)
6. **Salve o JSON** em `output/copy-estruturada.json` (criar arquivo se não existir)
7. **Retorne ao chat** APENAS o resumo de 3-5 linhas (ver seção Saída acima)

---

## ⚠️ Restrições

- ❌ Nunca invente informações
- ❌ Nunca saia das tags `<COPY_ESTRUTURADA>`
- ❌ Nunca adicione comentários ou explicações fora do JSON
- ✅ Sempre use aspas duplas (JSON padrão)
- ✅ Sempre retorne todas as chaves, mesmo que vazias
- ✅ Valide sintaxe JSON
