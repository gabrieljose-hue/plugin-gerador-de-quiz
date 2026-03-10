---
description: Gera um quiz de vendas completo com 18 telas estruturadas para converter leads em compradores
---

# Comando /criar-quiz

Orquestra o fluxo completo: coleta ou extrai dados → revisa e ajusta → gera as 18 telas → revisa qualidade.

## REGRA DE FLUXO CONTINUO

Cada passo deve fluir automaticamente para o proximo sem anunciar, pedir confirmacao ou esperar o usuario dizer "continue". So pausar quando houver uma decisao real do usuario (AskUserQuestion). Nunca emitir mensagens como "Pronto para...", "Agora vamos...", "Dados salvos!", "Otimo!".

---

## PASSO 0: Decisao Inicial

**Objetivo**: Perguntar se o usuario ja tem copy/briefing do produto ou precisa criar do zero.

**Usar `AskUserQuestion`** com 1 pergunta:
- Pergunta: "Voce ja tem um copy ou briefing do seu produto?" (header: "Copy do Produto")
- Opcoes:
  - "Sim, tenho copy/briefing pronto" (description: "Vou colar meu texto e o assistente extrai os dados automaticamente")
  - "Nao, vou criar agora" (description: "O assistente vai me guiar com perguntas para coletar os dados do zero")

**Bifurcacao**:

### Opcao A: Ja tem copy/briefing
- Define `DADOS_ORIGEM = 'copy'`
- Pedir ao usuario que cole o texto: "Cole aqui o seu copy ou briefing completo:"
- Aguardar texto colado
- Chamar skill **extrator-dados** com o texto recebido
- Skill salva `output/dados-quiz.json` a partir do copy
- Ir para **PASSO 0.5**

### Opcao B: Vai criar do zero
- Define `DADOS_ORIGEM = 'formulario'`
- Chamar skill **coletor-dados** para coletar os dados via perguntas interativas
- Skill salva `output/dados-quiz.json` a partir das respostas
- Ir para **PASSO 0.5**

---

## PASSO 0.5: Revisar e Validar os Dados

**Objetivo**: Garantir que os dados de qualquer origem estao completos e adequados antes de gerar o quiz.

Chamar skill **revisor-dados** para validar e ajustar `output/dados-quiz.json`.

**O revisor**:
- Verifica se todos os campos obrigatorios estao presentes
- Ajusta proativamente o que puder (sem perguntar): completa consequencias, desejos, objetivos especificos, infere amplificar_oportunidade
- Perguntas ao usuario APENAS para campos criticos ausentes que nao podem ser inferidos (maximo 3 perguntas, 1 rodada)
- Aprova o JSON quando todos os campos obrigatorios estiverem OK

**Saida**: `output/dados-quiz.json` validado e completo

---

## PASSO 1: Gerar o Quiz Completo

Chamar skill **criador-quiz** para gerar todas as 18 telas.

**Entrada**: `output/dados-quiz.json`

**18 telas geradas em ordem**:
```
Tela 1:  Abertura (apresentacao — escolha de genero)
Tela 2A: Dados Demograficos — Masculino (escolha-unica — idade com fotos masculinas)
Tela 2B: Dados Demograficos — Feminino (escolha-unica — idade com fotos femininas)
Tela 3:  Prova Social (apresentacao — numeros de alunos)
Tela 4:  Diagnostico 1 — Pergunta comportamental (escolha-unica)
Tela 5:  Captura — Nome e Email (captura)
Tela 6:  Diagnostico 2 — Pergunta com {{nome}} (escolha-unica)
Tela 7:  Diagnostico 3 — Terceira dimensao do problema (escolha-unica)
Tela 8:  Tocar na Ferida 1 — "Sente-se culpado?" (escolha-unica — SO opcoes afirmativas)
Tela 9:  Tocar na Ferida 2 — Consequencias negativas (multipla-escolha)
Tela 10: Desejo Principal — Objetivo do lead (multipla-escolha)
Tela 11: Depoimentos — Prova de solucao (apresentacao)
Tela 12: Nivel de Conhecimento — "{{nome}}, o que voce sabe sobre [tema]?" (escolha-unica)
Tela 13: Objetivo Especifico — 1 resultado do produto (escolha-unica — resposta usada na Tela 14)
Tela 14: Promessa Personalizada — "{{nome}}, nos iremos te ajudar!" com {{resposta_tela_13}} (apresentacao)
Tela 15: Micro-Compromisso — Motivacao (escolha-unica — so opcoes positivas)
Tela 16: Carregamento — Analise de perfil (carregamento)
Tela 17: Resultado — Pronto para resultados + grafico (apresentacao)
Tela 18: Pagina de Vendas Interna — Oferta completa (apresentacao longa)
```

