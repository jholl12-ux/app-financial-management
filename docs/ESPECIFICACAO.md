# Especificação do Produto — App de Gestão Financeira Pessoal

> **Status:** ✅ **v1.0 — Especificação de produto CONGELADA** (todas as pendências resolvidas).
> **Data:** 2026-09-07
> **Etapa atual:** Planejamento de produto (Product Management) concluído. Nenhum código deve ser escrito ainda.
> **Próximas etapas:** (2) Identidade visual / design → (3) Arquitetura técnica → (4) Desenvolvimento.

Este documento consolidou três rodadas de refinamento com o usuário. Todos os itens antes marcados como "A VALIDAR" foram decididos (ver §10).

> **Mudanças na v0.2 (após 2ª rodada de respostas):** a v1 passa a ser **local-first no celular** (roda offline, sem nuvem/login), com você gerenciando você e o cônjuge como "titulares" no mesmo aparelho; nuvem + multiusuário em tempo real viram fase futura. Cotações de investimentos passam a ser **automáticas, em tempo real, via internet**. Bens físicos ganham **depreciação/valorização**. Relatórios devem ser **visualmente bonitos, com gráficos**. Adicionado o princípio **"Educativo"** (o app explica finanças em linguagem simples, pois o usuário não tem familiaridade com o tema).

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
4. **Educativo** — o usuário **não tem familiaridade com finanças**. Todo termo técnico (FIRE, reserva de emergência, rebalanceamento, amortização) é explicado em **linguagem simples**, com tooltips/ajuda contextual e **valores padrão prontos** que a pessoa pode apenas aceitar. O app ensina enquanto organiza, sem exigir conhecimento prévio.

---

## 2. Decisões já tomadas (validadas com você)

Estas decisões saíram das nossas rodadas de perguntas e são a **base fixa** da especificação:

| Tema | Decisão |
|------|---------|
| **Plataforma** | Web app (navegador), acessível de celular e computador (responsivo, mobile-first). |
| **Entrada de dados** | Manual + importação de planilhas/extratos (CSV/OFX). **Sem** integração bancária (Open Finance) nesta versão. |
| **Usuários (v1)** | **Um dispositivo, sem login.** Você gerencia você e o cônjuge como dois **"titulares"** no mesmo app local. Multiusuário real (cada um no seu aparelho) vira fase futura. |
| **Hospedagem (v1)** | **Local-first no celular** — dados guardados no próprio aparelho, funciona **offline**, sem nuvem/servidor. **Custo zero.** Fase futura: nuvem gratuita + login + sincronização em tempo real. |
| **Mercado / moeda** | Brasil, moeda principal **R$ (BRL)**. Suporte a ativos no exterior e cripto (com câmbio). |
| **Cotações** | **Automáticas, em tempo real**, buscadas na internet (ações/FIIs B3, cripto, câmbio, ativos no exterior). Quando offline, usa o último valor conhecido. |
| **Classes de ativo do usuário hoje** | Renda variável BR (ações, FIIs, ETFs), cripto/exterior, bens físicos (casa, carro). Renda fixa será suportada mesmo sem posição atual. |
| **Carteira ideal** | Híbrido: app **sugere** uma alocação a partir do perfil de risco **e** o usuário pode **ajustar** as metas manualmente. |
| **Aposentadoria / liberdade financeira** | Dois cenários: (a) FIRE — renda passiva cobre os gastos (regra dos 4%); (b) metas definidas pelo usuário (valor ou renda-alvo). Explicado em linguagem simples, com premissas padrão prontas. |
| **Dívidas** | Registrar juros e parcelas, mostrar custo total de juros, sugerir estratégia de quitação (bola de neve x avalanche) e simular antecipação. |
| **Assinaturas / cartão** | Cadastro **manual**; app soma, categoriza e ranqueia os maiores gastos. Sem detecção automática. |
| **Orçamento / metas** | Orçamento por categoria **opcional e flexível** (avisa ao estourar, mas não é obrigatório) + reserva de emergência + metas de objetivos. |
| **Receitas** | Registradas **por titular** (você / cônjuge), com **visão individual e visão unificada** (família). |
| **Bancos para importação** | Prioridade nos formatos de **XP, Bradesco, Binance e Nomad**. |
| **Relatórios** | Simples, porém **visualmente bonitos e organizados**, com **gráficos**; exportáveis (PDF/CSV). |
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
- **Sempre vinculadas a um titular** (você / cônjuge). O app oferece **visão individual** (só sua renda, só a do cônjuge) **e visão unificada** (renda da família somada). O mesmo conceito de titular se aplica a despesas, contas, cartões e investimentos, permitindo enxergar "quem ganha, quem gasta, quem investe" e o consolidado do casal.

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
- **Cotações automáticas em tempo real:** o app busca na internet os preços do dia — ações/FIIs/ETFs da B3, criptomoedas, câmbio (USD/BRL) e ativos no exterior — e atualiza o valor da carteira automaticamente. Você não precisa digitar preço. Quando o celular estiver **offline**, o app usa o **último preço conhecido** e sinaliza "cotação de {data}".

