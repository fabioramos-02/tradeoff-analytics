# Comparativo — APIs, SDKs e superfície de integração

> **Corte transversal** · Foco: API pública, autenticação, exportação de dados, integração com BI
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/07-matriz-decisao.md)

---

## Sumário

- [1. Dimensões avaliadas](#1-dimensões-avaliadas)
- [2. Matriz sinóptica](#2-matriz-sinóptica)
- [3. Autenticação](#3-autenticação)
- [4. Rate limits e paginação](#4-rate-limits-e-paginação)
- [5. Versionamento](#5-versionamento)
- [6. Exportação e importação](#6-exportação-e-importação)
- [7. SDKs oficiais](#7-sdks-oficiais)
- [8. Webhooks](#8-webhooks)
- [9. Integração com BI corporativo](#9-integração-com-bi-corporativo)
- [10. Exemplo canônico — mesma consulta em todas as plataformas](#10-exemplo-canônico--mesma-consulta-em-todas-as-plataformas)
- [11. Recomendação](#11-recomendação)

---

## 1. Dimensões avaliadas

| Dimensão | O que é avaliado |
|----------|-----------------|
| Superfície de API | REST, GraphQL, Batch, gRPC |
| Autenticação | Chaves, OAuth, tokens, permissões |
| Rate limits | Cotas declaradas e comportamento sob carga |
| Versionamento | Estabilidade e política de depreciação |
| Exportação | Formatos, granularidade (bruto × agregado), fluxos massivos |
| Importação | Migração de histórico entre plataformas |
| SDKs | Linguagens oficiais suportadas |
| Webhooks | Notificações push de eventos |
| BI | Conectores nativos e caminhos alternativos |

---

## 2. Matriz sinóptica

| Plataforma | REST | GraphQL | SDKs oficiais | Webhooks | Rate limit padrão | Exportação bruta | Documentação |
|-----------|:----:|:-------:|:-------------:|:--------:|:-----------------:|:----------------:|:------------:|
| **Matomo** | 🟢 v1 | ❌ | JS, PHP, Java, Python (comunidade) | 🟠 Via plugin | Configurável (sem limite hard-coded) | 🟢 SQL direto + API | 🟢 Extensa |
| Plausible | 🟢 v1 + v2 | ❌ | JS oficial; comunidade nas demais | ❌ | 600 req/hora | 🟠 Agregado (via CSV/JSON) | 🟢 Boa |
| Umami | 🟢 v1 | ❌ | JS | ❌ | Não declarado | 🟠 Agregado | 🟠 Média |
| PostHog | 🟢 v1 | 🟠 HogQL (interno) | JS, Python, Node, Ruby, Go, PHP, iOS, Android, Flutter, RN, .NET | 🟢 | 240 req/min | 🟢 Data warehouse sink + HogQL | 🟢 Extensa |
| Adobe Analytics | 🟢 v2 | ❌ | JS, iOS, Android, Web SDK | 🟢 | 12 req/s por org | 🟢 Data Feeds + Data Warehouse | 🟢 Extensa |
| GA4 | 🟢 Data API v1beta | ❌ | JS (gtag/GTM), Firebase, Measurement Protocol | 🟠 Via GTM | 25 mil req/dia (property) | 🟠 Bruto apenas via BigQuery Export | 🟢 Extensa |
| Simple Analytics | 🟢 v1 | ❌ | JS | 🟠 Via Zapier | 60 req/min | 🟠 Agregado | 🟠 Média |
| Microsoft Clarity | 🟠 v1-beta (Data Export) | ❌ | JS (clarity-js) | ❌ | Não declarado | 🔴 Só agregado, limite de projetos com >1k sessões/dia | 🟠 Média |
| Cloudflare Web Analytics | 🟢 v4 (parte da Cloudflare API) | 🟢 Analytics API | Genéricos (via HTTP) | ❌ (dessa camada) | 1.200 req/5 min | 🟠 Só agregado | 🟢 Boa |
| Open Web Analytics | 🟠 REST parcial | ❌ | JS, PHP | ❌ | Não declarado | 🟢 SQL direto | 🔴 Escassa |

---

## 3. Autenticação

| Plataforma | Método principal | RBAC | OAuth/OIDC | SSO SAML |
|-----------|-----------------|:----:|:----------:|:--------:|
| Matomo | `token_auth` por usuário | 🟢 Papéis + permissões por site | 🟢 Via plugin oficial (LoginLdap, LoginOIDC) | 🟢 Via plugin comercial (LoginSAML) |
| Plausible | API Key (Bearer) | 🟠 Básico | 🟠 Login social apenas | ❌ CE (só Cloud Enterprise) |
| Umami | JWT Bearer | 🟠 Básico | ❌ | ❌ |
| PostHog | Project API key (client) + Personal API key (server) | 🟢 EE | 🟠 EE | 🟢 EE |
| Adobe Analytics | JWT via Adobe IMS | 🟢 Enterprise-grade | 🟢 | 🟢 |
| GA4 | OAuth 2.0 (contas Google) | 🟢 Roles do GA + IAM Google Cloud | 🟢 (nativo Google) | 🟢 (via Google Workspace) |
| Simple Analytics | API Key + User ID | 🟠 Básico | ❌ | ❌ |
| Microsoft Clarity | Bearer token de projeto | 🟠 Básico | 🟢 Via Entra ID (Azure AD) | 🟢 Via Entra ID |
| Cloudflare Web Analytics | Cloudflare API token com escopos | 🟢 Granular | 🟢 SSO via Cloudflare Access | 🟢 (Access Enterprise) |
| Open Web Analytics | Sessão baseada em cookies | 🔴 Muito limitado | ❌ | ❌ |

> **📌 Observação — SSO federado é requisito para governo**
> A SETDIG opera IdP corporativo. Plataformas sem SSO SAML/OIDC integrado ficam impraticáveis operacionalmente ou dependem de plugin/EE pago.

---

## 4. Rate limits e paginação

| Plataforma | Limite declarado | Estratégia de paginação |
|-----------|-----------------|-------------------------|
| Matomo | Configurável (padrão sem limite) | `filter_limit` + `filter_offset` |
| Plausible | 600 req/hora por token | `page` + `limit` |
| PostHog | 240 req/min por token | `next` cursor URL |
| GA4 | 25 mil req/dia + 10 QPS por property | `nextPageToken` |
| Adobe Analytics | 12 req/s por org | Cursor-based |
| Cloudflare | 1.200 req/5 min | `page` + `per_page` |
| Simple Analytics | 60 req/min | `offset` + `limit` |
| Umami | Não declarado | `startAt` + `endAt` |
| Clarity | Não declarado | Sem paginação — resposta única |
| OWA | Não declarado | Cursor-based |

> **⚠️ Bloco de Risco — GA4 Data API tem cota diária**
> O limite de 25 mil requisições/dia por propriedade é baixo para pipelines diários que rodam sobre múltiplos relatórios. É prática usar BigQuery Export para volume alto — o que introduz custo BigQuery.

---

## 5. Versionamento

| Plataforma | Versão atual | Política de depreciação declarada | Estabilidade percebida |
|-----------|:------------:|----------------------------------|:----------------------:|
| Matomo | v1 (Reporting API) | Depreciação anunciada com > 12 meses | 🟢 Muito estável |
| Plausible | Stats API v1 + v2 (coexistem) | v1 mantida por prazo aberto | 🟢 Estável |
| PostHog | v1 | Breaking changes com aviso prévio | 🟠 Cadência alta introduz mudanças |
| Adobe | v2 | Ciclo declarado (v1 depreciada) | 🟢 Estável |
| GA4 | v1beta (Data API) | Beta pode mudar; comprometimento futuro para v1 | 🟠 Beta com risco de mudança |
| Cloudflare | v4 | Muito estável | 🟢 |
| Simple Analytics | v1 | Estável | 🟢 |
| Umami | v1 | Mudou entre v1 → v2 (2023, breaking) | 🟠 |
| Clarity | v1-beta | Beta | 🟠 |
| OWA | Sem política formal | Projeto estagnado | 🔴 |

---

## 6. Exportação e importação

### 6.1 Exportação de dado bruto (evento a evento)

| Plataforma | Bruto exportável? | Caminho canônico |
|-----------|:-----------------:|------------------|
| Matomo | 🟢 | SQL direto no MariaDB/MySQL + tabela `matomo_log_visit` / `matomo_log_link_visit_action` |
| PostHog | 🟢 | Data warehouse sink (S3, BigQuery, Snowflake, Postgres) |
| GA4 | 🟢 (só via BigQuery Export) | Vinculação obrigatória de projeto GCP com BigQuery |
| Adobe | 🟢 | Data Feeds (arquivo diário) + Data Warehouse |
| Plausible | 🟠 | Bruto exportável apenas via SQL direto no ClickHouse do próprio deployment |
| Umami | 🟠 | Idem — via banco (PostgreSQL/MySQL) |
| OWA | 🟢 | SQL direto |
| Simple Analytics | 🟠 Agregado | API — sem exportação evento a evento |
| Clarity | 🔴 | Não exporta bruto |
| Cloudflare | 🔴 | Só agregado (via GraphQL Analytics) |

### 6.2 Importação de histórico (migração)

| Plataforma | Suporta import de outra plataforma? |
|-----------|:-----------------------------------:|
| Matomo | 🟠 Via `LogAnalytics` (importa arquivos de log web); não importa GA/Adobe diretamente |
| PostHog | 🟢 Batch import via API + sink reverso |
| GA4 | 🔴 Universal Analytics não migrou completamente para GA4 — recurso reduzido |
| Todas as demais | 🔴 Sem caminho canônico |

> **📌 Observação — migração é sempre custosa**
> Analytics é dado agregado ao longo do tempo. Nenhuma migração preserva 100 % do histórico entre plataformas heterogêneas. Estratégia realista: **operar plataformas em paralelo por 6 meses** durante troca, garantindo continuidade de série histórica.

---

## 7. SDKs oficiais

| Plataforma | JS | Python | Node | Ruby | Go | PHP | iOS | Android | .NET | Java |
|-----------|:--:|:------:|:----:|:----:|:--:|:---:|:---:|:-------:|:----:|:----:|
| Matomo | 🟢 | 🟠¹ | 🟠¹ | 🟠¹ | 🟠¹ | 🟢 | 🟢 | 🟢 | 🟠¹ | 🟢 |
| Plausible | 🟢 | 🟠¹ | 🟠¹ | 🟠¹ | 🟠¹ | 🟠¹ | 🟠¹ | 🟠¹ | 🟠¹ | 🟠¹ |
| Umami | 🟢 | 🟠¹ | 🟠¹ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| PostHog | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 |
| Adobe | 🟢 (Web SDK) | ❌ | ❌ | ❌ | ❌ | ❌ | 🟢 | 🟢 | ❌ | ❌ |
| GA4 | 🟢 (gtag/GTM) | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 (Firebase) | 🟢 (Firebase) | 🟢 | 🟢 |
| Simple Analytics | 🟢 | 🟠¹ | 🟠¹ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Microsoft Clarity | 🟢 | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Cloudflare WA | 🟢 (beacon) | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Open Web Analytics | 🟢 | ❌ | ❌ | ❌ | ❌ | 🟢 | ❌ | ❌ | ❌ | ❌ |

¹ **Comunidade** — não oficial. Uso pode ser feito, mas sem garantia de suporte pelo mantenedor.

---

## 8. Webhooks

| Plataforma | Webhook nativo | Uso típico |
|-----------|:--------------:|-----------|
| PostHog | 🟢 | Notificar Slack/Teams em conversão, integrar CRM |
| Adobe | 🟢 | Ativação server-side |
| GA4 | 🟠 Via GTM server-side | Idem |
| Matomo | 🟠 Via plugin `WebhookNotifications` (comunidade) | Alertas |
| Demais | ❌ | Requer ETL customizado ou Zapier |

---

## 9. Integração com BI corporativo

### 9.1 Conectores nativos publicados

| Plataforma | Power BI | Looker Studio | Grafana | Metabase | Superset | Qlik |
|-----------|:--------:|:-------------:|:-------:|:--------:|:--------:|:----:|
| **Matomo** | 🟢 Conector oficial + acesso direto ao MySQL/MariaDB | 🟢 Community Connector + REST | 🟢 Via SQL datasource | 🟢 Via SQL datasource | 🟢 Via SQL datasource | 🟢 Via ODBC MySQL |
| Plausible | 🟠 Via API REST | 🟠 Community Connector | 🟠 Via API + JSON datasource | 🟠 Via API/ClickHouse direto | 🟠 Via ClickHouse | 🟠 Via API |
| Umami | 🟠 Via API + PostgreSQL/MySQL direto | 🟠 Via community | 🟢 Via SQL | 🟢 Via SQL | 🟢 Via SQL | 🟠 |
| PostHog | 🟠 Via API + data warehouse sink | 🟢 Via BigQuery sink | 🟢 Via ClickHouse direto | 🟢 Via ClickHouse | 🟢 Via ClickHouse | 🟠 |
| Adobe | 🟢 Nativo | 🟢 Nativo | 🟠 Via API | 🟠 Via API | 🟠 Via API | 🟢 |
| GA4 | 🟢 Nativo (via BigQuery ou API) | 🟢 Nativo | 🟢 Plugin BigQuery | 🟢 Via BigQuery | 🟢 Via BigQuery | 🟢 |
| Simple Analytics | 🟠 Via API | 🟠 Custom Connector | 🟠 Via JSON datasource | 🟠 | 🟠 | 🟠 |
| Microsoft Clarity | 🔴 Sem conector, API limitada | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 |
| Cloudflare | 🟠 Via API GraphQL | 🟠 | 🟢 Via JSON datasource | 🟠 | 🟠 | 🟠 |
| Open Web Analytics | 🟠 Via SQL direto | 🟠 | 🟢 | 🟢 | 🟢 | 🟠 |

### 9.2 Caminhos preferenciais para BI institucional do Estado

O ecossistema de BI da SETDIG tem **Power BI como principal** e **Superset / Metabase** como secundários (open-source). O caminho técnico preferencial é:

1. Plataforma auto-hospedada → banco relacional/analítico → conector SQL direto do BI.
2. Plataforma SaaS → sink de warehouse (BigQuery, Snowflake, S3) → conector nativo do BI ao warehouse.

**Antipadrão:** consumo direto do BI a partir da API REST do produto de analytics (fragilidade contra rate limits e reprocessamento).

---

## 10. Exemplo canônico — mesma consulta em todas as plataformas

**Pergunta de negócio:** número diário de *page views* em `www.exemplo.gov.br` no mês corrente.

### 10.1 Matomo

```http
GET /index.php?module=API&method=Actions.getPageUrls
  &idSite=1&period=day&date=last30
  &format=JSON&token_auth=<TOKEN>
```

### 10.2 Plausible (Stats API v2)

```http
POST /api/v2/query
Authorization: Bearer <TOKEN>
Content-Type: application/json

{
  "site_id": "exemplo.gov.br",
  "metrics": ["pageviews"],
  "date_range": "30d",
  "dimensions": ["time:day"]
}
```

### 10.3 Umami

```http
GET /api/websites/{websiteId}/stats?startAt=1719792000000&endAt=1722384000000
Authorization: Bearer <TOKEN>
```

### 10.4 PostHog (HogQL)

```json
POST /api/projects/{projectId}/query/
{
  "query": {
    "kind": "HogQLQuery",
    "query": "SELECT toDate(timestamp) AS d, count() FROM events WHERE event = '$pageview' AND timestamp >= now() - INTERVAL 30 DAY GROUP BY d ORDER BY d"
  }
}
```

### 10.5 GA4 Data API

```http
POST /v1beta/properties/<PROP_ID>:runReport
{
  "dateRanges": [{ "startDate": "30daysAgo", "endDate": "today" }],
  "metrics": [{ "name": "screenPageViews" }],
  "dimensions": [{ "name": "date" }]
}
```

### 10.6 Cloudflare (GraphQL)

```graphql
query {
  viewer {
    zones(filter: { zoneTag: "<ZONE>" }) {
      httpRequests1dGroups(limit: 30, filter: { date_geq: "2026-07-01" }) {
        dimensions { date }
        sum { pageViews }
      }
    }
  }
}
```

### 10.7 Adobe Analytics 2.0

```http
POST /api/{rsid}/reports
{
  "rsid": "exemplo",
  "globalFilters": [{ "type": "dateRange", "dateRange": "2026-07-01/2026-07-31" }],
  "metricContainer": { "metrics": [{ "id": "metrics/pageviews" }] },
  "dimension": "variables/daterangeday"
}
```

### 10.8 Simple Analytics

```http
GET /v1/statistics?version=5&hostname=exemplo.gov.br&start=2026-07-01&end=2026-07-31&fields=histogram
X-Api-Key: <TOKEN>
User-Id: <UID>
```

### 10.9 Microsoft Clarity

```http
GET /export-data/api/v1-beta/project-live-insights?numOfDays=30
Authorization: Bearer <TOKEN>
```
> Retorna insights agregados de 3 dias por chamada; para 30 dias, iterar 10 chamadas. Sem consulta paramétrica por data.

### 10.10 Open Web Analytics

```http
GET /owa/api.php?owa_do=owa.getResultSet
  &owa_metrics=pageViews&owa_dimensions=day
  &owa_startDate=20260701&owa_endDate=20260731
  &owa_apiKey=<KEY>
```

---

## 11. Recomendação

> **✅ Bloco de Decisão — perfil ideal de API para o Estado**
>
> Priorizar plataformas com:
>
> 1. **Acesso direto ao dado bruto** (SQL no banco ou sink para warehouse).
> 2. **Versionamento estável** com política formal de depreciação.
> 3. **SDKs oficiais** em Python e Node.js (linguagens do parque técnico da SETDIG).
> 4. **Autenticação** compatível com IdP corporativo (SSO SAML/OIDC).
> 5. **Documentação em português ou inglês estável** e exemplos executáveis.
>
> Plataformas que **cumprem** os cinco critérios: **Matomo** (com plugin LoginOIDC), **PostHog** (EE), **Adobe** (enterprise), **GA4** (com Google Workspace).
>
> **Matomo** oferece o **melhor equilíbrio** entre superfície de API razoável, acesso direto ao banco, custo zero de licenciamento da API e maturidade da documentação — resposta consistente com a recomendação primária do ADR-001.

Detalhamento por plataforma: [`plataformas/`](../plataformas/).
Trade-offs: [`docs/06-tradeoffs.md`](../docs/06-tradeoffs.md).
