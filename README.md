# Estudo de Web Analytics — SETDIG-MS

> **Documento de Arquitetura de Soluções**
> **Órgão:** Secretaria-Executiva de Transformação Digital — SETDIG (SEGOV/MS)
> **Versão:** 2.0 (revisão 2026 — ADR-002 supersedes ADR-001)
> **Data-base:** agosto/2026
> **Status:** Proposto (aguardando homologação do Comitê de Arquitetura)

---

## Pergunta central

**Manter Matomo, substituir por PostHog, ou operar os dois em coexistência?**

Estado usa Matomo hoje (portal + sites gov), implantado **sem estudo prévio** de alternativas. Portal institucional migrará para o novo **portal Xvia** (superapp cidadão) — cuja stack on-premise gratuita adotada é **PostHog**. Antes de bater o martelo, benchmark documental das ferramentas **on-premise gratuitas** que rodam dentro da infra do Estado.

**Escopo fechado deste ciclo:** Matomo, PostHog, Plausible, Umami. Alternativas SaaS/proprietárias arquivadas em `anexos/historico/`.

---

## Resposta

> **✅ Bloco de Decisão — ADR-002 vigente**
>
> **Parque (EDS + sites gov MS):** manter **Matomo On-Premise** como plataforma padrão única.
>
> **Portal Xvia:** arquitetura **híbrida deliberada** — **Matomo On-Premise + PostHog auto-hospedado em paralelo**:
> - Matomo → audiência agregada, campanhas, SEO, base cookieless.
> - PostHog → jornada identificada, funil transacional, feature flags, session replay consentido.
>
> **Direção estratégica de longo prazo:** coexistência de curto prazo → Matomo camada de consulta histórica → convergência em uma única ferramenta.

---

## Ranking on-premise gratuito (matriz revisada 2026)

| # | Plataforma | Pontos | % | Faixa | Papel |
|--:|-----------|-------:|--:|-------|-------|
| 🥇 **1** | **Matomo On-Premise** | **500** / 600 | **83,3 %** | 🟢 Recomendada | Padrão do parque |
| 🥈 2 | Piwik PRO ⁽ᶜ⁾ | 499 / 600 | 83,2 % | 🟢 Recomendada | Contingência se P2 falhar |
| 🥉 3 | Matomo Cloud ⁽ᶜ⁾ | 485 / 600 | 80,8 % | 🟢 Recomendada | Contingência gerenciada |
| 4 | **PostHog auto-hospedado** | 464 / 600 | 77,3 % | 🟡 Viável | **Product analytics no Xvia** |
| 5 | **Plausible CE** | 460 / 600 | 76,7 % | 🟡 Viável | Opcional em conteúdo classe C |
| 6 | **Umami** | 414 / 600 | 69,0 % | 🟠 Condicionada | Reprovado em RF-27 (funil) |

⁽ᶜ⁾ Contingência formal — ativação por novo ADR se P2 (competência STI) falhar.

Detalhamento nota-a-nota + sensibilidade em [`docs/05-matriz.md`](docs/05-matriz.md).

---

## Próximos passos

1. **Homologar ADR-002** no Comitê de Arquitetura (Onda 0 — mês 0–1).
2. **Onda 1** (mês 1–3): diagnóstico, RIPD, gate P2. **Ativa contingência via ADR-003 se P2 falhar.**
3. **Onda 2** (mês 4–9): resiliência (fila Redis, réplica, DR, IdP) + segurança.
4. **Onda 3** (mês 10–14): capacidade analítica (plugins premium, funis dos 20 serviços prioritários) + integração BI.
5. **Onda 4** (mês 15–18): escala, migração dos órgãos, autoatendimento, dados abertos.
6. **Onda 5** (mês 12–24, paralela): Portal Xvia + PostHog auto-hospedado + gate 12 meses.

**Orçamento marginal 24 meses:** ~R$ 915 k. **Recorrente pós-projeto:** ~R$ 145 k/ano. Detalhes em [`docs/08-roadmap.md`](docs/08-roadmap.md).

---

## Índice

### 📘 Núcleo do estudo