**Saidas**:
- `output/quiz.json` — estrutura JSON completa
- `output/quiz.md` — documento legivel para implementacao no Inlead

---

## PASSO 1.5: Designer de Quiz

Chamar skill **designer-quiz** para criar a identidade visual e sugerir imagens para cada tela.

**Entrada**: `output/dados-quiz.json` + `output/quiz.json`

**O designer faz**:
- Pergunta 1: Cor principal do produto/marca (ou define automaticamente pelo tema)
- Pergunta 2: Estilo visual (Moderno e Limpo / Bold e Impactante / Suave e Acolhedor / Profissional e Serio / Energico e Jovem)
- Gera paleta de cores completa (primaria, secundaria, acento, fundo, texto)
- Escolhe tipografia do Google Fonts adequada ao estilo
- Define estilo e cor dos botoes
- Para cada tela com imagem: sugere termo de busca + link direto no Unsplash e Pexels

**Saidas**:
- `output/design-tokens.json` — paleta, tipografia, botoes
- `output/quiz-design.md` — guia visual completo + links de imagens por tela + checklist de implementacao

---

## PASSO 1.75: Revisar o Design

Chamar skill **revisor-design** para validar o kit visual gerado pelo designer-quiz.

**Entrada**: `output/design-tokens.json` + `output/design-preview.html` + `output/dados-quiz.json`

**O revisor valida (10 pontos)**:
- Coerencia da paleta com o nicho e publico
- Cor primaria vibrante e distinta (nao cinza, nao branco)
- Cards de opcao com estados selecionado/nao-selecionado claramente distintos
- Botao CTA com alto contraste
- Barra de progresso presente em 3 estados
- Formulario de captura visivel
- Loading screen com checklist animado
- Tipografia legivel e proporcional
- 10 secoes presentes no preview HTML
- Design com identidade propria (nao parece template generico de IA)

**Resultado**:
- **APROVADO**: Seguir para PASSO 2
- **AJUSTES**: designer-quiz corrige os pontos indicados → revisor-design valida novamente
- **REPROVADO**: designer-quiz refaz o design system do zero → revisor-design valida novamente (maximo 2 tentativas)

---

## PASSO 2: Revisar o Quiz

Chamar skill **revisor-quiz** para validar qualidade e conversao.

**Entrada**: `output/quiz.json` + `output/dados-quiz.json`

**O revisor valida**:
- 18 telas presentes e em ordem correta
- Tela 8 com apenas opcoes afirmativas
- Tela 13 com 4 objetivos especificos
- Tela 14 referencia `{{resposta_tela_13}}` dinamicamente
- `{{nome}}` apenas a partir da Tela 6
- Tela 9 com minimo de 5 consequencias em primeira pessoa
- Tela 15 com apenas opcoes positivas (micro-compromisso)
- Tela 18 com os 8 blocos da pagina de vendas
- Fluxo psicologico e sequencia corretos
- Tom progressivo

