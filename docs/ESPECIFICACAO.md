# Especificação do Produto — App de Gestão Financeira Pessoal

> **Status:** Rascunho v0.1 para refinamento.
> **Data:** 2026-09-07
> **Etapa atual:** Planejamento de produto (Product Management). Nenhum código deve ser escrito ainda.
> **Próximas etapas:** (2) Identidade visual / design → (3) Arquitetura técnica → (4) Desenvolvimento.

Este documento é um **rascunho vivo**. As seções marcadas com ⚠️ **A VALIDAR** contêm suposições que assumi para preencher lacunas — precisam do seu "ok" ou ajuste antes de congelarmos a especificação.

---

## 1. Visão geral

Um aplicativo **web** de gestão financeira pessoal que centraliza **toda** a vida financeira de um casal em um só lugar: patrimônio, bens, dívidas, financiamentos, gastos, assinaturas, investimentos e projeção de independência financeira.

O produto deve responder, a qualquer momento e com precisão, a perguntas como:

- Quanto eu tenho, quanto eu devo e qual é meu **patrimônio líquido** hoje?
- Para onde vai meu dinheiro? Quais são meus **maiores gastos** e **assinaturas**?
- Qual a melhor forma de **quitar minhas dívidas** e quanto de juros eu economizo antecipando?
- Minha **carteira de investimentos** está distribuída de forma ideal? Quanto preciso aportar para rebalancear?
- **Com quantos anos** vou atingir a liberdade financeira / poder me aposentar?

### 1.1 Princípios do produto (o que "bom" significa aqui)

O próprio pedido definiu três pilares inegociáveis. Eles guiam toda decisão de design e engenharia:

1. **Fácil** — qualquer lançamento em poucos toques; nada de tela intimidadora. Um casal sem formação em finanças precisa conseguir usar.
2. **Rápido** — abrir e entender a situação em segundos; lançamentos ágeis; sem travar.
3. **Preciso** — os números têm que estar **certos**. Cálculos financeiros (juros, amortização, projeções) seguem fórmulas corretas e auditáveis. Dinheiro nunca é representado por número de ponto flutuante que acumula erro de arredondamento.

---

## 2. Decisões já tomadas (validadas com você)

Estas decisões saíram das nossas rodadas de perguntas e são a **base fixa** da especificação:

| Tema | Decisão |
|------|---------|
| **Plataforma** | Web app (navegador), acessível de celular e computador (responsivo, mobile-first). |
| **Entrada de dados** | Manual + importação de planilhas/extratos (CSV/OFX). **Sem** integração bancária (Open Finance) nesta versão. |
| **Usuários** | Compartilhado: você + cônjuge. Finanças da família em conjunto, com login individual. |
| **Hospedagem** | Nuvem, sincronizado entre os dois, com login. **Custo zero** (usar planos gratuitos). |
| **Mercado / moeda** | Brasil, moeda principal **R$ (BRL)**. Suporte a ativos no exterior e cripto (com câmbio). |
| **Classes de ativo do usuário hoje** | Renda variável BR (ações, FIIs, ETFs), cripto/exterior, bens físicos (casa, carro). Renda fixa será suportada mesmo sem posição atual. |
| **Carteira ideal** | Híbrido: app **sugere** uma alocação a partir do perfil de risco **e** o usuário pode **ajustar** as metas manualmente. |
| **Aposentadoria / liberdade financeira** | Dois cenários: (a) FIRE — renda passiva cobre os gastos (regra dos 4%); (b) metas definidas pelo usuário (valor ou renda-alvo). |
| **Dívidas** | Registrar juros e parcelas, mostrar custo total de juros, sugerir estratégia de quitação (bola de neve x avalanche) e simular antecipação. |
| **Assinaturas / cartão** | Cadastro **manual**; app soma, categoriza e ranqueia os maiores gastos. Sem detecção automática. |
| **Orçamento / metas** | Orçamento por categoria **opcional e flexível** (avisa ao estourar, mas não é obrigatório) + reserva de emergência + metas de objetivos. |
| **Tela inicial** | Dashboard de "visão geral equilibrada": patrimônio + mês atual + progresso rumo à liberdade financeira. |
| **Entrega** | Especificação completa de toda a visão; implementação organizada em fases (ver §12). |

---

## 3. Público e cenários de uso

**Usuários:** um casal brasileiro que quer organizar a vida financeira sem depender de planilhas manuais complexas. Perfil não necessariamente técnico.

**Cenários típicos (histórias de uso):**