| # | Documento | Assunto |
|---|-----------|---------|
| 01 | [Contexto](docs/01-contexto.md) | Situação atual, pergunta central, stakeholders, direcionadores |
| 02 | [Requisitos](docs/02-requisitos.md) | RL, RF, RNF, RI, RG, RN + rastreabilidade + cenários ATAM |
| 03 | [Critérios](docs/03-criterios.md) | Modelo de avaliação, triagem eliminatória, pesos, definição operacional |
| 04 | [Mercado](docs/04-mercado.md) | Segmentação, plataformas no escopo, tendências, viabilidade de fornecedor |
| 05 | [Matriz de decisão](docs/05-matriz.md) | Notas nota-a-nota, ranking, sensibilidade, dominância |
| 06 | [ADR-002](docs/06-adr-002.md) | Decisão vigente formal (MADR 4.0) |
| 07 | [Recomendação](docs/07-recomendacao.md) | Aderência, arquitetura-alvo, padrão LGPD, integração BI, riscos, condicionantes |
| 08 | [Roadmap](docs/08-roadmap.md) | 5 ondas + gates + orçamento + critérios de aceite |
| 99 | [Referências](docs/99-referencias.md) | Fontes primárias, normativos, documentação oficial |

### 📗 Fichas de plataforma

- [Matomo](plataformas/matomo.md) · [PostHog](plataformas/posthog.md) · [Plausible](plataformas/plausible.md) · [Umami](plataformas/umami.md)

### 📙 Comparativos

- [Matomo × PostHog — complementaridade](comparativos/matomo-vs-posthog.md)
- [On-premise gratuito — 4 plataformas lado a lado](comparativos/on-premise-gratis.md)
- [Coexistência Matomo + PostHog no Xvia](comparativos/coexistencia-matomo-posthog.md)

### 📕 Anexos

- [Matriz completa](anexos/matriz.md) · [Benchmark](anexos/benchmark.md) · [Glossário](anexos/glossario.md)
- **Histórico** (arquivado, preservado no git): ADR-001, comparativos redundantes, fichas de plataformas fora do escopo atual (GA4, Adobe, Clarity, Cloudflare, Simple, OWA) — em `anexos/historico/`.

### 🎯 Apresentações

- [Slides Marp — estudo completo](apresentacoes/camada-complementar-posthog.md)
- [Slidão HTML standalone](apresentacoes/slidao-estudo-web-analytics.html) (paleta SETDIG, autocontido)

---

## Metodologia

| Arcabouço | Uso |
|-----------|-----|
| TOGAF 10 ADM (fases A e B) | Contexto + requisitos |
| ATAM (SEI/CMU) | Árvore de qualidade + trade-offs |
| Gartner Decision Framework | Critérios ponderados, must-have vs nice-to-have |
| MAUT | Normalização + agregação da matriz |
| ADR (Nygard / MADR 4.0) | Registro formal da decisão |

Detalhamento em [`docs/03-criterios.md §1`](docs/03-criterios.md#1-modelo).

---

## Como publicar

```bash
pip install mkdocs-material mkdocs-same-dir
mkdocs serve      # http://127.0.0.1:8000
mkdocs build --strict
```

Deploy automático → GitHub Pages via workflow `.github/workflows/gh-pages.yml`.

---

## Convenções e ciclo

Convenções editoriais (pt-BR, blocos semânticos, links relativos, tabelas, notas 1–5): [`CLAUDE.md §3`](CLAUDE.md).

**Ciclo de revisão do estudo:** 24 meses (próxima: agosto/2028), ou antecipada por gatilhos G1–G12 do [ADR-002 §11](docs/06-adr-002.md#11-revisão-futura).

**Governança:** alterações estruturais (nova plataforma, novo critério, mudança de peso) exigem aprovação do Comitê de Arquitetura da SETDIG.

---

## Nota sobre preços e dados de fornecedores

Preços de plataformas enterprise não são públicos — valores marcados como **estimativa de mercado** exigem cotação formal antes de contratação. Notas de escalabilidade e desempenho **não foram medidas** no ambiente do Estado; exigem PoC prevista na Onda 1 do [`docs/08-roadmap.md`](docs/08-roadmap.md).

Toda afirmação técnica é rastreável a fonte primária em [`docs/99-referencias.md`](docs/99-referencias.md). Se algo não puder ser verificado, é rotulado como projeção fundamentada.
