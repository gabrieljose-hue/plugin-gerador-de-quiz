# 📄 Plugin Página de Venda RTG

## O que é?

Este é um **plugin automático** que transforma seu copy de vendas em uma **página HTML profissional e pronta para publicar**.

Tudo que você precisa fazer:
1. Escrever seu copy em um arquivo Word (.docx)
2. Rodar um comando no Claude Code
3. Responder 5 perguntas simples
4. Pronto! Sua página está gerada.

**Sem precisar saber programação.**

---

## Como funciona?

```
Seu copy (Word)
        ↓
  5 perguntas (cor, link, etc)
        ↓
  Agente IA extrai estrutura
        ↓
  Agente IA gera HTML
        ↓
  Página pronta para publicar 🎉
```

---

## Passo a passo

### 1️⃣ Abra Claude Cowork

Abra o aplicativo **Claude Cowork** (ferramenta de IA da Anthropic).

Quando solicitado, importe a pasta `plugin-pagina-de-venda/`.

### 2️⃣ Rode o comando `/criar-pagina`

No chat do Claude Cowork, digite:

```
/criar-pagina
```

### 3️⃣ Escolha: Tem copy pronto ou quer gerar?

O sistema vai fazer essa pergunta inicial:

```
Você já tem um copy pronto para sua página de vendas?
1️⃣ Sim, tenho um copy pronto
2️⃣ Não, quero gerar um copy agora
```

---

## Opção A: Você já tem copy pronto

Se escolher **Opção 1: Sim, tenho um copy pronto**

1. Cole o texto completo do seu copy no chat
2. Vá para seção **Opção Comum: Responda as 8 perguntas** (abaixo)

**Seu copy deve incluir:**
- **Headline** (promessa principal)
- **Subheadline** (detalhe)
- **Problemas** (o que o cliente sofre)
- **Benefícios** (por que seu produto é bom)
- **Depoimentos** (prova social)
- **Preço** (valor)
- **Garantia** (segurança)
- **Urgência** (vagas, prazo, etc)

👉 **Dica**: Quanto mais detalhes você colocar, melhor o HTML será.

---

## Opção B: Você quer gerar um copy agora

Se escolher **Opção 2: Não, quero gerar um copy agora**

O sistema vai chamar o **Copywriter AI** que fará 3 rodadas de perguntas:

### Rodada 1: O PROBLEMA (Quadro)
- Qual é seu produto/curso/serviço?
- Qual é seu público-alvo?
- Qual é o problema que eles sofrem?
- Por que agora é importante resolver?
- Como seu cliente se sente com isso?

### Rodada 2: PROVA E AUTORIDADE (Furadeira)
- Você tem depoimentos de alunos/clientes?
- Quais são suas credenciais?
- Qual é o resultado que seus clientes conquistaram?
- O que torna você diferente?

### Rodada 3: URGÊNCIA E EMOÇÃO (Decorado)
- Qual é a urgência/escassez?
- Qual é o benefício transformador?
- O que o cliente vai conseguir fazer depois?

**Depois**: O Copywriter gera seu copy automaticamente. Se gostar, ele aprova e segue. Se quiser ajustes, você pede mudanças.

---

## Opção Comum: Responda as 8 perguntas

**Pergunta 1: Cor principal**
- Qual é a cor do seu brand? (ex: `#FF6B00`)
- Vai aparecer nos botões de compra
- Se não sabe, use: `#FF6B00`

**Pergunta 2: Nome do produto**
- Como se chama seu produto?
- Ex: `Curso Python Avançado`
- Aparece no título da aba do navegador

**Pergunta 3: Preço do produto**
- Qual é o preço final?
- Ex: `R$ 297` ou `R$ 1.997`
- Se não definiu ainda, deixe em branco

**Pergunta 4: Link de checkout**
- Qual é o link da página de compra?
- Ex: `https://checkout.sua-empresa.com/produto123`
- Se não tem ainda, escreva: `será definido depois`

**Pergunta 5: Urgência/Escassez**
- Qual é a urgência para o cliente?
- Ex: `10 vagas restantes` ou `Prazo até 31/03` ou `Preço sobe amanhã`
- Se não tem, deixe em branco

