---
name: revisor-design
description: Valida o design system do quiz — verifica se os componentes sao coerentes, o kit visual nao parece generico de IA, e esta alinhado com o nicho e publico.
---

# Skill: Revisor de Design

## Missao

Ler o design system gerado e o preview HTML, e validar se o kit visual do quiz tem identidade propria, esta alinhado com o nicho/publico e os componentes estao corretos para uso em quiz de conversao.

---

## Entrada (INPUT)

- `output/design-tokens.json` (lido do arquivo)
- `output/design-preview.html` (lido do arquivo)
- `output/dados-quiz.json` (para comparar tema/publico com o design)

---

## Saida (OUTPUT)

**Se APROVADO**:
```
Design aprovado!

Checklist:
  [OK] Paleta de cores coerente e alinhada com o nicho
  [OK] Cor primaria adequada para botoes e CTA
  [OK] Tipografia legivel e proporcional ao estilo
  [OK] Cards de opcao com estado selecionado/nao-selecionado claros
  [OK] Barra de progresso presente e proporcional
  [OK] Botao CTA com contraste suficiente
  [OK] Formulario de captura com campos visiveis
  [OK] Loading screen com checklist animado definido
  [OK] Design com identidade propria (nao parece template generico de IA)

Seguindo para a revisao final do quiz...
```

**Se AJUSTES NECESSARIOS**:
```
Design precisa de ajustes.

Problemas encontrados:
  [PROBLEMA] [descricao do problema especifico]

Acoes necessarias:
  - [acao especifica para corrigir]

O designer-quiz vai corrigir e apresentar novo preview.
```

---

## Checklist de Validacao (10 pontos)

### 1. Coerencia com o Nicho (critico)

Comparar `design-tokens.json` com `dados-quiz.json`:

- [ ] A `cor_primaria` combina com o tema e publico do produto
  - Ex: Cor vibrante para fitness/motivacao ✓ | Cor vibrante para juridico/financas ✗
- [ ] A tipografia reflete o nivel de sofisticacao do produto
  - Ex: Montserrat Bold para quiz de vendas ✓ | Cormorant Garamond para quiz de fitness ✗
- [ ] O `card_estilo` e adequado ao nicho
  - Ex: `elevado` para produto premium ✓ | `flat` para produto direto ao ponto ✓

**Falha critica**: Cor ou tipografia completamente incompativel com o nicho → AJUSTE NECESSARIO

---

### 2. Paleta Completa e Funcional (critico)

- [ ] `cor_primaria` esta definida e e uma cor vibrante/distincta (nao cinza, nao branco)
- [ ] `cor_primaria_dark` existe e e visivelmente mais escura que `cor_primaria`
- [ ] `cor_primaria_light` existe e e muito mais clara (serve como fundo de card selecionado)
- [ ] `cor_fundo` e `cor_fundo_card` sao diferentes entre si
- [ ] `cor_borda` e `cor_borda_ativa` sao diferentes entre si
- [ ] `cor_texto` tem contraste suficiente sobre `cor_fundo` (minimo 4.5:1)
- [ ] `cor_progresso_bg` e claramente diferente de `cor_primaria`

**Falha critica**: Cor primaria indefinida ou sem contraste → AJUSTE NECESSARIO

---

### 3. Cards de Opcao (critico)

Verificar no preview HTML (Secoes 6 e 7):

- [ ] Estado nao-selecionado e visivelmente diferente do estado selecionado
- [ ] Estado selecionado usa `cor_primaria_light` como fundo E `cor_borda_ativa` como borda
- [ ] Letra da opcao (A, B, C, D) e visivelmente destacada com `cor_primaria`
- [ ] Cards de multipla-escolha tem checkbox visual claro
- [ ] Tamanho dos cards e adequado para touch (minimo 48px de altura)

**Falha critica**: Impossivel distinguir selecionado de nao-selecionado → AJUSTE NECESSARIO

---