**Fontes de cotação (gratuitas) previstas:**
- **Ações/FIIs/ETFs B3:** API pública gratuita (ex.: brapi.dev).
- **Criptomoedas:** API pública gratuita (ex.: CoinGecko).
- **Câmbio e ativos no exterior (Nomad/dólar):** API pública de câmbio + cotação de ações/ETFs internacionais.

⚠️ **A VALIDAR (detalhe técnico):** um app local no navegador pode esbarrar em bloqueios de segurança (CORS) ao chamar algumas dessas APIs diretamente. Se acontecer, a solução é um pequeno "intermediário" gratuito para buscar as cotações. Isso é detalhe de engenharia (etapa 3) e **não muda** a sua experiência — você verá as cotações atualizadas do mesmo jeito. Registro aqui só por transparência.

### 4.9 Carteira ideal e rebalanceamento

- **Questionário de perfil de risco** (conservador / moderado / agressivo) → o app **sugere** uma alocação-alvo por classe (ex.: X% renda fixa, Y% ações, Z% FIIs, W% exterior/cripto).
- Usuário pode **ajustar manualmente** os percentuais-alvo.
- **Comparação alvo × atual:** quanto cada classe está acima/abaixo da meta.
- **Sugestão de aporte:** dado um valor a investir, o app diz **quanto colocar em cada classe** para aproximar a carteira da meta (rebalanceamento por aporte, sem precisar vender).

⚠️ **A VALIDAR (aviso legal):** o app **não é** um consultor de investimentos certificado (CVM). As sugestões de alocação são **educativas/organizacionais**, baseadas em modelos genéricos de perfil de risco, e não recomendação de compra/venda de ativos específicos. Vamos incluir esse aviso na interface.

### 4.10 Aposentadoria / Liberdade financeira (FIRE + metas)

> **Em linguagem simples (o app fala assim com o usuário):** "Liberdade financeira é quando o dinheiro que seus investimentos rendem já paga todas as suas contas — aí trabalhar passa a ser opcional. Este app estima **em quantos anos** você chega lá, com base no que você já tem, no quanto guarda por mês e em uma estimativa segura de rendimento." Nenhum termo técnico é obrigatório para o usuário; tudo tem explicação em "linguagem de gente" e valores padrão já preenchidos.

Dois cenários, lado a lado:

**Cenário A — FIRE (renda passiva cobre os gastos):**
- Calcula o **"número mágico"** = gasto anual desejado ÷ taxa de retirada segura (padrão **regra dos 4%** → 25× o gasto anual). Taxa ajustável.
- Projeta, com base no patrimônio investido atual, nos aportes mensais e numa taxa de retorno real estimada, **em quantos anos / com que idade** o patrimônio atinge o número mágico.

