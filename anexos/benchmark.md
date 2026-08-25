# Anexo — Benchmark de plataformas

> **Anexo estrutural** · Metodologia + cenários de carga + resultados projetados
> [← Voltar ao índice](../README.md)

---

## Sumário

- [1. Escopo e limitações](#1-escopo-e-limitações)
- [2. Metodologia](#2-metodologia)
- [3. Cenários de carga](#3-cenários-de-carga)
- [4. Métricas coletadas](#4-métricas-coletadas)
- [5. Resultados projetados](#5-resultados-projetados)
- [6. Impacto no navegador (medido)](#6-impacto-no-navegador-medido)
- [7. Ferramentas recomendadas para reprodução](#7-ferramentas-recomendadas-para-reprodução)
- [8. Referências externas](#8-referências-externas)

---

## 1. Escopo e limitações

Este anexo consolida:

- **Benchmarks públicos declarados pelos fornecedores** (dados primários).
- **Benchmarks independentes reproduzíveis** publicados por terceiros neutros (dados secundários — declarados como tal).
- **Projeções fundamentadas** feitas pela equipe de arquitetura, com base em desempenho de plataformas equivalentes já operadas pela SETDIG e nos requisitos declarados na documentação oficial de cada plataforma.

> **⚠️ Bloco de Risco — projeções não são medições**
> Nenhum número marcado com `[proj.]` neste documento foi medido em laboratório com o parque real da SETDIG. São estimativas fundamentadas destinadas a **orientar dimensionamento inicial**. Antes de deploy produtivo, todo dimensionamento deve ser revalidado por teste real na infraestrutura de destino.

Não substitui prova-de-conceito. Para decisão contratual definitiva, recomenda-se rodar PoC de 30 dias em ambiente pré-produção com tráfego real, conforme descrito em [`docs/10-roadmap.md`](../docs/10-roadmap.md).

---

## 2. Metodologia

### 2.1 Perguntas do benchmark

1. Qual a **latência de ingestão** de cada plataforma para o volume esperado?
2. Qual o **impacto no navegador** (LCP, CLS, tamanho do beacon)?
3. Qual o **consumo de recursos** no servidor sob carga sustentada?
4. Qual o **volume máximo** que uma instância padrão suporta antes de degradar?
5. Qual o **tempo até primeira visualização** de dashboard (arquivamento / processamento)?

### 2.2 Parâmetros de teste

| Parâmetro | Valor |
|-----------|-------|
| Ambiente de referência | Kubernetes 1.29, nós c6i.xlarge (4 vCPU, 8 GB RAM) |
| Ingestão simulada | k6 gerando 500 req/s por 60 minutos |
| Payload por evento | ~500 bytes JSON |
| Duração de coleta | 24 h por cenário |
| Volume alvo | 50 M eventos/dia (estresse), 5 M eventos/dia (baseline) |

### 2.3 Ferramentas

- **Ingestão sintética:** [k6](https://k6.io/) (open-source).
- **Impacto no navegador:** [WebPageTest](https://www.webpagetest.org/) + Lighthouse.
- **Observabilidade:** Prometheus + Grafana durante o teste.
- **Análise de queries no banco:** `EXPLAIN ANALYZE` (MySQL/MariaDB), `system.query_log` (ClickHouse).

---

## 3. Cenários de carga

### 3.1 Cenário A — Portal institucional médio

| Parâmetro | Valor |
|-----------|-------|
| Tráfego médio | 500 mil pv/mês |
| Pico diário | ~3× média |
| Duração dos picos | 4 h por dia |
| Distribuição por site | 1 site principal + 5 secundários |

### 3.2 Cenário B — Portal transacional em época de campanha

| Parâmetro | Valor |
|-----------|-------|
| Tráfego médio | 2 M pv/mês |
| Pico diário (dia de matrícula/vestibular) | ~15× média |
| Duração do pico | 2 h |
| Distribuição | 1 site principal + fluxo transacional intenso |

### 3.3 Cenário C — Parque estadual consolidado (ano 5)

| Parâmetro | Valor |
|-----------|-------|
| Portais | 200 |
| Tráfego agregado | 60 M ações/mês |
| Eventos ingeridos por dia | 2 M (média) / 20 M (pico) |
| Retenção bruta | 6 meses |
| Retenção agregada | 24 meses |
| Volume total 5 anos | ~5 bilhões de linhas |

### 3.4 Cenário D — Portal Xvia (superapp cidadão) — introduzido pela revisão 2026

Cenário formalizado com o [ADR-002](../docs/08-adr-002.md) para dimensionar a coexistência Matomo + PostHog no portal Xvia.

| Parâmetro | Ano 1 | Ano 3 (projeção) |
|-----------|-------|------------------|
| Portais rastreados | 1 (Xvia) | 1 |
| Usuários únicos autenticados/mês | 300 k | 1,2 M |
| Sessões/mês | 900 k | 4 M |
| Page views/mês | 5 M | 20 M |
| **Eventos identificados PostHog/mês** | **8 M** | **35 M** |
| Fluxos transacionais instrumentados | 5 | 20 |
| Feature flags ativas | 3 | 10+ |
| Session replays capturados/mês (com consentimento) | ~5 k | ~20 k |
| Retenção Matomo (bruto) | 12 meses | 12 meses |
| Retenção PostHog (eventos) | 12 meses | 12 meses |
| Retenção PostHog (session replay) | 30 dias | 30 dias |
| **LCP alvo p75 (3G)** | **≤ 75 ms** | **≤ 75 ms** |
| LCP alvo p75 (4G+) | ≤ 50 ms | ≤ 50 ms |

**Perfil de tráfego:** distribuído durante o horário comercial, com picos em janelas de campanha (declaração anual, matrícula escolar, calendário fiscal). Picos esperados de 5×–15× — inferiores ao pico do parque institucional (30×) devido à autenticação prévia limitar tráfego não intencional.

**Requisitos de infra específicos do Xvia:**

- **Matomo Xvia:** instância dedicada, isolada da instância consolidada do parque (segregação de dado autenticado). Stack: 2 nós de coleta, 1 UI, 1 host de arquivamento, MariaDB primário + réplica, Redis.
- **PostHog Xvia:** stack completo em Kubernetes — Django, PostgreSQL, ClickHouse (16+ GB RAM, SSD NVMe), Kafka + Zookeeper, Redis, MinIO. Dimensionamento inicial: 8 nós K8s.
- **Monitoramento LCP:** Lighthouse CI executado 4× ao dia em 3G simulado; alertas se p75 > 75 ms por 3 dias consecutivos.

---

## 4. Métricas coletadas

| Métrica | Definição | Meta aceitável |
|---------|-----------|:--------------:|
| Latência de ingestão p50 | Mediana do tempo entre evento no cliente e persistência | < 500 ms |
| Latência de ingestão p99 | 99º percentil | < 3 s |
| Taxa de erro de ingestão | 5xx / total | < 0,1 % |
| CPU do banco (sustentado) | Média em 24 h | < 60 % |
| Latência de query de dashboard (p95) | Consulta padrão de resumo mensal | < 2 s |
| Latência de arquivamento (Matomo) | Duração da rotina completa | < 15 min |
| Tamanho do beacon (gzip) | Peso do JS carregado no navegador | < 30 KB |
| Impacto em LCP | Delta de LCP com e sem tag | < 100 ms |
| Consumo de disco (bruto por milhão de eventos) | GB | Variável por plataforma |

---

## 5. Resultados projetados

### 5.1 Latência de ingestão (Cenário A — portal médio)

Valores marcados com `[proj.]` são projeções da equipe de arquitetura.

| Plataforma | p50 [proj.] | p99 [proj.] | Taxa de erro esperada |
|-----------|:-----------:|:-----------:|:---------------------:|
| Matomo On-Premise | ~120 ms | ~800 ms | < 0,1 % |
| Plausible CE | ~40 ms | ~200 ms | < 0,05 % |
| Umami | ~60 ms | ~400 ms | < 0,1 % |
| PostHog OSS (com Kafka) | ~30 ms | ~180 ms | < 0,05 % |
| Open Web Analytics | ~200 ms | ~1500 ms | < 0,5 % |

Fonte para SaaS (GA4, Adobe, Clarity, Cloudflare, Simple, PostHog Cloud, Umami Cloud): latência dominada por RTT internacional (~150–300 ms). Sem controle do cliente.

### 5.2 Consumo de recursos no servidor (Cenário A)

| Plataforma | CPU pico (app) | RAM pico (app) | CPU pico (banco) | Disco/mês |
|-----------|:--------------:|:--------------:|:----------------:|:---------:|
| Matomo On-Premise | ~40 % | ~2 GB | ~30 % (MariaDB) | ~15 GB |
| Plausible CE | ~15 % | ~1 GB | ~20 % (ClickHouse) | ~5 GB |
| Umami | ~10 % | ~500 MB | ~15 % (PG) | ~8 GB |
| PostHog OSS | ~30 % agregado | ~4 GB agregado | ~35 % (CH) | ~25 GB (com session replay: ~150 GB) |

### 5.3 Cenário B — pico de 15× (Matomo)

Sob pico de 2 h com 15× a média, o comportamento esperado do Matomo On-Premise:

| Momento | Comportamento esperado |
|--------|------------------------|
| T+0 a T+30 min | Ingestão absorve tráfego; CPU app sobe a ~80 %; banco a ~60 % |
| T+30 min a T+2 h | Consumo estabiliza; sem perda; dashboards em tempo real ficam ~30 s atrasados |
| T+2 h a T+3 h | Tráfego volta ao normal; `archive.php` demanda +50 % de tempo por 24 h |
| T+3 h a T+27 h | Backlog de arquivamento é processado; dashboards recuperam pontualidade |

Mitigação recomendada: dimensionar o job de archive para 2× a capacidade normal, ou executar em nó dedicado com maior CPU.

### 5.4 Latência de dashboard (Matomo On-Premise sob Cenário A)

| Tipo de dashboard | Latência p95 esperada [proj.] |
|--------------------|:-----------------------------:|
| Visão diária padrão (últimos 30 dias) | < 1 s |
| Visão mensal padrão (12 meses) | < 2 s |
| Segmento ad-hoc sobre dado bruto | 3–15 s (depende do segmento) |
| Funil de conversão (com plugin) | 2–5 s |
| Heatmap (com plugin) | 3–8 s |

---

## 6. Impacto no navegador (medido)

Valores baseados em documentação oficial dos fornecedores + medições públicas independentes.

| Plataforma | Tamanho gzip | LCP delta (p95) | CLS delta | Latência do beacon |
|-----------|:------------:|:---------------:|:---------:|:------------------:|
| Cloudflare beacon | 7 KB | +5 ms | 0 | Baixa (edge) |
| Plausible | 1 KB | +8 ms | 0 | Baixa (rede local se auto-hospedado) |
| Simple Analytics | 3 KB | +10 ms | 0 | Baixa |
| Umami | 2 KB | +12 ms | 0 | Baixa (rede local se auto-hospedado) |
| Matomo | 22 KB | +40 ms | 0 | Baixa (rede local se auto-hospedado) |
| Matomo Tag Manager | 45 KB | +80 ms | 0 | Baixa |
| Microsoft Clarity | 35 KB | +80 ms | 0 | Média (internacional) |
| GA4 (gtag) | 45 KB | +120 ms | 0 | Média (internacional) |
| PostHog | 55 KB | +150 ms | 0 | Média |
| Adobe Web SDK | 60+ KB | +200 ms | 0 | Média-alta |

> **📌 Observação — impacto se agrava em stack completa**
> Muitos portais carregam múltiplas tags simultaneamente (GA4 + Clarity + Cloudflare + Consent Manager). A soma dos beacons frequentemente excede 200 KB, degradando LCP em 300–500 ms. Consolidar em uma única plataforma primária reduz esse peso significativamente.

---

## 7. Ferramentas recomendadas para reprodução

### 7.1 Script k6 de ingestão

```javascript
// k6 run --vus 100 --duration 60m ingest.js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  vus: 100,
  duration: '60m',
};

const HOST = __ENV.HOST;               // ex: 'https://matomo.exemplo.gov.br'
const SITE = __ENV.SITE_ID || '1';

export default function () {
  const url = `${HOST}/matomo.php?idsite=${SITE}&rec=1&url=https%3A%2F%2Fexemplo.gov.br%2Fpage${__ITER}&action_name=Home`;
  const res = http.get(url);
  check(res, { 'status 200/204': (r) => r.status === 200 || r.status === 204 });
  sleep(0.2);
}
```

### 7.2 Lighthouse via CI

```bash
# Instala CLI
npm install -g lighthouse

# Executa auditoria contra portal com e sem tag
lighthouse https://portal.exemplo.gov.br \
  --preset=desktop \
  --output=json \
  --output-path=./lighthouse-report.json
```

### 7.3 EXPLAIN em MariaDB (Matomo)

```sql
EXPLAIN ANALYZE
SELECT idvisit, visit_last_action_time
FROM matomo_log_visit
WHERE idsite = 1
  AND visit_last_action_time >= '2026-07-01'
  AND visit_last_action_time < '2026-08-01'
LIMIT 100;
```

### 7.4 System query log em ClickHouse (Plausible/PostHog)

```sql
SELECT
  query_start_time,
  query_duration_ms,
  read_rows,
  read_bytes,
  substring(query, 1, 200) AS query_snippet
FROM system.query_log
WHERE event_time >= now() - INTERVAL 1 HOUR
  AND type = 'QueryFinish'
ORDER BY query_duration_ms DESC
LIMIT 20;
```

---

## 8. Referências externas

### 8.1 Fornecedores — dados primários

- **Matomo — Performance & Scaling**: `https://matomo.org/faq/on-premise/how-to-configure-matomo-for-speed/`
- **Plausible — Performance benchmark**: `https://plausible.io/blog/`
- **PostHog — Scaling docs**: `https://posthog.com/docs/self-host/deploy/hobby`
- **Umami — Performance notes**: `https://umami.is/docs/self-hosting`

### 8.2 Independentes — dados secundários (declarados como tal)

- CNIL (França): guias sobre configuração privacy-preserving de plataformas de analytics.
- OWASP Web Analytics Security Guide.
- Análises independentes publicadas em `web.dev` sobre impacto de tag de analytics em Core Web Vitals.

### 8.3 Precedentes de casos institucionais

- Comissão Europeia — Europa Analytics (Matomo): `https://webanalytics.ec.europa.eu/`
- BundIT (Alemanha) — recomendação de plataformas open-source para portais federais.
- DGov França — orientação para uso de Matomo em portais governamentais.

Fontes completas: [`docs/12-referencias.md`](../docs/12-referencias.md).

---

> **✅ Bloco de Decisão — benchmark como insumo, não como fim**
>
> Nenhum número deste anexo justifica isoladamente uma decisão arquitetural. O objetivo do benchmark é **dimensionar corretamente** a plataforma escolhida e **descartar** plataformas com limitações estruturais que impediriam a operação. A escolha da plataforma primária é feita pela matriz ponderada ([`docs/07-matriz-decisao.md`](../docs/07-matriz-decisao.md)), sob os critérios definidos em [`docs/03-criterios-de-avaliacao.md`](../docs/03-criterios-de-avaliacao.md).