- *"É fim de semana, vou lançar os gastos do cartão da semana"* → lançamento rápido de despesas, em lote, pelo celular.
- *"Recebi o extrato do banco em CSV"* → importar arquivo, revisar e categorizar em massa.
- *"Sobrou dinheiro este mês, quanto aporto em cada investimento?"* → tela de rebalanceamento diz exatamente quanto colocar em cada classe.
- *"Vale a pena antecipar o financiamento do carro?"* → simulador mostra economia de juros e nova data de quitação.
- *"Estamos no caminho certo pra aposentadoria?"* → dashboard FIRE mostra a idade projetada e o quanto falta.
- *"Quanto estamos gastando com assinaturas que nem usamos?"* → relatório de assinaturas ativas e total mensal/anual.

---

## 4. Módulos e funcionalidades

Cada módulo abaixo lista **o que faz** e as **regras principais**. Fórmulas detalhadas em §5.

### 4.1 Dashboard (tela inicial) — "Visão geral equilibrada"

Painel de abertura com os indicadores-chave, agrupados em três blocos:

- **Patrimônio:** patrimônio líquido total (bens + investimentos − dívidas), com variação no período (mês/ano). Mini-gráfico de evolução.
- **Mês atual:** total que entrou, total que saiu, saldo do mês (fluxo de caixa), e status do orçamento (se configurado).
- **Liberdade financeira:** % do caminho já percorrido rumo ao FIRE e **idade projetada** de aposentadoria.

Complementos: atalhos rápidos ("lançar despesa", "importar extrato"), alertas relevantes (contas a vencer, orçamento estourado, carteira desbalanceada), e maiores gastos do mês.

### 4.2 Contas e saldos

Cadastro das "carteiras" onde o dinheiro está: conta corrente, poupança, dinheiro físico, corretoras, carteiras de cripto. Cada conta tem saldo, e os saldos somam no patrimônio.
- Suporte a **contas conjuntas** e contas individuais de cada cônjuge.

### 4.3 Receitas (entradas)

Registro de rendas: salários, pró-labore, aluguéis, rendimentos, extras.
- Receitas **recorrentes** (ex.: salário todo dia 5) e avulsas.
- Por titular (você / cônjuge) para permitir análise individual e familiar.

### 4.4 Despesas e categorização

Registro de saídas de dinheiro, com:
- **Categorias e subcategorias** (moradia, alimentação, transporte, lazer, saúde, educação...) — conjunto padrão editável pelo usuário.
- Despesas **recorrentes** (contas fixas) e avulsas.
- Vínculo opcional com conta, cartão, forma de pagamento e titular.
- Lançamento em lote (rápido) e edição em massa.

### 4.5 Cartão de crédito

- Cadastro de cartões (limite, dia de fechamento, dia de vencimento).
- Lançamento de compras no cartão, incluindo **compras parceladas** (o app distribui as parcelas nas faturas futuras).
- Visão de **fatura por mês** (o que já fechou, o que está aberto).
- Relatório de **maiores gastos no cartão** por categoria, estabelecimento e período.

### 4.6 Assinaturas (cadastro manual)

- Cadastro de serviços recorrentes (streaming, apps, academia, software...) com valor, ciclo (mensal/anual) e forma de pagamento.
- Painel com **total mensal e anualizado** de assinaturas.
- Ranking das assinaturas mais caras e sinalização de "quanto isso custa por ano".
- Marcar assinatura como ativa/cancelada para acompanhar economia ao cortar.

### 4.7 Dívidas e financiamentos

Coração do módulo de "sair do vermelho". Para cada dívida/financiamento:
- Cadastro com: valor original, saldo devedor atual, **taxa de juros**, número e valor das parcelas, sistema de amortização (**Price** ou **SAC**), data de início/fim.
- **Custo total de juros** projetado até o fim.
- **Cronograma de amortização** (parcela a parcela: quanto é juros, quanto é principal, saldo restante).
- **Estratégia de quitação** quando há várias dívidas: comparação entre **Avalanche** (maior juro primeiro — economiza mais) e **Bola de Neve** (menor saldo primeiro — motivacional), mostrando data de liberdade das dívidas e juros totais em cada estratégia.
- **Simulador de antecipação:** "se eu pagar R$ X a mais por mês (ou um aporte único de R$ Y), quito em quanto tempo e economizo quanto de juros?".

### 4.8 Investimentos (carteira atual)

