# Plugin Página de Venda RTG

**Transforma copy de vendas em página HTML profissional — sem programação.**

Um plugin para [Claude Cowork](https://claude.com/product/cowork) que converte seu copy de vendas (documento `.docx`) numa página HTML completa, responsiva e pronta para publicar.

## Instalação

```bash
claude plugins add plugin-pagina-de-venda
```

Ou importe a pasta diretamente no Claude Cowork.

---

## Como funciona

```
Seu copy (Word)
     ↓
5 perguntas (cor, link, urgência, etc)
     ↓
Agente extrai estrutura (QFD framework)
     ↓
Agente gera HTML (8 seções profissionais)
     ↓
Página pronta para publicar 🎉
```

---

## Uso rápido

1. **Coloque seu copy** em `copy.docx` (na pasta do plugin)
2. **Abra Claude Cowork** e navegue para a pasta do plugin
3. **Digite no chat**: `/criar-pagina`
4. **Responda as 5 perguntas**:
   - Cor principal (ex: `#FF6B00`)
   - Link de checkout
   - Urgência/escassez (ex: "10 vagas")
   - Nome do produto
   - Tipo de depoimentos (texto ou vídeo)
5. **Pronto!** Seu arquivo HTML foi gerado em `output/pagina-de-venda.html`

---

## Comandos

### `/criar-pagina`

Orquestra todo o fluxo:
- Lê seu copy.docx
- Faz 5 perguntas interativas
- Extrai estrutura (agente **extrator-copy**)
- Gera HTML (agente **construtor-html**)
- Salva em `output/pagina-de-venda.html`

---

## Skills

### `extrator-copy`

Extrai o copy bruto e estrutura em JSON usando o **framework QFD**:

- **Quadro (Q)**: O problema do cliente (dor, contexto)
- **Furadeira (F)**: Prova e evidência (depoimentos, credenciais)
- **Decorado (D)**: Apelo irresistível (urgência, FOMO)

**Regra crítica**: Nunca inventa. Campos faltando aparecem como `[INSERIR X]` no HTML.

### `construtor-html`

Gera HTML com **8 seções profissionais**:

1. **Hero** — Headline + subheadline + CTA + urgência
2. **Benefícios** — Grid de 3 colunas com ícones
3. **Autoridade Expert** — Bio, credenciais, resultados
4. **Depoimentos** — Cards texto ou iframes vídeo
5. **Bônus** — Cards com preço riscado
6. **Oferta + Preço + Garantia** — Preço destacado + garantia
7. **FAQ** — Accordion interativo
8. **CTA Final** — Último apelo emocional + botão

**Features**:
- Mobile-first responsivo
- Design system integrado (cores, tipografia, espaçamento)
- Sticky CTA button (aparece após scroll)
- FAQ accordion (clique para expandir/recolher)
- Countdown timer (opcional, comentado)
- Self-contained (tudo em um arquivo `.html`, zero dependências)

---

## Estrutura de arquivos

```
plugin-pagina-de-venda/
├── .claude-plugin/
│   └── plugin.json              ← Metadados do plugin
├── commands/
│   └── criar-pagina.md          ← Comando principal
├── skills/
│   ├── extrator-copy/
│   │   └── SKILL.md             ← Extrai e estrutura copy
│   └── construtor-html/
│       └── SKILL.md             ← Gera HTML completo
├── README.md                    ← Este arquivo
├── LICENSE
├── copy.docx                    ← Seu copy aqui
├── output/
│   └── pagina-de-venda.html    ← HTML gerado
└── ia/
    └── LEIA-ME.md              ← Instruções para o mentorado
```

---

## Exemplo de fluxo

```
Você: /criar-pagina

Claude Cowork: Lendo copy.docx... ✓

Claude Cowork: Pergunta 1 de 5: Qual é a cor principal? (ex: #FF6B00)
Você: #FF6B00

Claude Cowork: Pergunta 2 de 5: Link de checkout?
Você: https://checkout.sua-empresa.com/produto123

Claude Cowork: Pergunta 3 de 5: Urgência/escassez?
Você: 10 vagas restantes

Claude Cowork: Pergunta 4 de 5: Nome do produto?
Você: Python para Análise de Dados

Claude Cowork: Pergunta 5 de 5: Depoimentos (texto ou video)?
Você: texto

Claude Cowork: Extraindo estrutura...
Claude Cowork: Gerando HTML...

✅ Sucesso! Página salva em output/pagina-de-venda.html

Campos ausentes (complete manualmente):
  • expert.credenciais
  • bonus
  • faq
```

---

## Abrindo o HTML

Depois que o plugin gera o arquivo:

1. **Localize** `output/pagina-de-venda.html`
2. **Clique 2x** para abrir no navegador
3. **Revise** as 8 seções
4. **Complete** os campos `[INSERIR X]` (se houver)
5. **Publique** (suba para seu hosting, WordPress, etc)

---

## Abrindo o HTML

### No seu computador (local)

1. Procure `output/pagina-de-venda.html`
2. Clique 2x para abrir (abre no navegador automaticamente)

### Publicar online

**Opção 1: Seu servidor**
- Suba o arquivo via FTP/SFTP
- Coloque na raiz do seu domínio

**Opção 2: WordPress**
- Crie página em branco
- Cole o HTML (modo texto)
- Publique

**Opção 3: GitHub Pages (gratuito)**
1. Crie repositório no GitHub
2. Suba o arquivo .html
3. Ative GitHub Pages (settings)
4. Seu link: `seu-username.github.io/pagina-de-venda.html`

**Opção 4: Vercel/Netlify (gratuito)**
1. Conecte seu repositório
2. Suba o arquivo
3. Recebe URL automática

---

## Framework QFD

O plugin usa o **método QFD** (Quadro/Furadeira/Decorado) para estruturar vendas:

### Quadro (Q) — O PROBLEMA
Qual é a dor do cliente? Por que agora é importante resolver?

*Exemplo*: "Você está cansado de trabalhar 12h por dia e não consegue crescer financeiramente."

### Furadeira (F) — PROVA E EVIDÊNCIA
Como você sabe que funciona? Depoimentos, credenciais, números.

*Exemplo*: "João aumentou renda em 300% em 3 meses. Certificado Google Cloud."

### Decorado (D) — APELO IRRESISTÍVEL
Urgência, FOMO, emoção. Agora é a chance!

*Exemplo*: "10 vagas restantes. Preço sobe amanhã. Garanta agora."

---

## Dicas para melhor resultado

### ✅ Bom copy

- **Específico**: "Ganhe R$ 5.000/mês em 90 dias" (melhor que "ganhe renda extra")
- **Prova**: "João ganhou R$ 5.000/mês" (melhor que "pessoas ganharam")
- **Urgência clara**: "5 vagas de 50" (melhor que "vagas limitadas")
- **Detalhes**: Nomes, números, datas

### ❌ Evite

- Copiar/colar de outros sites (original é melhor)
- Prometer resultados impossíveis
- Omitir garantia/segurança
- Esquecer CTA (call-to-action) claro

---

## Campos faltando?

Se o plugin não encontrou uma informação no seu copy:

1. **Veja a lista** na mensagem de sucesso
2. **Abra o HTML** em editor de texto
3. **Procure por** `[INSERIR X]`
4. **Substitua** com a informação correta
5. **Salve** e pronto!

**Ou refaça**: Atualize seu copy.docx e rode `/criar-pagina` novamente.

---

## Requisitos

- Claude Cowork (aplicativo Anthropic)
- Python 3 (para ler .docx, fallback automático)
- Arquivo `copy.docx` com seu copy de vendas

---

## Checklist antes de publicar

- [ ] Abriu o HTML no navegador?
- [ ] Leu as 8 seções?
- [ ] Preencheu os campos `[INSERIR X]`?
- [ ] Testou os botões de compra?
- [ ] Abriu no celular (teste responsive)?
- [ ] Clicou nas perguntas do FAQ?
- [ ] A cor principal está correta?
- [ ] O link de checkout funciona?

---

## Suporte

Para problemas:

1. **Veja `ia/LEIA-ME.md`** — Guia completo em português
2. **Rodando novamente** — `/criar-pagina` com um copy simples
3. **Contacte suporte** — Se o erro persistir

---

## License

MIT License — Veja [LICENSE](LICENSE) para detalhes.

---

**Pronto para transformar seu copy em uma página de vendas profissional? 🚀**

Coloque seu copy em `copy.docx` e digite `/criar-pagina`!
