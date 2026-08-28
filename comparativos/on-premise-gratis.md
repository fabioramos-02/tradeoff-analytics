# Comparativo — Web Analytics on-premise gratuito

> **Corte transversal** · As 4 plataformas do escopo lado a lado nas dimensões estratégicas: LGPD, controle, custo marginal, complexidade e cobertura funcional.
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/05-matriz.md) · [ADR-002](../docs/06-adr-002.md)

## Sumário

- [1. Sinopse](#1-sinopse)
- [2. LGPD e controle de dados](#2-lgpd-e-controle-de-dados)
- [3. Stack e complexidade operacional](#3-stack-e-complexidade-operacional)
- [4. Custo marginal 5 anos](#4-custo-marginal-5-anos)
- [5. Cobertura funcional](#5-cobertura-funcional)
- [6. API e integração com BI](#6-api-e-integração-com-bi)
- [7. Comunidade e sustentabilidade](#7-comunidade-e-sustentabilidade)
- [8. Papel na arquitetura recomendada](#8-papel-na-arquitetura-recomendada)

---

## 1. Sinopse

| Dimensão | **Matomo OP** | **PostHog SH** | **Plausible CE** | **Umami** |
|----------|:-------------:|:--------------:|:----------------:|:---------:|
| Licença | GPL v3 | MIT + EE proprietária | AGPL v3 | MIT |
| Segmento | WA tradicional | Product analytics | Privacy-first | Privacy-first |
| Pontuação matriz 2026 | **500** (83,3 %) | 464 (77,3 %) | 460 (76,7 %) | 414 (69,0 %) |
| Stack (componentes) | 3 (PHP + MySQL + Redis) | 6 (Django + PG + ClickHouse + Kafka + Redis + MinIO) | 3 (Elixir + PG + ClickHouse) | 2 (Next.js + PG/MySQL) |
| Cookieless por default | ⚠️ Configurável | ❌ Padrão identifica | ✅ Sim | ✅ Sim |
| Precedente CNIL | ✅ | ❌ | ❌ | ❌ |
| Funil (RF-27, *Must*) | 💲 Plugin pago | ✅ Nativo | ✅ Nativo | ❌ **Reprovado** |
| MFA nativo | ✅ TOTP | ✅ EE | ❌ (via proxy) | ❌ |
| SSO SAML/OIDC | 💲 Plugin pago | 💲 EE | ❌ | ❌ |
| Suporte oficial ao self-host | ✅ Estável | ❌ **Descontinuado 2023** | ✅ Docker Compose | ✅ Docker Compose |
| TCO marginal 5a (SETDIG) | ~R$ 235 k | ~R$ 955 k | ~R$ 120 k | ~R$ 100 k |

---

## 2. LGPD e controle de dados

| Aspecto | Matomo OP | PostHog SH | Plausible CE | Umami |
|---------|:---------:|:----------:|:------------:|:-----:|
| Sem transferência internacional | ✅ | ✅ | ✅ | ✅ |
| Anonimização de IP nativa | ✅ Configurável (0–4 bytes) | ⚠️ Configurável | ✅ Não armazena | ✅ Hash com sal rotativo |
| Cookieless nativo | ⚠️ Configurável | ❌ Cookie identifica | ✅ Design | ✅ Design |
| Retenção controlada pelo Estado | ✅ | ✅ | ✅ | ✅ |
| Direitos do titular exercíveis (art. 18) | ✅ SQL direto | ✅ HogQL | ✅ SQL direto | ✅ SQL direto |
| Opt-out nativo | ✅ Iframe + API | ✅ SDK | ⚠️ Manual | ⚠️ Manual |
| Log de auditoria | ✅ Nativo | ✅ | ⚠️ Básico | ❌ |
| Session replay (implica RIPD) | 💲 Plugin | ✅ Nativo | ❌ | ❌ |
| Nota C01 (LGPD) na matriz | **5** | 4 | **5** | **5** |
| Nota C02 (Controle) na matriz | **5** | **5** | **5** | **5** |

> **📌 Configuração ótima**
> Matomo, Plausible e Umami operam em modo em que dado coletado **não é pessoal** (art. 5º I) — passa a anonimizado (art. 12) e LGPD deixa de incidir. PostHog é orientado a identificação: conformidade exige trabalho ativo (mascaramento agressivo, opt-in). Trade-off aceito no ADR-002 para o Xvia por RIPD específico + governança do consentimento.

---

## 3. Stack e complexidade operacional

| Componente | Matomo OP | PostHog SH | Plausible CE | Umami |
|-----------|:---------:|:----------:|:------------:|:-----:|
| Aplicação | PHP 8.x | Django (Python) | Elixir | Node.js |
| Banco relacional | MySQL/MariaDB | PostgreSQL | PostgreSQL | PG ou MySQL |
| Banco colunar | ❌ | ✅ ClickHouse | ✅ ClickHouse | ❌ (opcional CH em nuvem) |
| Fila / streaming | Redis (plugin) | ✅ Kafka | ❌ | ❌ |
| Cache | Redis | Redis | ❌ | ❌ |
| Object storage | ❌ | ✅ MinIO (session replay) | ❌ | ❌ |
| Container oficial | ✅ | ✅ (Compose "não suportado") | ✅ | ✅ |
| Helm chart oficial | ⚠️ Comunidade | ❌ **Descontinuado 2023** | ⚠️ Comunidade | ⚠️ Comunidade |
| Cron obrigatório | ✅ Arquivamento | ❌ | ❌ | ❌ |
| Esforço STI (~FTE) | ~0,3 | **~0,8** | ~0,15 | ~0,1 |
| Nota C10 (Operação) | 3 | **1** | 3 | 4 |

> **🚨 PostHog SH exige competência ClickHouse + Kafka**
> Descontinuação do suporte oficial ao K8s em 2023 desloca integralmente o ônus operacional ao Estado. Documentação self-host explicitamente rotulada como "não suportada em produção" — recomendada só para avaliação. R-18 mitiga por capacitação + reserva orçamentária para suporte especializado.

---

## 4. Custo marginal 5 anos

Regime marginal SETDIG — infra compartilhada e operação de kernel absorvidas pelo contrato existente. Detalhamento em `../anexos/historico/comparativos/custo.md`.

| Rubrica | Matomo OP | PostHog SH | Plausible CE | Umami |
|---------|----------:|-----------:|-------------:|------:|
| Licença de software | R$ 0 | R$ 300 k (EE: SSO+RBAC) | R$ 0 | R$ 0 |
| Plugins premium | R$ 60 k | — | — | — |
| Infra marginal | R$ 30 k | R$ 300 k (Kafka + CH dedicado + MinIO fora do padrão) | R$ 20 k | R$ 15 k |
| Equipe marginal (FTE incremental) | R$ 75 k (~0,1) | R$ 240 k (~0,4) | R$ 60 k (~0,05) | R$ 50 k (~0,04) |
| Implantação inicial | R$ 10 k | R$ 45 k | R$ 10 k | R$ 5 k |
| Adequação LGPD | R$ 30 k | R$ 40 k | R$ 15 k | R$ 15 k |
| Contingência (riscos) | R$ 30 k | R$ 30 k | R$ 15 k | R$ 15 k |
| **Total 5 anos** | **~R$ 235 k** | **~R$ 955 k** | **~R$ 120 k** | **~R$ 100 k** |

> **📌 Custo não é filtro de decisão no ADR-002** (C03 = peso 0). Modelagem serve a dimensionamento e prestação de contas ao TCE-MS.

---

## 5. Cobertura funcional

Ponderação por MoSCoW: M=3, S=2, C=1.

| Requisito | Prio | Matomo OP | PostHog SH | Plausible CE | Umami |
|-----------|:----:|:---------:|:----------:|:------------:|:-----:|
| RF-01 Page views | M | ✅ | ✅ | ✅ | ✅ |
| RF-02 Eventos customizados | M | ✅ | ✅ | ✅ | ✅ |
| RF-05 Dimensões customizadas | S | ✅ | ✅ | 💲 Plano pago | ⚠️ |
| RF-09 SPAs | M | ✅ | ✅ | ✅ | ✅ |
| RF-21 Audiência | M | ✅ | ⚠️ (via evento) | ✅ | ✅ |
| RF-22 Aquisição / UTM | M | ✅ | ⚠️ | ✅ | ✅ |
| RF-26 Metas | M | ✅ | ✅ | ✅ | ⚠️ |
| **RF-27 Funil** | **M** | ✅ 💲 | ✅ | ✅ | ❌ |
| RF-28 Segmentação avançada | M | ✅ | ✅ | ⚠️ Filtros | ⚠️ |
| RF-30 Tempo real | S | ✅ | ✅ | ✅ | ✅ |
| RF-31 Coortes / retenção | C | ⚠️ | ✅ | ❌ | ❌ |
| RF-32 Heatmap | C | 💲 | ⚠️ Via replay | ❌ | ❌ |
| RF-33 Session replay | C | 💲 | ✅ | ❌ | ❌ |
| RF-35 A/B testing / Feature flags | W | 💲 | ✅ | ❌ | ❌ |
| RF-36 Jornada do usuário | S | ✅ | ✅ | ❌ | ❌ |
| RF-40 Multi-site + segregação | M | ✅ | ✅ | ✅ | ✅ |
| RF-43 MFA admin | M | ✅ | 💲 EE | ❌ | ❌ |
| RF-45 Tag Manager próprio | S | ✅ | ⚠️ | ❌ | ❌ |
| **Nota C05 (Recursos)** | | **4** | **5** | 3 | **2** |

> **🚨 Umami reprovado em RF-27 (funil, *Must have*)**
> Não pode ser plataforma padrão do parque nem do Xvia. Papel residual: portais classe D (temporários) ou instrumentação de contagem simples.

---

## 6. API e integração com BI

| Aspecto | Matomo OP | PostHog SH | Plausible CE | Umami |
|---------|:---------:|:----------:|:------------:|:-----:|
| API REST leitura | ✅ Reporting API | ✅ | ✅ Stats API | ✅ |
| API REST escrita/ingestão | ✅ Tracking API | ✅ | ✅ Events API | ✅ |
| Query SQL sobre bruto | ✅ Direto ao MySQL | ✅ HogQL | ✅ Direto ao ClickHouse | ✅ Direto ao PG/MySQL |
| Formatos de saída | JSON, XML, CSV, TSV, HTML, RSS | JSON | JSON, CSV | JSON, CSV |
| SDKs oficiais | PHP + móveis | JS, Py, Node, Go, iOS, Android, RN | ❌ (só comunitário) | ❌ (só comunitário) |
| Webhooks nativos | ❌ | ✅ | ❌ | ❌ |
| Versionamento explícito | ⚠️ Implícito | ✅ v1 | ✅ | ❌ |
| GraphQL | ❌ | ❌ | ❌ | ❌ |
| Conector Power BI nativo | ❌ (via ODBC MySQL) | ❌ (via ClickHouse) | ❌ (via ClickHouse) | ❌ (via ODBC) |
| Integração Metabase/Superset/Grafana | ✅ SQL direto | ✅ ClickHouse | ✅ ClickHouse | ✅ SQL direto |
| **Nota C06 (APIs)** | 4 | 4 | 3 | 3 |

> **📌 SQL direto sobre banco vs API**
> Todas as 4 permitem acesso SQL ao banco de bruto — vantagem estrutural sobre qualquer SaaS. Padrão do Estado: **Reporting API para consumo externo; réplica de leitura só para ETL do DW corporativo** (não expor a BI de autoatendimento).

---

## 7. Comunidade e sustentabilidade

| Aspecto | Matomo OP | PostHog SH | Plausible CE | Umami |
|---------|:---------:|:----------:|:------------:|:-----:|
| Ano de criação | 2007 | 2020 | 2019 | 2020 |
| Mantenedor principal | InnoCraft (NZ) + comunidade | PostHog Inc. | Plausible Insights OÜ (EE) | Umami Software Inc. |
| Adoção institucional pública | ✅ Comissão Europeia; setor público FR/DE | ⚠️ Emergente | ⚠️ Emergente | ⚠️ Baixa |
| Precedente regulatório (DPA) | ✅ CNIL | ❌ | ❌ | ❌ |
| Cadência de releases | Regular (mensal) | Elevada (semanal) | Regular (mensal) | Regular (mensal) |
| Risco de relicenciamento | 🟢 Baixo (GPL v3, contribuidores diversos) | 🟡 Médio (**já concretizado 2023**) | 🟡 Médio (AGPL, mantenedor único) | 🟡 Médio (MIT permite fechar futuras) |
| Profissionais no BR | ✅ Razoável | ⚠️ Baixa | ⚠️ Baixa | ❌ |
| **Nota C11 (Comunidade)** | 4 | 4 | 3 | 3 |
| **Nota C12 (Documentação)** | 4 | 4 | 4 | 3 |

---

## 8. Papel na arquitetura recomendada

| Plataforma | Papel | Onde | Fundamento |
|-----------|-------|------|------------|
| **Matomo OP** | 🥇 **Padrão único** do parque | EDS + sites gov MS | Cobertura + precedente CNIL + auditoria verificável + custo previsível |
| **Matomo OP** | Co-instância no **Xvia** para audiência anônima | Portal Xvia | Base legal cookieless + roll-up + Tag Manager próprio |
| **PostHog SH** | Camada de **product analytics** no Xvia | Portal Xvia | Único soberano que cobre funil identificado + feature flags + replay + HogQL |
| **Plausible CE** | Camada leve **opcional** para conteúdo classe C | Portais de altíssimo volume, baixa criticidade | Cookieless por design + stack simples |
| **Umami** | **Referência de leveza** | Fora do escopo produtivo (reprovado em RF-27) | Menor stack e custo — sem funil = sem plataforma padrão |

---

## Navegação

| Documento | Papel |
|-----------|-------|
| [Matomo × PostHog](matomo-vs-posthog.md) | Complementaridade binária |
| [Coexistência no Xvia](coexistencia-matomo-posthog.md) | Modelo operacional |
| [Matriz de decisão](../docs/05-matriz.md) | Notas nota-a-nota |
| [ADR-002](../docs/06-adr-002.md) | Decisão vigente |