Registro das posições reais, por classe de ativo:
- **Renda fixa BR:** Tesouro Direto, CDB, LCI/LCA, poupança, fundos DI (com indexador: prefixado, CDI/%CDI, IPCA+).
- **Renda variável BR:** ações (B3), FIIs, ETFs nacionais.
- **Cripto e exterior:** criptomoedas, ações/ETFs internacionais (valor convertido para R$ pelo câmbio).
- Cada posição: quantidade, preço médio, valor investido, valor atual, rentabilidade.
- **Consolidação:** valor total investido, distribuição atual por classe (gráfico de pizza), rentabilidade da carteira.

⚠️ **A VALIDAR (cotações):** buscar preços de ações/FIIs/cripto/câmbio automaticamente exige uma fonte de dados (API). Há opções gratuitas com limites (ex.: brapi.dev para B3, APIs públicas de cripto/câmbio). **Proposta:** começar com **atualização manual de preços** (você informa o valor atual) e, se as APIs gratuitas se mostrarem confiáveis, adicionar atualização automática numa fase posterior. Confirmar preferência.

### 4.9 Carteira ideal e rebalanceamento

- **Questionário de perfil de risco** (conservador / moderado / agressivo) → o app **sugere** uma alocação-alvo por classe (ex.: X% renda fixa, Y% ações, Z% FIIs, W% exterior/cripto).
- Usuário pode **ajustar manualmente** os percentuais-alvo.
- **Comparação alvo × atual:** quanto cada classe está acima/abaixo da meta.
- **Sugestão de aporte:** dado um valor a investir, o app diz **quanto colocar em cada classe** para aproximar a carteira da meta (rebalanceamento por aporte, sem precisar vender).

⚠️ **A VALIDAR (aviso legal):** o app **não é** um consultor de investimentos certificado (CVM). As sugestões de alocação são **educativas/organizacionais**, baseadas em modelos genéricos de perfil de risco, e não recomendação de compra/venda de ativos específicos. Vamos incluir esse aviso na interface.

### 4.10 Aposentadoria / Liberdade financeira (FIRE + metas)

Dois cenários, lado a lado:

**Cenário A — FIRE (renda passiva cobre os gastos):**
- Calcula o **"número mágico"** = gasto anual desejado ÷ taxa de retirada segura (padrão **regra dos 4%** → 25× o gasto anual). Taxa ajustável.
- Projeta, com base no patrimônio investido atual, nos aportes mensais e numa taxa de retorno real estimada, **em quantos anos / com que idade** o patrimônio atinge o número mágico.

**Cenário B — Metas definidas por você:**
- "Quero R$ X acumulados" ou "quero R$ Y de renda passiva por mês" → o app calcula a data/idade em que isso é atingido com os aportes atuais, ou quanto precisa aportar para bater a meta numa data escolhida.

Ambos exibem projeção com premissas transparentes e editáveis (retorno esperado, inflação, aportes), e um gráfico da evolução do patrimônio ao longo do tempo. Análise de sensibilidade simples ("e se eu aportar R$200 a mais por mês?").

⚠️ **A VALIDAR (premissas padrão):** sugiro valores iniciais editáveis — retorno real (acima da inflação) de **4% a.a.**, inflação de referência **~4,5% a.a.**, taxa de retirada segura **4%**. Confirmar ou ajustar.

### 4.11 Orçamento (opcional e flexível)

- Definir **limite mensal por categoria** (ex.: R$ 800 em mercado). Opcional: quem não define, só acompanha.
- Barra de progresso por categoria e **aviso ao se aproximar/estourar** o limite.
- Comparativo "orçado × realizado" no mês.

### 4.12 Reserva de emergência

- App calcula **quantos meses de custo de vida** você já tem guardado (com base no gasto médio mensal e no saldo em ativos líquidos).
- Meta configurável (padrão sugerido: **6 meses**), com barra de progresso e quanto falta.

### 4.13 Metas e objetivos

- Criar objetivos (viagem, entrada de imóvel, troca de carro) com valor-alvo e prazo.
- Acompanhar progresso, quanto guardar por mês para chegar no prazo, e vincular a uma conta/reserva.

### 4.14 Bens / Patrimônio físico

- Cadastro de bens (casa, carro, imóveis, outros) com valor atual estimado.
- Entram no cálculo do patrimônio líquido.
- ⚠️ **A VALIDAR:** permitir **depreciação/valorização** manual periódica (você atualiza o valor do carro/imóvel de tempos em tempos). Sem avaliação automática de mercado.

### 4.15 Importação de planilhas/extratos (CSV/OFX)

- Upload de arquivo → **pré-visualização** → mapeamento de colunas (data, descrição, valor) → **categorização assistida** (o app sugere categorias com base em lançamentos anteriores parecidos) → confirmação.
- Detecção de **duplicatas** para não lançar a mesma transação duas vezes.
- Suporte inicial a **CSV**; **OFX** numa fase seguinte (formato mais estruturado, comum em bancos brasileiros).