**Pergunta 6: Tipo de depoimentos**
- Seus depoimentos são em **texto** ou **vídeo**?
- Escolha: `texto` ou `video`
- Se não sabe, escolha: `texto`

**Pergunta 7: URL da logo** (opcional)
- Qual é a URL da sua logo? (ex: `https://cdn.seu-cloudron.com/logo.png`)
- Pode ser vazia — a navbar vai mostrar apenas o nome do produto
- Se deixar em branco, nenhuma logo aparece

**Pergunta 8: URL da foto do expert** (opcional)
- Qual é a URL da sua foto? (ex: `https://cdn.seu-cloudron.com/expert.jpg`)
- Pode ser vazia — vai aparecer um placeholder cinza
- Se deixar em branco, aparece um avatar genérico

### 5️⃣ Pronto! ✅

O arquivo `pagina-de-venda.html` foi criado na pasta `output/`.

```
plugin-pagina-de-venda/
└── output/
    └── pagina-de-venda.html  ← Sua página completa!
```

A página já inclui:
- ✅ Sua logo (se você passou a URL na pergunta 7)
- ✅ Sua foto (se você passou a URL na pergunta 8)
- ✅ Seu preço (respondido na pergunta 3)
- ✅ Seu link de checkout (respondido na pergunta 4)
- ✅ Sua cor principal (respondido na pergunta 1)

---

## O que falta? (Campos ausentes)

Após gerar, o sistema lista os campos que **não foram encontrados** no seu copy.

Exemplo:

```
⚠️  Campos ausentes:
  • expert.credenciais
  • bonus
  • faq
```

Isso significa que seu copy não tinha informação sobre credenciais, bônus ou perguntas frequentes.

### Como completar?

1. Abra o arquivo `output/pagina-de-venda.html` em um editor de texto
2. Procure por `[INSERIR NOME]` ou similares
3. Substitua com a informação correta
4. Salve o arquivo

---

## 8 Seções da sua página

A página gerada tem **8 seções profissionais**:

### 1. Hero
- Seu headline em grande
- Botão de compra destacado
- Texto de urgência (vagas limitadas, etc)

### 2. Benefícios
- 3-5 pontos principais do seu produto
- Cada um com ícone ✓

### 3. Quem é você?
- Sua foto/bio
- Suas credenciais
- Resultados de alunos/clientes

### 4. Depoimentos
- Provas sociais de quem já comprou
- Em texto ou vídeo (conforme respondeu)

### 5. Bônus
- Extras que vêm com a compra
- Mostra valor (de R$ X - hoje incluído)

### 6. Preço + Garantia
- Preço final destacado
- Garantia (segurança de compra)

### 7. FAQ
- Perguntas frequentes
- Clique para expandir/recolher

### 8. CTA Final
- Último apelo emocional
- Botão de compra final

---

## Abrindo o HTML

### No navegador

1. Procure o arquivo `output/pagina-de-venda.html` no seu computador
2. Clique 2x para abrir (abre automaticamente no navegador)
3. Veja como ficou!

### Para publicar

Você pode publicar de várias formas:

**Opção 1: Usar um hosting próprio**
- Suba o arquivo .html via FTP/SFTP
- Coloque na raiz do seu domínio

**Opção 2: Usar WordPress**
- Crie uma página em branco
- Cole o código HTML (ativar modo texto)
- Publique

**Opção 3: Usar Google Sites**
- Embed/incorpore o arquivo em uma página

**Opção 4: GitHub Pages (gratuito)**
- Crie um repositório no GitHub
- Suba o arquivo .html
- Ative GitHub Pages nas settings
- Seu link será: `seu-username.github.io/pagina-de-venda.html`

---

## Checklist antes de publicar

- [ ] Abriu o HTML no navegador?
- [ ] Leu as 8 seções?
- [ ] Preencheu os campos `[INSERIR X]`?
- [ ] Testou os botões de compra?
- [ ] Abriu no celular e viu se ficou bom?
- [ ] Clicou nas perguntas do FAQ (accordion)?
- [ ] A cor principal está correta?
- [ ] O link de checkout funciona?

---

## O que é QFD?

O plugin usa um método chamado **QFD** (Quadro/Furadeira/Decorado) para organizar sua copy:

