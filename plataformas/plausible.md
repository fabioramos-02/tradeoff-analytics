# Plausible Analytics

> **Ficha técnica de plataforma** · Pontuação na matriz: **485/600 (80,8 %)** — 🥈 2º lugar · 🟢 Viável
> **Situação:** ✅ **Aprovado como camada complementar** (recomendação RC-1 do ADR-001)
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/07-matriz-decisao.md)

---

## Sumário

- [1. Visão geral](#1-visão-geral)
- [2. Licenciamento](#2-licenciamento)
- [3. Hospedagem](#3-hospedagem)
- [4. Funcionalidades](#4-funcionalidades)
- [5. APIs](#5-apis)
- [6. Exemplos de integração](#6-exemplos-de-integração)
- [7. Integrações](#7-integrações)
- [8. Infraestrutura](#8-infraestrutura)
- [9. Segurança](#9-segurança)
- [10. Governança](#10-governança)
- [11. Performance](#11-performance)
- [12. Custos](#12-custos)
- [13. Pontos fortes](#13-pontos-fortes)
- [14. Pontos fracos](#14-pontos-fracos)
- [15. Quando utilizar](#15-quando-utilizar)
- [16. Quando evitar](#16-quando-evitar)
- [17. Notas do avaliador](#17-notas-do-avaliador)

---

## 1. Visão geral

### 1.1 Identificação

| Campo | Valor |
|-------|-------|
| **Nome** | Plausible Analytics |
| **Edição avaliada** | Community Edition (CE), auto-hospedada |
| **Fundadores** | Uku Täht e Marko Saric |
| **Ano de lançamento** | 2019 |
| **Mantenedor** | Plausible Insights OÜ |
| **Sede** | Estônia (União Europeia) |
| **Segmento** | Privacy-first analytics |
| **Repositório** | `https://github.com/plausible/analytics` |
| **Site oficial** | `https://plausible.io/` |

### 1.2 História

```mermaid
timeline
    title Evolução do Plausible
    2019 : Lançamento como alternativa privacy-first ao Google Analytics
         : Proposta central — analytics sem cookies e sem dado pessoal
    2020 : Migração da persistência para ClickHouse
         : Modalidade auto-hospedado disponibilizada sob AGPL v3
    2021 : Funis, metas e objetivos de conversão
    2022 : Crescimento acelerado após decisões de DPAs europeias sobre Google Analytics
    2023 : Stats API v1 estabilizada
    2024 : Renomeação de "auto-hospedado" para Community Edition
         : Diferenciação explícita entre CE e planos comerciais da nuvem
    2025 : Stats API v2 com maior expressividade de consulta
```

### 1.3 Comunidade

| Indicador | Situação |
|-----------|----------|
| Idade do projeto | 6+ anos |
| Modelo de governança | Mantenedor comercial único + contribuições da comunidade |
| Cadência de releases | Regular |
| Base instalada | Concentrada em pequenas e médias organizações, agências e projetos independentes |
| Adoção institucional pública | ⚠️ Crescente na Europa, ainda incipiente |
| Profissionais no Brasil | 🔴 Escassa |
| Idioma da interface | Inglês (traduções parciais) |
| **Nota C11 (Comunidade)** | **3/5** |

### 1.4 Modelo de negócio

```mermaid
flowchart LR
    CE["Plausible CE<br/>AGPL v3<br/>Auto-hospedado gratuito"] -.->|"diferenciação"| CLOUD["Plausible Cloud<br/>SaaS por assinatura<br/>Hospedagem na UE"]
    CLOUD --> P1["Plano Growth"]
    CLOUD --> P2["Plano Business<br/>recursos adicionais"]
    CLOUD --> P3["Plano Enterprise"]

    style CE fill:#d4edda,stroke:#28a745,stroke-width:2px
    style CLOUD fill:#fff3cd,stroke:#f59e0b
```

**Classificação:** *open core* com AGPL v3. Determinadas funcionalidades são reservadas aos planos comerciais da nuvem, criando divergência entre a CE e o produto pago.

> **⚠️ Bloco de Risco — Divergência CE × Cloud**
> O mantenedor tem incentivo econômico para manter funcionalidades avançadas fora da Community Edition. Esse é o risco **TP-08** identificado em [`../docs/03-criterios-de-avaliacao.md`](../docs/03-criterios-de-avaliacao.md#31-pontos-de-sensibilidade-e-trade-off-points).
>
> **Fator atenuante:** a **AGPL v3** é copyleft de rede — impede que o mantenedor ofereça o software como serviço sem disponibilizar o código correspondente. É a proteção mais forte disponível entre as licenças livres para esse cenário, e é o motivo da nota **4** (e não 3) em C04.

### 1.5 Casos de uso

| Caso de uso | Aderência |
|-------------|:---------:|
| Portal de conteúdo / notícias | 🟢 Excelente |
| Blog institucional | 🟢 Excelente |
| Site de campanha ou hotsite | 🟢 Excelente |
| Portal institucional público (métricas agregadas) | 🟢 Excelente |
| Documentação técnica | 🟢 Excelente |
| Serviço digital transacional com análise de jornada | 🟠 Limitada |
| Análise comportamental (heatmap, replay) | 🔴 Inadequada |
| Analytics de produto (coortes, retenção) | 🔴 Inadequada |

---

## 2. Licenciamento

| Campo | Valor |
|-------|-------|
| **Tipo** | 🟢 Open Source |
| **Licença** | **GNU AGPL v3** |
| **Texto** | `https://www.gnu.org/licenses/agpl-3.0.html` |
| **Copyleft** | **Forte, com cláusula de rede** |
| **Uso comercial** | ✅ Permitido |
| **Modificação** | ✅ Permitida |
| **Redistribuição** | ✅ Sob a mesma licença |
| **Uso como serviço** | ⚠️ Exige disponibilizar o código-fonte modificado aos usuários do serviço |
| **Direito perpétuo** | ✅ Sim |
| **Custo — Community Edition** | **R$ 0** |
| **Custo — Cloud** | Assinatura escalonada por page views; preço público em `https://plausible.io/#pricing` |

> **📌 Observação — A cláusula de rede da AGPL**
> A AGPL v3 estende o copyleft ao uso em rede: quem oferece o software modificado **como serviço** deve disponibilizar o código correspondente aos usuários. Isso é relevante para o Estado em dois sentidos:
>
> 1. **Proteção:** impede que um terceiro tome o código, o melhore e ofereça como SaaS proprietário, empobrecendo a versão livre.
> 2. **Obrigação:** se o Estado modificar o Plausible e o disponibilizar como serviço a terceiros (por exemplo, a municípios), deverá publicar suas modificações. **Uso interno na Administração estadual não aciona a cláusula.**

---

## 3. Hospedagem

| Modalidade | Suporte |
|-----------|:-------:|
| Auto-hospedado (Community Edition) | ✅ Nativo |
| Docker | ✅ Imagem oficial |
| Docker Compose | ✅ Documentado oficialmente |
| Kubernetes | ⚠️ Comunidade |
| SaaS (Plausible Cloud) | ✅ Hospedagem na União Europeia |
| On-premises com suporte | ❌ |
| Região de dados selecionável (Cloud) | ⚠️ UE por padrão |

### 3.1 Requisitos

| Componente | Requisito |
|-----------|-----------|
| Runtime | Elixir / Erlang BEAM (abstraído pelo container) |
| **PostgreSQL** | 13+ — metadados, usuários, sites, configuração |
| **ClickHouse** | 23+ — eventos e sessões |
| SMTP | Necessário para convites e relatórios |
| Memória | ≥ 4 GB (aplicação) + ≥ 8 GB (ClickHouse) |

### 3.2 Implantação com Docker Compose

```yaml
# docker-compose.yml — Plausible Community Edition
services:
  plausible_db:
    image: postgres:16-alpine
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/pg_password
      POSTGRES_DB: plausible_db
    volumes:
      - pg_data:/var/lib/postgresql/data
    secrets: [pg_password]
    restart: unless-stopped

  plausible_events_db:
    image: clickhouse/clickhouse-server:24-alpine
    volumes:
      - ch_data:/var/lib/clickhouse
      - ./clickhouse-config.xml:/etc/clickhouse-server/config.d/logging.xml:ro
      - ./clickhouse-user-config.xml:/etc/clickhouse-server/users.d/logging.xml:ro
    ulimits:
      nofile: { soft: 262144, hard: 262144 }
    restart: unless-stopped

  plausible:
    image: ghcr.io/plausible/community-edition:v3
    depends_on: [plausible_db, plausible_events_db]
    command: >
      sh -c "/entrypoint.sh db createdb &&
             /entrypoint.sh db migrate &&
             /entrypoint.sh run"
    environment:
      BASE_URL: https://analytics-leve.ms.gov.br
      DATABASE_URL: postgres://postgres:${PG_PASSWORD}@plausible_db:5432/plausible_db
      CLICKHOUSE_DATABASE_URL: http://plausible_events_db:8123/plausible_events_db
      SECRET_KEY_BASE_FILE: /run/secrets/secret_key_base
      TOTP_VAULT_KEY_FILE: /run/secrets/totp_vault_key
      DISABLE_REGISTRATION: "true"
      MAILER_ADAPTER: Bamboo.SMTPAdapter
      SMTP_HOST_ADDR: smtp.interno.ms.gov.br
      SMTP_HOST_PORT: "587"
    ports:
      - "127.0.0.1:8000:8000"
    secrets: [secret_key_base, totp_vault_key]
    restart: unless-stopped

volumes:
  pg_data:
  ch_data:

secrets:
  secret_key_base:
    file: ./secrets/secret_key_base.txt
  totp_vault_key:
    file: ./secrets/totp_vault_key.txt
  pg_password:
    file: ./secrets/pg_password.txt
```

> **📌 Observação — Configuração obrigatória do ClickHouse**
> A configuração padrão do ClickHouse grava logs internos volumosos que consomem disco rapidamente. A documentação oficial do Plausible fornece os arquivos `clickhouse-config.xml` e `clickhouse-user-config.xml` que desabilitam esse comportamento. **Omiti-los é o erro de implantação mais comum**, e produz crescimento de disco desproporcional ao volume real de eventos.

---

## 4. Funcionalidades

| Funcionalidade | Suporte | Detalhe |
|---------------|:-------:|---------|
| **Dashboards** | ⚠️ Fixo | Painel único, não configurável; filtros aplicáveis |
| **Eventos customizados** | ✅ | Eventos nomeados com propriedades |
| **Goals (metas)** | ✅ | Por URL, por evento customizado; com valor de receita |
| **Conversion Funnel** | ✅ | Funis multi-etapa |
| **Heatmaps** | ❌ | Não disponível |
| **Session Recording** | ❌ | Não disponível |
| **User Journey** | ❌ | Sem fluxo de navegação agregado |
| **A/B Testing** | ❌ | Não disponível |
| **Form Analytics** | ❌ | Não disponível |
| **Real Time** | ✅ | Visitantes ativos nos últimos 30 min |
| **Cohort** | ❌ | Não disponível |
| **Retention** | ❌ | Não disponível |
| **Custom Dimensions** | ⚠️ | Propriedades customizadas de evento; recurso avançado gated no Cloud |
| **Segmentação** | ⚠️ | Filtros aplicáveis ao painel; sem segmentos compostos salvos comparáveis |
| **Comparação de períodos** | ✅ | |
| **Comparação de segmentos** | ❌ | |
| **Campanhas / UTM** | ✅ | |
| **Geolocalização** | ✅ | País, região, cidade |
| **Tecnologia** | ✅ | Navegador, SO, dispositivo, tamanho de tela |
| **Downloads e links externos** | ✅ | Extensão de script `file-downloads`, `outbound-links` |
| **Busca interna** | ⚠️ | Via evento customizado |
| **E-commerce** | 💲 | Rastreamento de receita nos planos comerciais |
| **Relatórios agendados** | ✅ | E-mail semanal e mensal |
| **Alertas** | ❌ | Não disponível |
| **Tag Manager** | ❌ | Não disponível |
| **Roll-up multi-site** | ❌ | Sem visão consolidada |
| **Painel público** | ✅ | Compartilhamento de painel por URL pública — útil para transparência ativa |
| **Amostragem** | ❌ | Dado sempre completo |

### 4.1 Cobertura dos requisitos funcionais

| Prioridade | Atendidos | Cobertura |
|-----------|:---------:|----------:|
| *Must have* | 13 / 17 | 76 % |
| *Should have* | 10 / 18 | 56 % |
| *Could have* | 3 / 12 | 25 % |
| **Ponderada** | **60 / 99** | **61 %** |

**Nota C05 (Recursos analíticos): 3/5** — faixa 55–74 %.

> **🚨 Alerta — Falha em requisitos obrigatórios**
> O Plausible **não atende** aos requisitos *Must have* **RF-36** (jornada do usuário) e **RNF-31** (MFA nativo), e atende apenas parcialmente ao **RF-28** (segmentação avançada). Isso o **desqualifica como plataforma padrão**, mas não como camada complementar para portais de conteúdo, onde esses requisitos não se aplicam.

### 4.2 Painel público — recurso relevante para o setor público

O Plausible permite tornar o painel de uma propriedade **publicamente acessível por URL**, sem autenticação.

| Aplicação | Valor |
|-----------|-------|
| Transparência ativa (Lei 12.527/2011) | Publicar estatísticas de audiência de portais públicos sem esforço |
| Prestação de contas | Números verificáveis por qualquer cidadão |
| Requisito RN-06 e RF-14 | Atendidos diretamente, sem desenvolvimento |

> **📌 Observação**
> Este é o recurso em que o Plausible supera o Matomo para o caso de uso público. O Matomo permite compartilhar widgets e criar usuários "view", mas o painel público de um clique do Plausible é operacionalmente mais simples para transparência ativa.

---

## 5. APIs

| API | Tipo | Finalidade |
|-----|------|-----------|
| **Stats API** | REST | Leitura de métricas (v1 e v2) |
| **Events API** | HTTP POST | Ingestão server-side |
| **Sites API** | REST | Gestão de propriedades (uso limitado na CE) |
| **GraphQL** | ❌ | — |
| **Webhooks** | ❌ | — |
| **Acesso SQL** | ✅ | Direto no ClickHouse (auto-hospedado) |

### 5.1 Stats API

| Característica | Valor |
|---------------|-------|
| **Endpoint v1** | `https://<host>/api/v1/stats/{aggregate,timeseries,breakdown}` |
| **Endpoint v2** | `https://<host>/api/v2/query` |
| **Autenticação** | `Authorization: Bearer <api_key>` |
| **Versionamento** | ✅ Explícito |
| **Formato** | JSON |
| **Rate limits** | ✅ Documentados (600 req/h por chave, por padrão) |
| **Filtros** | Sintaxe própria: `event:page==/servicos;visit:country==BR` |

#### Endpoints v1

| Endpoint | Retorno |
|----------|---------|
| `/api/v1/stats/realtime/visitors` | Visitantes ativos agora |
| `/api/v1/stats/aggregate` | Métricas agregadas do período |
| `/api/v1/stats/timeseries` | Série temporal |
| `/api/v1/stats/breakdown` | Detalhamento por dimensão |

### 5.2 Events API

```
POST https://<host>/api/event
Content-Type: application/json
User-Agent: <obrigatório — identifica o cliente>
X-Forwarded-For: <IP do visitante>

{
  "name": "pageview",
  "url": "https://portal.ms.gov.br/servicos",
  "domain": "portal.ms.gov.br",
  "referrer": "https://google.com",
  "props": { "orgao": "SETDIG", "tipo": "servico" }
}
```

### 5.3 Acesso direto ao ClickHouse

```sql
-- Tabelas principais do Plausible no ClickHouse
-- events_v2   : um registro por evento
-- sessions_v2 : um registro por sessão (atualizado por ReplacingMergeTree)

SELECT
    toDate(timestamp)                          AS data,
    domain                                     AS portal,
    countIf(name = 'pageview')                 AS pageviews,
    uniq(user_id)                              AS visitantes,
    uniq(session_id)                           AS sessoes
FROM events_v2
WHERE timestamp >= now() - INTERVAL 30 DAY
GROUP BY data, portal
ORDER BY data DESC;
```

**Nota C06 (APIs): 3/5** — API bem documentada e versionada, com acesso SQL direto. Rebaixada pela ausência de SDK oficial, de webhooks e pela cobertura limitada de administração na CE.

---

## 6. Exemplos de integração

### 6.1 Python

```python
"""Cliente da Stats API do Plausible."""
import os
import requests


class PlausibleClient:
    def __init__(self, base_url: str, api_key: str) -> None:
        self.base = base_url.rstrip("/")
        self.session = requests.Session()
        self.session.headers["Authorization"] = f"Bearer {api_key}"

    def aggregate(self, site_id: str, period: str = "30d",
                  metrics: str = "visitors,pageviews,bounce_rate,visit_duration",
                  filters: str | None = None) -> dict:
        params = {"site_id": site_id, "period": period, "metrics": metrics}
        if filters:
            params["filters"] = filters
        r = self.session.get(f"{self.base}/api/v1/stats/aggregate",
                             params=params, timeout=30)
        r.raise_for_status()
        return r.json()["results"]

    def timeseries(self, site_id: str, period: str = "30d",
                   interval: str = "date") -> list[dict]:
        params = {"site_id": site_id, "period": period, "interval": interval,
                  "metrics": "visitors,pageviews"}
        r = self.session.get(f"{self.base}/api/v1/stats/timeseries",
                             params=params, timeout=30)
        r.raise_for_status()
        return r.json()["results"]

    def breakdown(self, site_id: str, property_: str, period: str = "30d",
                  limit: int = 100) -> list[dict]:
        params = {"site_id": site_id, "property": property_, "period": period,
                  "metrics": "visitors,pageviews", "limit": limit}
        r = self.session.get(f"{self.base}/api/v1/stats/breakdown",
                             params=params, timeout=30)
        r.raise_for_status()
        return r.json()["results"]


if __name__ == "__main__":
    cli = PlausibleClient(os.environ["PLAUSIBLE_URL"], os.environ["PLAUSIBLE_KEY"])

    resumo = cli.aggregate("portal.ms.gov.br")
    print(f"Visitantes (30d): {resumo['visitors']['value']:,}")
    print(f"Pageviews  (30d): {resumo['pageviews']['value']:,}")

    # Apenas acessos do Brasil por dispositivo móvel
    br_movel = cli.aggregate(
        "portal.ms.gov.br",
        filters="visit:country==BR;visit:device==Mobile",
    )
    print(f"Visitantes móveis do BR: {br_movel['visitors']['value']:,}")

    for pagina in cli.breakdown("portal.ms.gov.br", "event:page", limit=10):
        print(f"{pagina['pageviews']:>8,}  {pagina['page']}")
```

### 6.2 Node.js

```javascript
/** Cliente da Stats API do Plausible — Node.js 18+ */
class PlausibleClient {
  constructor(baseUrl, apiKey) {
    this.base = baseUrl.replace(/\/$/, '');
    this.headers = { Authorization: `Bearer ${apiKey}` };
  }

  async #get(path, params) {
    const url = new URL(`${this.base}${path}`);
    Object.entries(params).forEach(([k, v]) => url.searchParams.set(k, v));
    const res = await fetch(url, { headers: this.headers });
    if (!res.ok) throw new Error(`Plausible ${res.status}: ${await res.text()}`);
    return (await res.json()).results;
  }

  aggregate(siteId, period = '30d',
            metrics = 'visitors,pageviews,bounce_rate,visit_duration') {
    return this.#get('/api/v1/stats/aggregate', { site_id: siteId, period, metrics });
  }

  breakdown(siteId, property, period = '30d', limit = 100) {
    return this.#get('/api/v1/stats/breakdown', {
      site_id: siteId, property, period, limit, metrics: 'visitors,pageviews',
    });
  }
}

const cli = new PlausibleClient(process.env.PLAUSIBLE_URL, process.env.PLAUSIBLE_KEY);
console.log(await cli.aggregate('portal.ms.gov.br'));
```

#### Envio server-side

```javascript
async function enviarEvento({ nome, url, dominio, props = {}, ip, userAgent }) {
  await fetch(`${process.env.PLAUSIBLE_URL}/api/event`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'User-Agent': userAgent,          // obrigatório
      'X-Forwarded-For': ip,            // IP real do visitante
    },
    body: JSON.stringify({ name: nome, url, domain: dominio, props }),
  });
}
```

### 6.3 Power BI

| Caminho | Avaliação |
|---------|-----------|
| Conector ClickHouse (ODBC) | ✅ Preferencial no auto-hospedado |
| Power Query M sobre a Stats API | 🔧 Viável |
| Conector nativo | ❌ Não existe |

```powerquery
// Power Query M — Stats API do Plausible
let
    BaseUrl = "https://analytics-leve.ms.gov.br",
    ApiKey  = "<chave>",   // usar Parâmetro do Power BI

    Fonte = Json.Document(Web.Contents(BaseUrl, [
        RelativePath = "api/v1/stats/timeseries",
        Query = [
            site_id  = "portal.ms.gov.br",
            period   = "12mo",
            interval = "date",
            metrics  = "visitors,pageviews,bounce_rate"
        ],
        Headers = [Authorization = "Bearer " & ApiKey]
    ])),

    Resultados = Fonte[results],
    Tabela     = Table.FromList(Resultados, Splitter.SplitByNothing()),
    Expandido  = Table.ExpandRecordColumn(Tabela, "Column1",
                    {"date", "visitors", "pageviews", "bounce_rate"},
                    {"data", "visitantes", "pageviews", "taxa_rejeicao"}),
    Tipado     = Table.TransformColumnTypes(Expandido, {
                    {"data", type date}, {"visitantes", Int64.Type},
                    {"pageviews", Int64.Type}, {"taxa_rejeicao", type number}})
in
    Tipado
```

### 6.4 Grafana

✅ **Caminho de melhor qualidade entre todas as plataformas do estudo** — o datasource oficial de ClickHouse do Grafana conecta diretamente à base de eventos.

```
Grafana → Connections → Add data source → ClickHouse
Server: clickhouse.interno.ms.gov.br  ·  Port: 9000 (nativo) ou 8123 (HTTP)
Database: plausible_events_db
```

```sql
-- Painel: visitantes únicos por hora
SELECT
    toStartOfHour(timestamp)            AS time,
    domain                              AS metric,
    uniq(user_id)                       AS value
FROM events_v2
WHERE $__timeFilter(timestamp)
GROUP BY time, metric
ORDER BY time;
```

```sql
-- Painel: top páginas do período
SELECT
    pathname                            AS pagina,
    countIf(name = 'pageview')          AS pageviews,
    uniq(user_id)                       AS visitantes
FROM events_v2
WHERE $__timeFilter(timestamp) AND domain = '$portal'
GROUP BY pagina
ORDER BY pageviews DESC
LIMIT 20;
```

### 6.5 Metabase

```
Admin → Databases → Add database → ClickHouse
(driver de ClickHouse disponível para Metabase)
Host: clickhouse.interno.ms.gov.br  ·  Port: 8123
Database: plausible_events_db
```

### 6.6 Apache Superset

```python
# Superset — string de conexão ao ClickHouse do Plausible
# clickhousedb://usuario:senha@clickhouse.interno.ms.gov.br:8123/plausible_events_db
```

```sql
-- Dataset virtual: métricas consolidadas por portal
SELECT
    toDate(timestamp)                              AS data,
    domain                                         AS portal,
    countIf(name = 'pageview')                     AS pageviews,
    uniq(user_id)                                  AS visitantes,
    uniq(session_id)                               AS sessoes,
    countIf(name != 'pageview')                    AS eventos_customizados
FROM events_v2
WHERE timestamp >= now() - INTERVAL 12 MONTH
GROUP BY data, portal;
```

### 6.7 Qlik Sense

```qlik
// Conexão ODBC ao ClickHouse
LIB CONNECT TO 'PlausibleClickHouse';

Metricas:
LOAD Data, Portal, Pageviews, Visitantes;
SQL SELECT
    toDate(timestamp) AS Data,
    domain            AS Portal,
    countIf(name='pageview') AS Pageviews,
    uniq(user_id)     AS Visitantes
FROM events_v2
WHERE timestamp >= now() - INTERVAL 24 MONTH
GROUP BY Data, Portal;
```

### 6.8 Google Looker Studio

| Caminho | Avaliação |
|---------|-----------|
| Conector da comunidade | ⚠️ Disponibilidade e manutenção variáveis |
| Google Sheets como intermediário | 🔧 Script consome a Stats API e escreve na planilha |
| **Painel público do Plausible** | ✅ **Alternativa nativa e superior** — dispensa o Looker Studio para publicação |

> **📌 Observação**
> Para publicação de estatísticas ao cidadão, o **painel público nativo do Plausible** é preferível ao Looker Studio: não transfere dados a terceiro, não requer conta Google e é ativado com um clique.

---

## 7. Integrações

| Integração | Suporte | Detalhe |
|-----------|:-------:|---------|
| **Google Tag Manager** | ✅ | Tag customizada de HTML |
| **Matomo Tag Manager** | ✅ | Tag customizada |
| **Consent Manager** | ➖ **Dispensável** | Não coleta dado pessoal — não há consentimento a gerenciar |
| **IAB TCF** | ➖ | Não aplicável |
| **Identity Provider** | ❌ | Sem SSO na Community Edition |
| **OAuth 2.0** | ❌ | Apenas token de API |
| **OpenID Connect** | ❌ | |
| **SAML 2.0** | ❌ | |
| **MFA** | ⚠️ | Disponível em versões recentes via TOTP; requer `TOTP_VAULT_KEY` configurado |
| **WordPress** | ✅ | Plugin oficial |
| **Drupal, Ghost, Hugo, Docusaurus** | ✅ | Integrações documentadas |
| **Slack** | ✅ | Notificações |
| **Webhooks** | ❌ | |

> **⚠️ Bloco de Risco — Ausência de SSO**
> A Community Edition não oferece federação de identidade (SAML/OIDC), falhando no requisito RI-15. Para uso institucional, a mitigação é colocar a aplicação atrás de um **proxy autenticador** (por exemplo, `oauth2-proxy`), delegando a autenticação ao provedor de identidade do Estado.
>
> Isso funciona, mas adiciona um componente à arquitetura e não integra com o modelo de permissões interno do Plausible — todos os usuários autenticados pelo proxy veriam a mesma interface. É uma limitação real para uso multi-órgão.

**Nota C07 (Integrações): 3/5**

---

## 8. Infraestrutura

| Aspecto | Detalhe |
|---------|---------|
| **Banco de metadados** | PostgreSQL 13+ |
| **Banco de eventos** | **ClickHouse** (colunar) |
| **Fila / streaming** | ❌ Ingestão direta, sem fila |
| **Cache** | Interno (ETS/BEAM) |
| **Componentes mínimos** | 3 (aplicação + PostgreSQL + ClickHouse) |
| **Alta disponibilidade** | ⚠️ Parcial — aplicação escalável; ClickHouse em cluster é possível mas não documentado pelo projeto |
| **Cluster** | ✅ ClickHouse suporta; requer engenharia própria |
| **Backup** | ⚠️ **Dois bancos distintos** — dobra a superfície de backup |
| **Disaster Recovery** | 🔧 Construir |

### 8.1 Backup dos dois bancos

```bash
#!/usr/bin/env bash
# backup-plausible.sh
set -euo pipefail
DATA=$(date +%Y%m%d_%H%M%S)
DESTINO=/backup/plausible

# 1. PostgreSQL — metadados, usuários, sites, metas
pg_dump -h plausible_db -U postgres -Fc plausible_db \
  > "${DESTINO}/pg_${DATA}.dump"

# 2. ClickHouse — eventos e sessões
clickhouse-client --host clickhouse.interno --query \
  "BACKUP DATABASE plausible_events_db TO Disk('backups', 'ch_${DATA}.zip')"

# 3. Verificação
pg_restore --list "${DESTINO}/pg_${DATA}.dump" > /dev/null \
  || { echo "ERRO: dump PostgreSQL inválido"; exit 1; }

echo "Backup concluído: ${DATA}"
```

> **⚠️ Bloco de Risco — Consistência entre os dois bancos**
> Backups de PostgreSQL e ClickHouse tomados em momentos distintos podem ficar inconsistentes: um site criado entre os dois momentos existiria no PostgreSQL sem eventos correspondentes, ou o inverso.
>
> **Mitigação:** executar os dois backups em janela curta e aceitar a inconsistência residual (que é tolerável — o pior caso é um site sem eventos recentes). Alternativamente, pausar a ingestão durante a janela de backup, o que não é recomendado em produção.
>
> Este é um custo operacional real da arquitetura de dois bancos, e é parte do fundamento da nota **3** em C10 (Operação).

---

## 9. Segurança

| Aspecto | Situação |
|---------|----------|
| **LGPD** | ✅ **Excelente** — não coleta dado pessoal por design |
| **GDPR** | ✅ Conformidade central ao produto |
| **ISO 27001** | ⚠️ Não aplicável ao auto-hospedado |
| **SOC 2** | ❌ |
| **Controle de acesso** | ⚠️ Básico — papéis de proprietário, administrador e visualizador |
| **MFA** | ⚠️ TOTP disponível; exige configuração de `TOTP_VAULT_KEY` |
| **Auditoria** | ❌ Sem log de auditoria administrativo |
| **Logs** | ⚠️ Log de aplicação; sem trilha de auditoria estruturada |
| **Criptografia em trânsito** | ✅ TLS (no proxy) |
| **Criptografia em repouso** | 🔧 Nível de infraestrutura |
| **Retenção de dados** | ✅ Configurável; controle total no auto-hospedado |
| **Superfície de ataque** | 🟢 Reduzida — escopo funcional mínimo |
| **Nota C09 (Segurança)** | **4/5** |

### 9.1 O que o Plausible coleta

| Dado | Coletado? | Observação |
|------|:---------:|------------|
| Endereço IP | ⚠️ Processado, **não armazenado** | Usado apenas para gerar hash de sessão e geolocalizar |
| Cookies | ❌ Nenhum | |
| Identificador persistente | ❌ Nenhum | |
| Hash de sessão | ✅ | Gerado a partir de IP + User-Agent + sal diário rotativo |
| URL da página | ✅ | |
| Referenciador | ✅ | |
| País, região, cidade | ✅ | Derivados do IP, que não é armazenado |
| Navegador, SO, dispositivo | ✅ | |
| Propriedades de evento | ✅ | Definidas pelo implementador |

> **✅ Bloco de Decisão — Por que a nota C01 é 5**
> O sal usado para gerar o identificador de sessão é **rotacionado diariamente**. Isso significa que:
>
> 1. O mesmo visitante recebe identificadores diferentes em dias diferentes — não há identificação persistente;
> 2. Não é possível reconstruir o IP original a partir do hash;
> 3. O dado resultante é **anonimizado** nos termos do art. 12 da LGPD — a lei não incide sobre ele.
>
> **Consequência:** dispensa consentimento, dispensa CMP, dispensa RIPD específico, dispensa procedimento de atendimento a direitos do titular (não há titular identificável).
>
> **Custo:** é impossível medir visitante recorrente entre dias, e a análise cross-sessão é inviável. É o trade-off TP-02 levado ao extremo favorável à conformidade.

---

## 10. Governança

| Aspecto | Avaliação |
|---------|:---------:|
| **Vendor lock-in** | 🟢 Muito baixo |
| **Controle dos dados** | 🟢 Total (auto-hospedado) |
| **Portabilidade** | 🟢 Total — SQL direto no ClickHouse |
| **Transparência** | 🟢 Código aberto; política de dados publicada |
| **Auditoria** | 🟢 Verificável — base de código pequena e legível |
| **Soberania** | 🟢 Total (auto-hospedado) |
| **Continuidade sem o fornecedor** | 🟢 Indefinida (AGPL v3) |
| **Custo de saída** | 🟢 ~R$ 20 k |
| **Nota C02 (Controle)** | **5/5** |
| **Nota C04 (Independência)** | **4/5** |

---

## 11. Performance

| Métrica | Valor |
|---------|-------|
| **Volume suportado** | ~500 M eventos/mês (ClickHouse escala muito além) |
| **Escalabilidade horizontal (coleta)** | ✅ |
| **Escalabilidade horizontal (banco)** | ✅ ClickHouse em cluster |
| **Escalabilidade vertical** | ✅ |
| **Ingestão com fila** | ❌ Direta — limita a absorção de picos instantâneos |
| **Latência do endpoint** | 🟢 < 50 ms |
| **Latência de relatório** | 🟢 Sub-segundo — consulta direta em base colunar |
| **Processo de arquivamento** | ➖ **Inexistente** — vantagem estrutural sobre o Matomo |
| **Tamanho do script (gzip)** | 🟢 **< 1 KB** |
| **Impacto no LCP** | 🟢 Praticamente nulo |
| **Amostragem** | ❌ Nenhuma |
| **Nota C08 (Escalabilidade)** | **4/5** |

> **📌 Observação — Script de menos de 1 KB**
> O tracker do Plausible é aproximadamente **50× menor** que o do Matomo e **50× menor** que o do GA4. Para portais públicos acessados majoritariamente por dispositivos móveis em conexões limitadas — perfil relevante no interior de MS — isso é um ganho concreto de acessibilidade digital, não apenas uma métrica técnica.
>
> **Contraponto honesto:** o tamanho reduzido é consequência direta do escopo funcional reduzido. Não há almoço grátis.

---

## 12. Custos

Cenário de referência: 12 M page views/mês, 80 propriedades, horizonte de 5 anos, **regime de custo marginal SETDIG** (fundamento em [`../docs/03-criterios-de-avaliacao.md §5.C03`](../docs/03-criterios-de-avaliacao.md#c03--tco-peso-15) e [`../comparativos/custo.md §1.4`](../comparativos/custo.md#14-regime-de-custo--marginal-para-o-estado)).

### 12.1 TCO consolidado — coluna dupla

| Rubrica | Custo marginal SETDIG (5 anos) | Custo pleno de referência (greenfield, 5 anos) |
|---------|-------------------------------:|----------------------------------------------:|
| **Licenciamento (Community Edition)** | R$ 0 | R$ 0 |
| **Infra marginal** — 1 VM + ClickHouse compartilhado no parque | R$ 20.000 | R$ 180.000 |
| **Equipe marginal** — ~0,05 FTE incremental | R$ 60.000 | R$ 150.000 |
| **Implantação (snippet ~200 portais)** — ~0,5 h/portal | R$ 10.000 | R$ 50.000 |
| **Adequação LGPD** — baixa, cookieless por design | R$ 15.000 | R$ 15.000 |
| **Riscos operacionais** — upgrades, tuning ClickHouse | R$ 15.000 | R$ 20.000 |
| **TCO 5 anos** | **≈ R$ 120.000** | **≈ R$ 415.000** |
| **Faixa de nota C03** | ≤ 250k = **5** | 250k–600k = 4 |

### 12.2 Racional das rubricas absorvidas

- **Infra marginal** — ClickHouse já pode ser compartilhado no parque de dados do Estado (existe em outros projetos SETDIG); PostgreSQL de metadados é módulo trivial no DBaaS existente. Marginal ≈ R$ 4k/ano.
- **Equipe marginal** — dos 0,15 FTE nominais (regra C10), parcela incremental é ~0,05 FTE. Operação de banco é absorvida pelo contrato; incremento cobre monitoração da aplicação Elixir e upgrades.
- **Implantação por portal** — mesmo padrão do Matomo: uma tag `<script defer data-domain="…" src="…/js/script.js">` em cada portal WordPress.

**Plausible Cloud (referência SaaS, não absorvível):** assinatura escalonada por page views mensais em `https://plausible.io/#pricing`. Para 12 M page views/mês em regime marginal, situa-se em faixa de plano elevada (~R$ 345k em 5 anos — ver [`../comparativos/custo.md §3.1`](../comparativos/custo.md#31-tco-consolidado--5-anos-portal-médio-regime-marginal)).

---

## 13. Pontos fortes

| # | Ponto forte |
|---|------------|
| 1 | **Conformidade por design** — não coleta dado pessoal; dispensa consentimento e CMP |
| 2 | **Script de menos de 1 KB** — impacto praticamente nulo no desempenho do portal |
| 3 | **ClickHouse** — desempenho analítico superior; sem processo de arquivamento |
| 4 | **Simplicidade radical de uso** — curva de aprendizado de minutos |
| 5 | **AGPL v3** — copyleft de rede, a proteção mais forte entre as licenças livres |
| 6 | **Painel público nativo** — transparência ativa com um clique |
| 7 | **Soberania total** na modalidade Community Edition |
| 8 | **Integração de excelência com Grafana, Superset e Metabase** via ClickHouse |
| 9 | **Sem amostragem** |
| 10 | **Documentação clara e enxuta**, incluindo self-hosting |

---

## 14. Pontos fracos

| # | Ponto fraco | Mitigável? |
|---|------------|:----------:|
| 1 | **Sem jornada do usuário (RF-36, *Must have*)** | ❌ Não |
| 2 | **Sem MFA robusto por padrão (RNF-31, *Must have*)** | ⚠️ TOTP com configuração; ou proxy autenticador |
| 3 | **Sem SSO (SAML/OIDC)** | ⚠️ Proxy autenticador |
| 4 | **Segmentação apenas por filtros simples** | ❌ Não |
| 5 | **Sem heatmap, replay, coorte, retenção, atribuição** | ❌ Não |
| 6 | **Sem Tag Manager** | ⚠️ Usar GTM ou Matomo Tag Manager |
| 7 | **Sem visitante recorrente confiável** | ❌ Consequência do design (deliberada) |
| 8 | **Dois bancos** — dobra a superfície de backup e DR | ⚠️ Automação |
| 9 | **Competência em ClickHouse é escassa** | ⚠️ Capacitação |
| 10 | **Ingestão sem fila** — limita absorção de picos instantâneos | ⚠️ Balanceador + réplicas |
| 11 | **Sem log de auditoria administrativo** | ❌ Não |
| 12 | **Sem roll-up multi-site** | ❌ Não |
| 13 | **Funcionalidades gated nos planos comerciais** | ❌ Não |
| 14 | **Comunidade e oferta de profissionais reduzidas no Brasil** | ⚠️ Capacitação |

---

## 15. Quando utilizar

> **✅ Use o Plausible quando:**
>
> - O requisito for **exclusivamente métrica agregada** — audiência, origem, páginas, dispositivos;
> - A **conformidade for prioridade máxima** e a simplicidade da base legal tiver alto valor;
> - O **desempenho do portal** for crítico e o peso do script importar;
> - Houver necessidade de **publicar estatísticas ao cidadão** com esforço mínimo (painel público);
> - O portal for de **conteúdo ou institucional**, sem jornada transacional a diagnosticar;
> - Houver equipe capaz de operar **ClickHouse**;
> - A **simplicidade de uso** para usuários não técnicos for requisito;
> - Como **camada complementar** ao Matomo em portais de alto volume e baixa criticidade analítica — **recomendação RC-1 deste estudo**.

---

## 16. Quando evitar

> **🚨 Evite o Plausible quando:**
>
> - For necessário **diagnosticar jornadas de serviço digital** — não há análise de fluxo (RF-36);
> - **Segmentação composta** for requisito analítico;
> - Forem necessários **heatmap, gravação de sessão ou análise de formulário**;
> - **Coortes e retenção** forem requisitos;
> - **SSO corporativo** for obrigatório sem componente adicional;
> - **Log de auditoria administrativo** for exigência de conformidade;
> - A organização não puder operar **ClickHouse**;
> - For necessária **visão consolidada multi-site** (roll-up);
> - Como **plataforma padrão única** de um Estado que precisa analisar serviços digitais — falha em requisitos obrigatórios.

---

## 17. Notas do avaliador

### 17.1 Notas atribuídas

| Critério | Peso | Nota | Pontos |
|----------|-----:|:----:|-------:|
| C01 — LGPD | 15 | **5** | 75 |
| C02 — Controle dos dados | 15 | **5** | 75 |
| C03 — TCO | 15 | **5** | 75 |
| C04 — Independência tecnológica | 10 | 4 | 40 |
| C05 — Recursos analíticos | 10 | 3 | 30 |
| C06 — APIs | 10 | 3 | 30 |
| C07 — Integrações | 10 | 3 | 30 |
| C08 — Escalabilidade | 10 | 4 | 40 |
| C09 — Segurança | 10 | 4 | 40 |
| C10 — Operação | 5 | 3 | 15 |
| C11 — Comunidade | 5 | 3 | 15 |
| C12 — Documentação | 5 | 4 | 20 |
| **Total** | **120** | | **485 / 600 (80,8 %)** |

### 17.2 Observação final

> **📌 Observação — Segundo lugar com papel complementar**
>
> O Plausible obtém a segunda maior pontuação do estudo, empatando com o Matomo On-Premise nos dois critérios de maior peso (C01 e C02, ambos nota 5). Sua limitação é a cobertura funcional: **nota 3 em C05**, com falha em requisitos *Must have* de jornada e MFA.
>
> Isso o torna **excelente no que faz e insuficiente para o escopo completo**. A recomendação do estudo — adotá-lo como camada complementar para portais de conteúdo (RC-1) — aproveita precisamente sua força sem depender de suas lacunas.
>
> Se o escopo do Estado fosse apenas portais institucionais e de conteúdo, sem serviços digitais transacionais, **o Plausible seria a recomendação principal**: menor TCO, menor complexidade, conformidade trivial. A presença de serviços digitais no escopo é o que desloca a decisão para o Matomo.

---

## Referências

- Site oficial: `https://plausible.io/`
- Documentação: `https://plausible.io/docs`
- Stats API: `https://plausible.io/docs/stats-api`
- Self-hosting: `https://github.com/plausible/community-edition`
- Política de dados: `https://plausible.io/data-policy`
- Repositório: `https://github.com/plausible/analytics`
- Licença AGPL v3: `https://www.gnu.org/licenses/agpl-3.0.html`
- Lista completa: [`../docs/12-referencias.md`, seção 9](../docs/12-referencias.md#9-documentação-oficial--plausible)

---

| ← Anterior | Índice | Próxima → |
|-----------|--------|-----------|
| [Google Analytics 4](google-analytics.md) | [Plataformas](../README.md) | [Umami](umami.md) |