### 4.16 Relatórios e visão histórica

- Evolução do patrimônio líquido no tempo.
- Fluxo de caixa mensal (entradas × saídas × saldo).
- Gastos por categoria (mês/ano), maiores gastos, maiores estabelecimentos.
- Rentabilidade e evolução da carteira de investimentos.
- Exportar relatórios (CSV/PDF) — ⚠️ **A VALIDAR** se é desejado.

---

## 5. Regras de cálculo (precisão)

Estas fórmulas garantem o pilar "preciso". Todas usam **aritmética de valores monetários em inteiros (centavos)** ou decimal de precisão fixa — nunca float binário.

- **Patrimônio líquido** = (saldos em contas + valor atual dos investimentos + valor dos bens) − (saldo devedor de todas as dívidas).
- **Amortização Price** (parcela fixa): `PMT = PV × [ i(1+i)^n ] / [ (1+i)^n − 1 ]`, decompondo cada parcela em juros (`saldo × i`) e principal (`PMT − juros`).
- **Amortização SAC** (amortização constante): principal fixo por parcela = `PV / n`; juros = `saldo × i`; parcela decrescente.
- **Custo total de juros** = soma dos juros de todas as parcelas do cronograma.
- **Simulação de antecipação** = recalcular o cronograma reduzindo o saldo devedor pelo aporte extra e comparar juros totais e nº de parcelas com o cenário original.
- **Estratégia de quitação** = simular pagamento das dívidas alocando o valor extra na dívida de maior taxa (avalanche) ou menor saldo (bola de neve), rolando o valor liberado para a próxima dívida ("efeito bola de neve").
- **Número mágico FIRE** = gasto anual desejado ÷ taxa de retirada segura (4% → ×25).
- **Projeção de acumulação** = valor futuro com aportes: `FV = P(1+r)^t + PMT × [ ((1+r)^t − 1) / r ]`, com `r` = retorno real mensal, iterando até atingir o número mágico/meta.
- **Reserva de emergência (em meses)** = ativos líquidos ÷ custo de vida mensal médio.
- **Rebalanceamento por aporte** = distribuir o novo aporte priorizando as classes mais abaixo da meta até reaproximar os percentuais-alvo.

---

## 6. Modelo de dados (alto nível)

Entidades principais (detalhamento na etapa de engenharia):

`Família/Household` → agrupa os usuários · `Usuário` (você, cônjuge) · `Conta` (banco/corretora/cripto) · `Receita` · `Despesa` (com categoria/subcategoria) · `Categoria` · `Cartão` · `Compra no cartão / Parcela` · `Assinatura` · `Dívida/Financiamento` + `Parcela de amortização` · `Ativo de investimento` + `Posição` · `Bem` (patrimônio físico) · `Perfil de risco` + `Alocação-alvo` · `Orçamento` (por categoria/mês) · `Meta/Objetivo` · `Premissas de projeção` (retorno, inflação, taxa de retirada).

---

## 7. Requisitos não-funcionais

- **Compartilhamento (casal):** dois logins compartilhando os mesmos dados da família; toda alteração de um aparece para o outro (sincronização).
- **Segurança:** finanças são dados sensíveis. Senhas com hash, dados por família isolados (um casal nunca vê dados de outro), conexão criptografada (HTTPS).
- **LGPD:** dados pessoais e financeiros tratados com o mínimo necessário; usuário pode exportar e excluir seus dados.
- **Custo zero:** stack em planos gratuitos (ver §8). Sem cobrança para o uso pessoal do casal.
- **Responsivo:** funciona bem no celular (uso no dia a dia) e no computador (planejamento).
- **Precisão monetária:** valores em centavos/decimal fixo; datas e fusos tratados corretamente.
- **Offline leve (desejável):** ⚠️ **A VALIDAR** — permitir lançar algo mesmo sem internet e sincronizar depois (PWA). Pode ficar para fase futura.
- **Idioma:** Português do Brasil; formatação R$, datas dd/mm/aaaa.

---

## 8. Stack recomendada (proposta — a fechar na etapa de engenharia)

Para atender "nuvem + login + sincronização + custo zero", a proposta é:

- **Front-end:** aplicação web responsiva (mobile-first), hospedada na **Vercel** (plano grátis).
- **Back-end / banco / autenticação:** **Supabase** (plano grátis) — banco PostgreSQL, login/senha, sincronização e regras de acesso por família.
- **PWA** (opcional) para instalar no celular e uso mais fluido.