**Cenário B — Metas definidas por você:**
- "Quero R$ X acumulados" ou "quero R$ Y de renda passiva por mês" → o app calcula a data/idade em que isso é atingido com os aportes atuais, ou quanto precisa aportar para bater a meta numa data escolhida.

Ambos exibem projeção com premissas transparentes e editáveis (retorno esperado, inflação, aportes), e um gráfico da evolução do patrimônio ao longo do tempo. Análise de sensibilidade simples ("e se eu aportar R$200 a mais por mês?").

**Premissas padrão (aprovadas, e editáveis):** retorno real (acima da inflação) de **4% a.a.**, inflação de referência **~4,5% a.a.**, taxa de retirada segura **4%**. Ficam pré-preenchidas e explicadas em linguagem simples; o usuário não precisa entender nem alterar, mas pode ajustar se quiser (com um texto de ajuda em cada campo).

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

- Cadastro de bens (casa, carro, imóveis, outros) com valor de aquisição e valor atual estimado.
- Entram no cálculo do patrimônio líquido.
- **Depreciação / valorização:** você atualiza o valor do bem ao longo do tempo (ex.: o carro desvaloriza, o imóvel valoriza). O app:
  - guarda o **histórico de valores** de cada bem e mostra a **variação** (ganho/perda) desde a compra, em R$ e em %;
  - exibe a **curva de valorização/depreciação** no tempo;
  - **opcional:** aplicar uma **taxa de depreciação/valorização estimada** (ex.: carro -10% ao ano) para o app projetar o valor automaticamente entre suas atualizações manuais, que você confirma/corrige quando quiser.
- Sem avaliação automática de mercado (você informa/atualiza os valores).

### 4.15 Importação de planilhas/extratos (CSV/OFX)

- Upload de arquivo → **pré-visualização** → mapeamento de colunas (data, descrição, valor) → **categorização assistida** (o app sugere categorias com base em lançamentos anteriores parecidos) → escolha do **titular** → confirmação.
- Detecção de **duplicatas** para não lançar a mesma transação duas vezes.
- **Formatos priorizados pelos seus bancos:** **XP**, **Bradesco**, **Binance** e **Nomad**. Cada um exporta de um jeito:
  - **Bradesco / XP:** extratos/faturas em CSV e OFX.
  - **Binance:** relatórios de transações em CSV (cripto).
  - **Nomad:** extrato de conta/investimentos em dólar (CSV) — importação com conversão para R$.
  - O app terá **perfis de importação** pré-configurados para reconhecer o layout de cada um automaticamente.
- Suporte inicial a **CSV**; **OFX** logo em seguida (formato mais estruturado, usado por Bradesco/XP).

### 4.16 Relatórios e visão histórica

Relatórios **simples de entender, porém visualmente bonitos e organizados**, com **gráficos e elementos visuais** (pizza, barras, linhas, medidores de progresso) e destaques em linguagem clara ("você gastou 12% a mais que no mês passado"). Sempre com opção de ver por titular ou unificado.
- Evolução do patrimônio líquido no tempo (gráfico de linha).
- Fluxo de caixa mensal (entradas × saídas × saldo) — barras.
- Gastos por categoria (mês/ano), maiores gastos, maiores estabelecimentos — pizza/barras e ranking.
- Rentabilidade e evolução da carteira de investimentos + distribuição por classe.
- **Exportar relatórios em PDF e CSV**, mantendo o visual bonito e organizado (o PDF sai pronto para guardar/compartilhar).

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

`Família/Household` → agrupa os titulares · `Titular` (você, cônjuge — na v1 é um rótulo local, sem login) · `Conta` (banco/corretora/cripto, vinculada a titular) · `Receita` · `Despesa` (com categoria/subcategoria) · `Categoria` · `Cartão` · `Compra no cartão / Parcela` · `Assinatura` · `Dívida/Financiamento` + `Parcela de amortização` · `Ativo de investimento` + `Posição` · `Cotação` (preço + data/hora da fonte) · `Bem` (patrimônio físico) + `Histórico de valor do bem` · `Perfil de risco` + `Alocação-alvo` · `Orçamento` (por categoria/mês) · `Meta/Objetivo` · `Premissas de projeção` (retorno, inflação, taxa de retirada) · `Backup` (exportação/importação local).