### Quadro (Q)
**O PROBLEMA do seu cliente**
- A dor: qual é o problema?
- Por que agora é importante resolver?
- Exemplo: "Você está cansado de trabalhar 12h por dia"

### Furadeira (F)
**A PROVA que funciona**
- Depoimentos de clientes
- Suas credenciais
- Resultados numéricos
- Exemplo: "João aumentou renda em 300% em 3 meses"

### Decorado (D)
**O APELO IRRESISTÍVEL**
- Urgência (vagas limitadas, prazo)
- FOMO (medo de perder)
- Emoção (agora é a chance!)
- Exemplo: "10 vagas restantes. Preço sobe amanhã."

---

## Dicas para melhor resultado

### ✅ Bom copy

- **Específico**: "Ganhe R$ 5.000/mês em 90 dias" ✓ (melhor que "ganhe renda extra")
- **Prova**: "João ganhou R$ 5.000/mês" ✓ (melhor que "pessoas ganharam")
- **Urgência clara**: "5 vagas de 50" ✓ (melhor que "vagas limitadas")
- **Detalhe**: Inclua nomes, números, datas

### ❌ Evite

- Copiar/colar de outros sites (original é melhor)
- Prometer resultados impossíveis
- Omitir garantia/segurança
- Esquecer CTA (call-to-action) claro

---

## Erros comuns

### ❌ "Meu copy não foi reconhecido"

**Solução**:
- Verifique se você colou o texto completo do copy no chat
- O texto precisa incluir headline, benefícios, depoimentos, etc
- Se tiver dúvidas, use a estrutura QFD sugerida no início

### ❌ "Os campos [INSERIR X] não desaparecem"

**Solução**:
- Você não preencheu no copy original
- Edite o HTML manualmente (procure `[INSERIR X]`)
- Ou refaça o copy incluindo essa info e rode `/criar-pagina` novamente

### ❌ "A cor não aparece nos botões"

**Solução**:
- Ao responder pergunta 1, use formato hex: `#FF6B00`
- Se respondeu algo como "laranja", o sistema usou cor padrão
- Edite o HTML manualmente, procure por `#FF6B00` e troque

### ❌ "O HTML não abre no navegador"

**Solução**:
- Clique 2x no arquivo .html (Windows/Mac)
- Ou arraste o arquivo para o navegador
- Verifique se a extensão é `.html` (não `.txt` ou outro)

---

## Suporte

Se tiver problemas:

1. **Verifique a documentação** em `README.md`
2. **Cheque os erros listados** acima
3. **Rode novamente** `/criar-pagina` (teste com um copy simples)
4. **Contacte o suporte** do seu curso/mentoria

---

## FAQ (Perguntas Frequentes)

### P: Preciso de hospedagem para usar?
**R**: Não! O arquivo .html funciona localmente. Para publicar online, sim, você precisa de hospedagem.

### P: Posso editarManualmente o HTML?
**R**: Sim! Abra com um editor de texto (VS Code, Notepad) e edite à vontade.

### P: E se mudar meu copy?
**R**: Rode `/criar-pagina` novamente. Ele sobrescreve o arquivo anterior.

### P: Quantas vezes posso rodar?
**R**: Quantas vezes quiser! Teste diferentes copies.

### P: Posso mudar cores/fontes?
**R**: Sim! Edite o arquivo .html ou rode novamente com uma cor diferente.

### P: Funciona em mobile?
**R**: Sim! A página é 100% responsiva. Teste no seu celular.

---

## Próximos passos

1. ✅ Crie um copy.docx com seu conteúdo
2. ✅ Rode `/criar-pagina`
3. ✅ Abra o HTML e revise
4. ✅ Complete os campos ausentes
5. ✅ Publique online
6. ✅ Compartilhe e venda! 🚀

---

## Referências

- **QFD Framework**: Método de copywriting estruturado em 3 camadas
- **Mobile-first**: Página funciona em todos os tamanhos de tela
- **HTML válido**: Seu arquivo é um HTML5 profissional
- **Sem dependências**: Tudo em um único arquivo, zero downloads

---

**Boa sorte! Sua página de vendas está pronta para decolar! 🚀**
