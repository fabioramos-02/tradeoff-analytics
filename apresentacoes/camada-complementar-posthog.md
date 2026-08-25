---
marp: true
theme: default
paginate: true
size: 16:9
header: "Camada complementar de analytics · SETDIG"
footer: "Fonte: estudo tradeoff-analytics (2026-07) · Ver README.md"
style: |
  section { font-family: system-ui, sans-serif; color: #30302e; }
  h1 { color: #004f9f; border-bottom: 3px solid #004f9f; padding-bottom: 8px; }
  h2 { color: #004f9f; }
  strong { color: #003a76; }
  table { font-size: 0.85em; }
  th { background: #004f9f; color: white; }
  tr:nth-child(even) { background: #f0f4fa; }
  .anchor { font-size: 1.4em; font-weight: 700; color: #003a76; margin: 0.4em 0; }
  .number { font-size: 2.6em; font-weight: 800; color: #004f9f; line-height: 1; }
  .caption { color: #555; font-size: 0.9em; }
  .warn { background: #fff4e5; border-left: 6px solid #e58900; padding: 10px 16px; }
  .ok { background: #e8f4ff; border-left: 6px solid #004f9f; padding: 10px 16px; }
---

# Proposta: camada complementar de analytics

**Somar, não trocar.**
O Matomo continua padrão. Onde entra outra ferramenta?

<div class="caption">
Secretaria-Executiva de Transformação Digital — SETDIG · agosto/2026<br>
Base: estudo formal de 13 plataformas (TOGAF + ATAM + MAUT + ADR)
</div>

---

## Pergunta de decisão

<div class="anchor">"O Matomo cobre todo o parque, ou precisamos de uma segunda camada?"</div>

O estudo respondeu com **matriz ponderada de 13 alternativas**.
Esta apresentação mostra o resultado e a proposta.

---

## Resultado da matriz — top 5

| # | Ferramenta | Modalidade | Nota | % |
|---|------------|-----------|-----:|--:|
| 🥇 1 | **Matomo** | On-Premise | **520/600** | **86,7%** |
| 🥈 2 | Plausible | Auto-hospedado | 485/600 | 80,8% |
| 🥉 3 | Matomo | Cloud (UE) | 465/600 | 77,5% |
| 3 | Piwik PRO | Private Cloud | 465/600 | 77,5% |
| 5 | **PostHog** | Auto-hospedado | **455/600** | **75,8%** |

<div class="caption">Memória de cálculo: <code>docs/07-matriz-decisao.md</code></div>

---

## O que o Matomo já cobre bem

<div class="ok">

- **Cobertura ampla:** funis, heatmaps, gravação de sessão, form analytics, A/B, Tag Manager
- **Soberania de dados:** MySQL sob custódia do Estado — nenhuma transferência internacional
- **LGPD:** operação **cookieless** validada (precedente CNIL)
- **Integração com BI:** API HTTP/JSON + acesso SQL direto (Power BI, Superset, Metabase, Grafana)
- **TCO 5 anos:** **R$ 235.000** (regime marginal SETDIG)

</div>

---

## Onde o PostHog acrescenta

<div class="ok">

Ganhos funcionais sobre o Matomo em **product analytics profundo**:

- **Feature flags** com liberação gradual e A/B com significância estatística
- **Cohorts comportamentais** de longo prazo (retenção em 30/60/90 dias)
- **Session replay** com filtros avançados por evento
- **SDKs modernos** (mais linguagens oficiais que o Matomo)

Onde faz diferença: **serviços digitais transacionais complexos** que precisam iterar em jornada do cidadão.

</div>

---

## Custo real da camada complementar

<div class="number">R$ 955.000</div>
<div class="anchor">TCO 5 anos do PostHog auto-hospedado — <strong>4× o Matomo</strong></div>

| Rubrica (5 anos) | Matomo | PostHog OSS |
|------------------|-------:|------------:|
| Licenciamento | R$ 60k | **R$ 300k** (SSO+RBAC) |
| Infraestrutura marginal | R$ 30k | **R$ 300k** (Kafka+ClickHouse+MinIO fora do padrão) |
| Equipe marginal | R$ 75k (~0,1 FTE) | **R$ 240k (~0,4 FTE)** |

<div class="caption">Regime de custo marginal SETDIG · Fonte: <code>comparativos/matomo-vs-posthog.md §5</code></div>

---

## Impacto na LGPD

<div class="warn">

**PostHog OSS exige adequação adicional que o Matomo já entrega por padrão:**

- Session replay grava conteúdo de tela → **precisa DPIA** (Avaliação de Impacto)
- Sem base legal consolidada em precedente europeu (Matomo tem CNIL)
- Requer política de retenção **mais rígida** e mascaramento de PII na origem
- Custo estimado de adequação LGPD: **R$ 40k** (vs R$ 30k Matomo)

</div>

<div class="caption">Fonte: <code>comparativos/lgpd.md</code> e <code>comparativos/matomo-vs-posthog.md §6</code></div>

---

## Três caminhos possíveis

| Cenário | O que entrega | TCO 5a | Recomendação do estudo |
|---------|---------------|-------:|------------------------|
| **A. Só Matomo** | Cobertura ampla suficiente para 90% do parque | R$ 235k | Base |
| **B. Matomo + Plausible** *(ADR-001)* | Camada leve para portais de conteúdo alto volume | +R$ 40k | ✅ **Aprovado** |
| **C. Matomo + PostHog** *(esta proposta)* | Product analytics profundo para serviços transacionais complexos | +R$ 720k | ⚠️ Requer justificativa de caso de uso |

<div class="anchor">Decisão pedida ao Comitê: aprovar B, C ou pedir estudo de caso antes de C.</div>

---

## Como ler esta apresentação

- Números são **projeções em regime marginal SETDIG** (P1/P2/R4), não cotação formal
- Matriz de decisão é **auditável nota a nota** em `docs/07-matriz-decisao.md`
- ADR-001 vigente recomenda **cenário B** (Matomo + Plausible)
- Cenário C exige, antes de aprovar, **mapear os serviços que justificam feature flags + cohorts de longo prazo**

**Referências completas:** [README.md](../README.md) · [docs/09-recomendacao.md](../docs/09-recomendacao.md) · [comparativos/matomo-vs-posthog.md](../comparativos/matomo-vs-posthog.md)