Todas as entidades transacionais carregam o **titular** para permitir as visões individual e unificada.

---

## 7. Requisitos não-funcionais

**v1 (local-first no celular):**
- **Local-first / offline:** o app roda **inteiramente no seu celular**, com os dados guardados no próprio aparelho. Funciona **sem internet** para tudo, menos buscar cotações (que atualizam quando há conexão). É um **PWA** (instalável na tela inicial, abre como um app).
- **Sem login na v1:** um único "espaço" no aparelho; você e o cônjuge são **titulares** dentro dele. Sem servidor, sem conta.
- **Backup/portabilidade:** como os dados ficam no aparelho, o app oferece **exportar e importar um backup** (arquivo) — **decidido:** backup manual é suficiente na v1 (protege ao trocar de celular ou limpar o navegador).
- **Custo zero:** nenhuma infraestrutura paga na v1.

**Comum a todas as versões:**
- **Precisão monetária:** valores em centavos/decimal fixo; datas e fusos tratados corretamente.
- **Responsivo:** foco no **celular** (uso diário); também utilizável no computador.
- **Segurança/privacidade:** dados sensíveis. Na v1 (local) os dados não saem do aparelho. **Decidido:** **sem** PIN/biometria na v1 (pode ser adicionado no futuro se desejado).
- **LGPD:** dados tratados com o mínimo necessário; usuário pode exportar e excluir seus dados.
- **Idioma:** Português do Brasil; formatação R$, datas dd/mm/aaaa.

**Fase futura (quando evoluirmos para nuvem/multiusuário):**
- Login individual para você e o cônjuge, cada um no seu aparelho, **gerindo a própria conta**, com **dados sincronizados em tempo real**.
- Backend gratuito (ver §8), dados por família isolados, HTTPS, senhas com hash.
- Migração dos dados locais da v1 para a nuvem sem perda.

---

## 8. Stack recomendada (proposta — a fechar na etapa de engenharia)

**v1 — local-first no celular (custo zero, offline):**
- **PWA** (aplicativo web instalável) rodando no navegador do celular, **mobile-first**.
- Dados guardados **localmente no aparelho** (armazenamento local do navegador, ex.: IndexedDB), sem servidor.
- Cotações buscadas em **APIs públicas gratuitas** quando online (com pequeno intermediário gratuito se necessário por causa de CORS — ver §4.8).
- **Distribuição (decidido):** instalação via **PWA por link** (adicionar à tela inicial do celular), **sem loja de apps**. O "casco" do app fica hospedado de graça (ex.: **Vercel/GitHub Pages**) — isso **não** guarda seus dados (eles ficam no aparelho), só serve a página do app.

**Fase futura — nuvem + multiusuário em tempo real (custo zero):**
- **Back-end / banco / autenticação:** **Supabase** (plano grátis) — PostgreSQL, login/senha, sincronização em tempo real e acesso por família.
- **Hospedagem do front:** **Vercel** (plano grátis).

⚠️ **A VALIDAR:** a stack exata (framework de front, biblioteca de UI, armazenamento local) será detalhada e justificada na etapa 3 (arquitetura). Aqui fica só o rumo, para garantir "gratuito", "offline" e "local na v1".

---

## 9. Fora de escopo (versão 1)

Para manter a v1 **fácil, rápida e precisa**, ficam de fora nesta versão:

- **Nuvem, login e multiusuário em tempo real** → é a **próxima grande fase** (planejada, ver §7 e §8), não a v1.
- Integração bancária automática via Open Finance (Pluggy/Belvo).
- Detecção automática de assinaturas/recorrências a partir de extratos.
- Consultoria de investimentos personalizada / recomendação regulada de ativos.
- Cálculo tributário automático (IR sobre investimentos, ganho de capital).
- App nativo iOS/Android (o PWA cobre o uso mobile, instalável na tela inicial).
- Multimoeda completa com câmbio em tempo real como recurso central (haverá conversão para R$, mas o foco é BRL).

