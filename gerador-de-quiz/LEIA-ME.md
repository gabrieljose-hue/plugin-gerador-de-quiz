# Gerador de Quiz RTG

## O que e?

Este plugin cria automaticamente um **quiz de vendas completo** — no estilo Inlead — pronto para converter leads em compradores.

Voce responde algumas perguntas sobre o seu produto e o assistente gera todas as telas do quiz: abertura, perguntas de diagnostico, depoimentos, micro-compromisso e uma pagina de vendas interna completa.

**Sem precisar saber programacao ou design.**

---

## Como funciona?

```
Voce responde perguntas sobre o produto
            |
   Assistente gera todas as telas
            |
  Quiz pronto para implementar no Inlead
```

---

## Passo a passo

### 1. Abra o Claude Code com este plugin

Abra o Claude Code (claude.ai/code) e importe esta pasta como projeto.

### 2. Rode o comando

No chat, digite:

```
/criar-quiz
```

### 3. Responda as perguntas (3 rodadas)

**Rodada 1 — Produto:**
- Tema do quiz (ex: "Leitura rapida", "Emagrecimento", "Vendas online")
- Nome do produto que sera vendido
- Preco do produto
- Link do checkout

**Rodada 2 — Publico:**
- Quem e seu publico-alvo
- O quiz vai segmentar por genero?
- Quantos alunos/clientes voce ja tem
- Qual e a transformacao principal do produto

**Rodada 3 — Conteudo:**
- Qual e a dor principal do seu publico
- Quais sao as consequencias desse problema
- Quais sao os principais desejos do seu publico
- Depoimentos de clientes (se tiver)

### 4. Pronto!

O arquivo `output/quiz.md` contem seu quiz completo, pronto para implementar no Inlead.

---

## O que voce recebe

Um quiz com **15 telas** estruturadas:

| Tela | Tipo | O que faz |
|------|------|-----------|
| 1 | Apresentacao | Abertura + escolha de genero |
| 2A/2B | Escolha unica | Idade (com fotos por genero) |
| 3 | Apresentacao | Prova social (numero de alunos) |
| 4 | Escolha unica | Diagnostico 1 — nivel do problema |
| 5 | Captura | Nome e email do lead |
| 6 | Escolha unica | Diagnostico 2 — com nome personalizado |
| 7 | Escolha unica | Diagnostico 3 — dimensao adicional |
| 8 | Multipla escolha | Consequencias negativas do problema |
| 9 | Multipla escolha | Objetivo/desejo principal |
| 10A | Apresentacao | Depoimentos de clientes |
| 10B | Escolha unica | Nivel de conhecimento do lead |
| 11 | Apresentacao | Promessa personalizada |
| 12 | Escolha unica | Micro-compromisso |
| 13 | Carregamento | Analise de perfil (loading) |
| 14 | Apresentacao | Resultado — lead esta pronto |
| 15 | Apresentacao | Pagina de vendas interna completa |

---

## Como implementar no Inlead

1. Abra `output/quiz.md` no computador
2. Acesse sua conta no Inlead
3. Crie um novo quiz
4. Para cada tela documentada no arquivo:
   - Escolha o tipo de tela correto
   - Cole o titulo, subtitulo e opcoes
   - Configure a logica de navegacao descrita
5. Substitua os `[INSERIR X]` por suas imagens e links reais
6. Publique!

---

## Arquivos gerados

```
output/
├── dados-quiz.json   — dados coletados nas perguntas
├── quiz.json         — estrutura completa do quiz (para referencia)
└── quiz.md           — documento para implementar no Inlead (USE ESTE)
```

---

## Checklist antes de publicar

- [ ] Abriu o quiz.md e revisou todas as 15 telas?
- [ ] Substituiu todos os [INSERIR X]?
- [ ] Adicionou as imagens sugeridas em cada tela?
- [ ] Configurou a logica condicional de genero (tela 1 → 2A ou 2B)?
- [ ] Testou o quiz do inicio ao fim?
- [ ] O link de checkout esta funcionando?
- [ ] Testou no celular?

---

## Erros frequentes

### "O quiz nao tem personalizacao com o nome"
O nome do lead e capturado na Tela 5. A variavel `{{nome}}` so aparece a partir da Tela 6. Se o Inlead nao suportar esta variavel, substitua manualmente ou use o campo de personalizacao da plataforma.

### "As fotos das opcoes de idade nao aparecem"
As fotos sao sugeridas, mas voce precisa adicionar as imagens reais no Inlead. Cada opcao de idade (18-28, 29-38, etc.) deve ter uma foto correspondente.

### "Quero adicionar mais perguntas"
Voce pode adicionar perguntas extras entre as telas de diagnostico (4, 6, 7). Evite adicionar depois da Tela 12 (micro-compromisso) — isso pode reduzir a conversao.

---

## Suporte

Se tiver problemas, rode novamente:

```
/criar-quiz
```

O quiz anterior sera substituido pelo novo.