### 4. Botao CTA (critico)

- [ ] Contraste entre texto do botao e fundo do botao e alto (branco sobre primaria, ou similar)
- [ ] Border-radius definido e consistente com o estilo
- [ ] Estado hover (cor_primaria_dark) esta definido
- [ ] O botao e visivelmente o elemento mais chamativo da tela

**Falha**: Botao com baixo contraste → AJUSTE NECESSARIO

---

### 5. Barra de Progresso

- [ ] Altura e raio definidos em `componentes.progresso_altura` e `componentes.progresso_raio`
- [ ] `cor_progresso_bg` e discreta (nao chama atencao sozinha)
- [ ] `cor_primaria` e usada como fill da barra
- [ ] Preview mostra a barra em pelo menos 3 estados (25%, 60%, 90%)

---

### 6. Formulario de Captura

- [ ] Inputs tem fundo branco ou claro, borda `cor_borda`
- [ ] Foco dos inputs usa `cor_borda_ativa` (cor_primaria)
- [ ] Raio dos inputs definido em `componentes.input_raio`
- [ ] Campos sao visivelmente distinguiveis do fundo da tela

---

### 7. Loading Screen

- [ ] Preview mostra checklist com itens em 3 estados: concluido, em andamento, pendente
- [ ] Item concluido usa `cor_primaria` (checkmark colorido)
- [ ] Item em andamento tem indicador visual de animacao (pulsando)
- [ ] Barra de progresso do loading esta presente

---

### 8. Tipografia Legivel

- [ ] `fonte_titulo` e `fonte_corpo` sao do Google Fonts e carregam no preview
- [ ] Tamanho do titulo da pergunta e adequado (minimo 20px)
- [ ] Contraste do texto sobre os fundos e suficiente
- [ ] Peso da fonte do titulo e maior que o peso do corpo

---

### 9. Kit Completo (10 secoes presentes)

Verificar se o preview HTML tem todas as secoes:
- [ ] Secao 1: Cabecalho do preview
- [ ] Secao 2: Paleta de cores (swatches)
- [ ] Secao 3: Tipografia
- [ ] Secao 4: Barra de progresso
- [ ] Secao 5: Botao CTA
- [ ] Secao 6: Cards de escolha unica
- [ ] Secao 7: Cards de multipla escolha
- [ ] Secao 8: Formulario de captura
- [ ] Secao 9: Loading screen
- [ ] Secao 10: Mockup de tela completa

---

### 10. Identidade Propria (Nao Parece IA Generico)

- [ ] A combinacao cor + fonte NAO e a combinacao mais generica possivel
  - Ex: Azul #007BFF + Roboto + border-radius 4px = template padrao de IA → FALHA
- [ ] Existe pelo menos 1 escolha de design que diferencia visualmente do padrao
  - Ex: fonte incomum, cor fora do padrao, estilo de botao pill em vez de square, etc.
- [ ] O design transmite personalidade alinhada com o produto (nao e neutro demais)

**Falha**: Design completamente generico, sem identidade → AJUSTE NECESSARIO

---

## Classificacao Final

### APROVADO
- Todos os checks 1-4 (criticos) passaram
- Maximo 2 checks menores falharam

### AJUSTES NECESSARIOS
- 1-2 checks criticos falharam
- Listar exatamente o que o designer-quiz deve corrigir

### REPROVADO (raro)
- 3+ checks criticos falharam
- O designer-quiz deve refazer o design system do zero

---

## Procedimento

1. Ler `output/design-tokens.json`
2. Ler `output/design-preview.html`
3. Ler `output/dados-quiz.json`
4. Executar os 10 checks em ordem
5. Classificar: APROVADO / AJUSTES / REPROVADO
6. Retornar resultado ao chat (checklist + lista de problemas se houver)
7. Se AJUSTES: especificar exatamente o que corrigir
8. Se APROVADO: confirmar que o design esta pronto e o quiz pode ser revisado