**Resultado**:
- **APROVADO**: Seguir para PASSO 3
- **AJUSTES**: criador-quiz corrige as telas indicadas → revisor-quiz valida novamente
- **REPROVADO**: criador-quiz refaz tudo → revisor-quiz valida novamente (maximo 2 tentativas)

---

## PASSO 3: Confirmacao

Exibir resultado final:

```
Quiz gerado com sucesso!

Arquivos salvos:
  • output/dados-quiz.json    — dados do produto e publico
  • output/quiz.json          — estrutura completa do quiz
  • output/quiz.md            — conteudo de cada tela (para implementar no Inlead)
  • output/design-tokens.json — paleta de cores, fontes e estilo dos botoes
  • output/quiz-design.md     — guia visual + imagens sugeridas por tela + checklist

Resumo:
  • Tema: [TEMA]
  • Produto: [PRODUTO]
  • Total de telas: [N] (varia conforme genero/depoimentos)
  • Personalizacao {{nome}}: Tela 6, 12, 14, 18
  • Resposta dinamica {{resposta_tela_13}}: Tela 14
  • Segmentacao por genero: [Sim/Nao — auto-detectado]
  • Pergunta de idade: [Sim/Nao — auto-detectado]
  • Telas de depoimentos: [0 / 1 / 2]
  • Cor principal: [HEX]
  • Estilo visual: [estilo escolhido]

Como implementar no Inlead:
  1. Abra output/quiz.md — crie cada tela na ordem indicada
  2. Abra output/quiz-design.md — configure cores, fontes e adicione as imagens
  3. Configure a logica condicional de genero (se houver): Tela 1 → 2A ou 2B
  4. Configure a variavel {{resposta_tela_13}} na Tela 14
  5. Teste do inicio ao fim antes de publicar
```

---

## Tratamento de Erros

### Se o usuario nao colar copy na Opcao A
```
Para extrair os dados, preciso do texto do seu copy ou briefing.
Cole aqui qualquer texto que descreva o produto: landing page, roteiro de vendas, descricao, briefing, etc.
```

### Se revisor-dados reprovar 2 vezes
```
Nao foi possivel validar os dados automaticamente. Vamos reiniciar a coleta.
```
Reiniciar pelo PASSO 0.

### Se revisor-quiz reprovar 2 vezes
```
O quiz nao passou na revisao apos 2 tentativas.
Verifique os dados em output/dados-quiz.json e rode /criar-quiz novamente.
```

---

## Tabela de Arquivos

| Arquivo | Escrito por | Lido por |
|---------|-------------|----------|
| `output/dados-quiz.json` | Passo 0 + Passo 0.5 | Passos 1, 1.5, 1.75, 2 |
| `output/quiz.json` | Passo 1 (criador-quiz) | Passos 1.5, 2 |
| `output/quiz.md` | Passo 1 (criador-quiz) | Final (usuario) |
| `output/design-tokens.json` | Passo 1.5 (designer-quiz) | Passo 1.75 (revisor-design) + Final (usuario) |
| `output/design-preview.html` | Passo 1.5 (designer-quiz) | Passo 1.75 (revisor-design) |
| `output/quiz-design.md` | Passo 1.5 (designer-quiz) | Final (usuario) |

---

## Skills Utilizadas (7 no total)

- **extrator-dados**: Extrai dados de copy existente (Passo 0 — Opcao A)
- **coletor-dados**: Coleta dados via perguntas interativas (Passo 0 — Opcao B)
- **revisor-dados**: Valida e ajusta os dados de qualquer origem (Passo 0.5)
- **criador-quiz**: Gera as telas do quiz (Passo 1)
- **designer-quiz**: Cria identidade visual + sugere imagens com links Unsplash/Pexels (Passo 1.5)
- **revisor-design**: Valida o kit visual gerado pelo designer-quiz (Passo 1.75)
- **revisor-quiz**: Valida qualidade e conversao do quiz (Passo 2)