---

## 10. Perguntas em aberto / suposições a validar (resumo)

**Já resolvidos na 2ª rodada:** cotações automáticas em tempo real (§4.8) · premissas FIRE aprovadas com defaults e explicação simples (§4.10) · bens com histórico + depreciação/valorização (§4.14) · relatórios bonitos com gráficos + export PDF/CSV (§4.16) · offline/local-first já na v1 (§7) · receitas por titular com visão unificada (§4.3) · bancos XP/Bradesco/Binance/Nomad (§4.15) · v1 local no celular, nuvem/multiusuário como fase futura (§7, §8).

**Resolvidos na 3ª rodada (não há mais pendências):**

1. **Backup na v1:** ✔️ exportar/importar arquivo de backup manual. (§7)
2. **Proteção do app:** ✔️ **sem** PIN/biometria na v1. (§7)
3. **Instalação via PWA:** ✔️ por link, sem loja de apps. (§8)
4. **Cotações:** ✔️ fontes gratuitas, pequenos atrasos aceitáveis. (§4.8)
5. **Escopo:** ✔️ usuário confirmou que está tudo contemplado.

> **A especificação está congelada.** Mudanças a partir daqui geram uma nova versão (v1.1+) e devem ser combinadas explicitamente.

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

A especificação cobre toda a visão; a **construção** será faseada para entregar valor cedo e reduzir risco. A v1 é toda **local-first no celular (PWA, offline, sem nuvem)**:

- **Fase 1 — Fundação local (PWA + offline):** app instalável no celular, armazenamento local, titulares (você/cônjuge), contas, receitas e despesas por categoria e titular (visão individual e unificada), dashboard "visão geral equilibrada", patrimônio líquido, e **backup exportar/importar**.
- **Fase 2 — Bens & dívidas:** bens físicos com histórico e depreciação/valorização; cartões e compras parceladas; dívidas/financiamentos com amortização (Price/SAC), custo de juros, estratégia de quitação (avalanche x bola de neve) e simulador de antecipação.
- **Fase 3 — Investimentos & carteira ideal:** posições por classe, **cotações automáticas em tempo real** (B3, cripto, câmbio/exterior), consolidação, perfil de risco, alocação-alvo e rebalanceamento por aporte.
- **Fase 4 — Futuro financeiro:** projeção FIRE + metas (linguagem simples), reserva de emergência, objetivos, orçamento por categoria flexível.
- **Fase 5 — Conveniência & relatórios:** importação de extratos (CSV, depois OFX) com perfis de XP/Bradesco/Binance/Nomad, assinaturas, relatórios bonitos com gráficos e exportação PDF/CSV.
- **Fase 6 (futuro) — Nuvem & multiusuário:** backend gratuito (Supabase), login individual, sincronização em tempo real, cada um gerindo a própria conta, com migração dos dados locais.

*(A ordem pode mudar conforme sua prioridade — é só me dizer o que mais te dói hoje que eu adianto.)*

---

## 13. Glossário

- **Patrimônio líquido:** tudo que você tem menos tudo que você deve.
- **FIRE (Financial Independence, Retire Early):** ponto em que a renda dos investimentos cobre seu custo de vida, dispensando trabalhar por dinheiro.
- **Regra dos 4%:** heurística que estima que dá para retirar ~4% do patrimônio ao ano de forma sustentável (logo, precisa de ~25× o gasto anual).
- **Avalanche x Bola de Neve:** estratégias de quitar dívidas priorizando a maior taxa (economiza mais) ou o menor saldo (motiva mais).
- **Price / SAC:** sistemas de amortização — parcela fixa (Price) ou amortização constante com parcela decrescente (SAC).
- **Rebalanceamento:** trazer a carteira de volta aos percentuais-alvo, aqui feito direcionando novos aportes.
