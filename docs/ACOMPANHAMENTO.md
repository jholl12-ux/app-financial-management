# Acompanhamento de Desenvolvimento — App de Gestão Financeira

> **Documento de acompanhamento em tempo real.**
> **Objetivo:** você acompanhar, a qualquer momento, **em que ponto está o desenvolvimento**, **quais tarefas deram certo ou não** e **quanto tempo** cada uma levou.
> **Como funciona:** durante o desenvolvimento (Etapa 4), eu atualizo este arquivo e faço **commit a cada tarefa concluída**. Basta abrir/atualizar este arquivo no GitHub para ver o estado mais recente.
> **Status atual:** ⏳ Desenvolvimento **ainda não iniciado** (estamos na Etapa 1 — Product Management; faltam ainda Design e Arquitetura). Todas as tarefas estão como *Pendente*.
> **Referência de escopo:** `docs/ESPECIFICACAO.md` (v1.0, congelada).

---

## Legenda de status

| Símbolo | Significado |
|:---:|---|
| ⬜ | **Pendente** — ainda não começou |
| 🔄 | **Em andamento** — sendo implementada agora |
| ✅ | **Sucesso** — concluída e validada (testes/checks passaram) |
| ⚠️ | **Concluída com ressalvas** — funciona, mas há pendência/observação |
| ❌ | **Falhou** — não passou (ver observação; será retomada) |
| ⏭️ | **Adiada** — movida para uma fase posterior |

**Colunas das tabelas:**
- **Estim.** = tempo estimado de implementação.
- **Real** = tempo efetivamente gasto (preenchido ao concluir).
- **Início / Fim** = data-hora (preenchidos durante o dev).
- **Obs.** = notas, links de commit, motivo de falha, etc.

---

## Resumo geral (atualizado ao vivo)

| Fase | Tarefas | ✅ Concluídas | 🔄 Em andamento | ❌ Falhas | Progresso |
|---|:---:|:---:|:---:|:---:|:---:|
| Fase 0 — Setup do projeto (PWA) | 6 | 0 | 0 | 0 | 0% |
| Fase 1 — Fundação local | 8 | 0 | 0 | 0 | 0% |
| Fase 2 — Bens & dívidas | 7 | 0 | 0 | 0 | 0% |
| Fase 3 — Investimentos & carteira ideal | 7 | 0 | 0 | 0 | 0% |
| Fase 4 — Futuro financeiro | 5 | 0 | 0 | 0 | 0% |
| Fase 5 — Conveniência & relatórios | 7 | 0 | 0 | 0 | 0% |
| Fase 6 (futuro) — Nuvem & multiusuário | 5 | 0 | 0 | 0 | 0% |
| **TOTAL** | **45** | **0** | **0** | **0** | **0%** |

> Tempo total estimado / real será consolidado aqui conforme o desenvolvimento avança.

---

## Fase 0 — Setup do projeto (PWA, offline, base visual)

| # | Tarefa | Status | Resultado | Estim. | Real | Início | Fim | Obs. |
|---|---|:---:|:---:|:---:|:---:|---|---|---|
| 0.1 | Estrutura do projeto e ferramentas (build, lint, testes) | ⬜ | — | — | — | — | — | — |
| 0.2 | Configurar PWA (instalável na tela inicial, ícone, manifesto) | ⬜ | — | — | — | — | — | — |
| 0.3 | Service worker + funcionamento **offline** | ⬜ | — | — | — | — | — | — |
| 0.4 | Armazenamento local no dispositivo (ex.: IndexedDB) | ⬜ | — | — | — | — | — | — |
| 0.5 | Design system base (cores, tipografia, componentes) — vem da Etapa 2 | ⬜ | — | — | — | — | — | — |
| 0.6 | Deploy inicial da "casca" (hospedagem gratuita, gerar link de instalação) | ⬜ | — | — | — | — | — | — |

---

## Fase 1 — Fundação local

| # | Tarefa | Status | Resultado | Estim. | Real | Início | Fim | Obs. |
|---|---|:---:|:---:|:---:|:---:|---|---|---|
| 1.1 | Cadastro de **titulares** (você / cônjuge) | ⬜ | — | — | — | — | — | — |
| 1.2 | **Contas** (banco/corretora/cripto) com saldo, por titular | ⬜ | — | — | — | — | — | — |
| 1.3 | **Categorias** (padrão + editáveis) | ⬜ | — | — | — | — | — | — |
| 1.4 | **Receitas** (recorrentes e avulsas), por titular | ⬜ | — | — | — | — | — | — |
| 1.5 | **Despesas** (recorrentes e avulsas) com categoria e titular | ⬜ | — | — | — | — | — | — |
| 1.6 | **Dashboard** "visão geral equilibrada" | ⬜ | — | — | — | — | — | — |
| 1.7 | **Patrimônio líquido** (cálculo consolidado) | ⬜ | — | — | — | — | — | — |
| 1.8 | **Backup** exportar/importar arquivo | ⬜ | — | — | — | — | — | — |

---

## Fase 2 — Bens & dívidas