⚠️ **A VALIDAR:** a stack exata (framework de front, biblioteca de UI, etc.) será detalhada e justificada na etapa 3 (arquitetura). Aqui fica só o rumo para garantir viabilidade do "gratuito".

---

## 9. Fora de escopo (versão 1)

Para manter o produto **fácil, rápido e preciso**, ficam de fora nesta versão (candidatos a futuro):

- Integração bancária automática via Open Finance (Pluggy/Belvo).
- Detecção automática de assinaturas/recorrências a partir de extratos.
- Consultoria de investimentos personalizada / recomendação regulada de ativos.
- Cálculo tributário automático (IR sobre investimentos, ganho de capital).
- App nativo iOS/Android (o web app responsivo/PWA cobre o uso mobile).
- Multimoeda completa com câmbio em tempo real como recurso central (haverá conversão para R$, mas o foco é BRL).

---

## 10. Perguntas em aberto / suposições a validar (resumo)

Antes de congelar a especificação, preciso do seu retorno sobre os itens ⚠️ espalhados no texto, consolidados aqui:

1. **Cotações de investimentos:** começar com atualização **manual** de preços e evoluir para automática depois? (§4.8)
2. **Premissas de projeção FIRE:** aceita retorno real 4% a.a., inflação ~4,5% a.a., retirada segura 4% (todos editáveis)? (§4.10)
3. **Bens físicos:** atualização de valor **manual** e periódica é suficiente? (§4.14)
4. **Exportar relatórios** (CSV/PDF): é desejado na v1? (§4.16)
5. **Uso offline / PWA:** interessa já na v1 ou pode ficar para depois? (§7)
6. **Receitas por titular:** faz sentido separar "renda sua" x "renda do cônjuge" para análises, ou tratar tudo como renda da família? 
7. **Fonte de importação:** de quais bancos/apps você exporta hoje (Nubank, Itaú, etc.)? Isso ajuda a priorizar os formatos de CSV/OFX.
8. Algo importante da sua realidade financeira que **não** apareceu aqui e deveria ser modelado?

---

## 11. Critérios de sucesso (como saberemos que deu certo)

- Você consegue lançar um gasto em **menos de 10 segundos**.
- Ao abrir o app, entende sua situação financeira em **menos de 5 segundos** (dashboard).
- Os números batem com a realidade (auditáveis): patrimônio, juros, projeções.
- Você e o cônjuge veem os mesmos dados, atualizados.
- O app responde com clareza: "com quantos anos vou me aposentar" e "quanto aportar em cada investimento".
- Custo de operação permanece **R$ 0**.

---

## 12. Fases de implementação sugeridas (para a etapa de desenvolvimento)

A especificação cobre toda a visão; a **construção** será faseada para entregar valor cedo e reduzir risco:

- **Fase 1 — Fundação:** login/família (nuvem, compartilhado), contas, receitas, despesas com categorias, dashboard básico, patrimônio líquido, bens físicos.
- **Fase 2 — Dívidas & cartão:** cartões, compras parceladas, dívidas/financiamentos com amortização, custo de juros, estratégia de quitação e simulador de antecipação.
- **Fase 3 — Investimentos & carteira ideal:** posições por classe, consolidação, perfil de risco, alocação-alvo e rebalanceamento por aporte.
- **Fase 4 — Futuro financeiro:** projeção FIRE + metas, reserva de emergência, objetivos, orçamento por categoria.
- **Fase 5 — Conveniência:** importação CSV (e depois OFX), assinaturas, relatórios avançados, PWA/offline.

*(A ordem pode mudar conforme sua prioridade — é só me dizer o que mais te dói hoje que eu adianto.)*

---

## 13. Glossário

- **Patrimônio líquido:** tudo que você tem menos tudo que você deve.
- **FIRE (Financial Independence, Retire Early):** ponto em que a renda dos investimentos cobre seu custo de vida, dispensando trabalhar por dinheiro.
- **Regra dos 4%:** heurística que estima que dá para retirar ~4% do patrimônio ao ano de forma sustentável (logo, precisa de ~25× o gasto anual).
- **Avalanche x Bola de Neve:** estratégias de quitar dívidas priorizando a maior taxa (economiza mais) ou o menor saldo (motiva mais).
- **Price / SAC:** sistemas de amortização — parcela fixa (Price) ou amortização constante com parcela decrescente (SAC).
- **Rebalanceamento:** trazer a carteira de volta aos percentuais-alvo, aqui feito direcionando novos aportes.