| # | Tarefa | Status | Resultado | Estim. | Real | Início | Fim | Obs. |
|---|---|:---:|:---:|:---:|:---:|---|---|---|
| 2.1 | **Bens físicos** (cadastro, valor de aquisição e atual) | ⬜ | — | — | — | — | — | — |
| 2.2 | Histórico de valor + **depreciação/valorização** (curva e taxa opcional) | ⬜ | — | — | — | — | — | — |
| 2.3 | **Cartões de crédito** (limite, fechamento, vencimento) | ⬜ | — | — | — | — | — | — |
| 2.4 | **Compras parceladas** (distribuição nas faturas) | ⬜ | — | — | — | — | — | — |
| 2.5 | **Dívidas/financiamentos** com amortização **Price/SAC** | ⬜ | — | — | — | — | — | — |
| 2.6 | **Custo total de juros** + cronograma de amortização | ⬜ | — | — | — | — | — | — |
| 2.7 | **Estratégia de quitação** (avalanche x bola de neve) + **simulador de antecipação** | ⬜ | — | — | — | — | — | — |

---

## Fase 3 — Investimentos & carteira ideal

| # | Tarefa | Status | Resultado | Estim. | Real | Início | Fim | Obs. |
|---|---|:---:|:---:|:---:|:---:|---|---|---|
| 3.1 | **Posições** por classe (renda fixa, variável, cripto, exterior) | ⬜ | — | — | — | — | — | — |
| 3.2 | **Cotações automáticas** — ações/FIIs/ETFs B3 | ⬜ | — | — | — | — | — | — |
| 3.3 | **Cotações automáticas** — cripto e câmbio/exterior (Nomad/USD) | ⬜ | — | — | — | — | — | — |
| 3.4 | **Consolidação** da carteira (total, distribuição, rentabilidade) | ⬜ | — | — | — | — | — | — |
| 3.5 | **Perfil de risco** (questionário) | ⬜ | — | — | — | — | — | — |
| 3.6 | **Alocação-alvo** (sugerida + ajuste manual) | ⬜ | — | — | — | — | — | — |
| 3.7 | **Rebalanceamento por aporte** (quanto colocar em cada classe) | ⬜ | — | — | — | — | — | — |

---

## Fase 4 — Futuro financeiro

| # | Tarefa | Status | Resultado | Estim. | Real | Início | Fim | Obs. |
|---|---|:---:|:---:|:---:|:---:|---|---|---|
| 4.1 | **FIRE** — número mágico + idade projetada (linguagem simples) | ⬜ | — | — | — | — | — | — |
| 4.2 | **Metas** definidas pelo usuário (valor / renda-alvo) | ⬜ | — | — | — | — | — | — |
| 4.3 | **Reserva de emergência** (meses guardados + meta) | ⬜ | — | — | — | — | — | — |
| 4.4 | **Objetivos** (viagem, imóvel...) com progresso | ⬜ | — | — | — | — | — | — |
| 4.5 | **Orçamento** por categoria (opcional, com aviso ao estourar) | ⬜ | — | — | — | — | — | — |

---

## Fase 5 — Conveniência & relatórios

| # | Tarefa | Status | Resultado | Estim. | Real | Início | Fim | Obs. |
|---|---|:---:|:---:|:---:|:---:|---|---|---|
| 5.1 | **Importação CSV** (pré-visualização, mapeamento, duplicatas) | ⬜ | — | — | — | — | — | — |
| 5.2 | **Perfis de importação**: Bradesco e XP | ⬜ | — | — | — | — | — | — |
| 5.3 | **Perfis de importação**: Binance (cripto) e Nomad (USD) | ⬜ | — | — | — | — | — | — |
| 5.4 | Suporte a **OFX** | ⬜ | — | — | — | — | — | — |
| 5.5 | **Assinaturas** (cadastro manual + total mensal/anual + ranking) | ⬜ | — | — | — | — | — | — |
| 5.6 | **Relatórios visuais** com gráficos (patrimônio, fluxo, gastos, carteira) | ⬜ | — | — | — | — | — | — |
| 5.7 | **Exportar relatórios** em PDF e CSV | ⬜ | — | — | — | — | — | — |

---

## Fase 6 (futuro) — Nuvem & multiusuário em tempo real

| # | Tarefa | Status | Resultado | Estim. | Real | Início | Fim | Obs. |
|---|---|:---:|:---:|:---:|:---:|---|---|---|
| 6.1 | Backend gratuito (Supabase) — banco + autenticação | ⬜ | — | — | — | — | — | — |
| 6.2 | **Login individual** (você e cônjuge, cada um no seu aparelho) | ⬜ | — | — | — | — | — | — |
| 6.3 | **Sincronização em tempo real** entre dispositivos | ⬜ | — | — | — | — | — | — |
| 6.4 | **Migração** dos dados locais (v1) para a nuvem | ⬜ | — | — | — | — | — | — |
| 6.5 | Isolamento de dados por família + segurança (HTTPS, hash) | ⬜ | — | — | — | — | — | — |

---

## Registro de ocorrências (falhas e decisões durante o dev)

> Aqui ficará o histórico de qualquer tarefa que **falhou** (com o motivo e como foi resolvida) e decisões técnicas tomadas no caminho. Vazio por enquanto.

| Data | Tarefa | O que aconteceu | Ação tomada |
|---|---|---|---|
| — | — | — | — |
