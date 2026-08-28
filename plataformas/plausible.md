# Plausible

> **Ficha técnica de plataforma** · Pontuação na matriz revisada 2026: **460/600 (76,7 %)** — 5º · 🟡 Viável
> **Situação:** ✅ **Opcional** — camada leve para portais classe C (conteúdo de altíssimo volume, baixa criticidade analítica)
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/05-matriz.md)

## Sumário

- [1. Visão geral](#1-visão-geral)
- [2. Licença e modelo](#2-licença-e-modelo)
- [3. Hospedagem on-premise](#3-hospedagem-on-premise)
- [4. Funcionalidades essenciais](#4-funcionalidades-essenciais)
- [5. APIs e integrações](#5-apis-e-integrações)
- [6. Segurança e LGPD](#6-segurança-e-lgpd)
- [7. Custos](#7-custos)
- [8. Pontos fortes / fracos](#8-pontos-fortes--fracos)
- [9. Quando usar / quando evitar](#9-quando-usar--quando-evitar)
- [10. Notas do avaliador](#10-notas-do-avaliador)

---

## 1. Visão geral

Analytics privacy-first lançado em 2019 na Estônia por Uku Täht e Marko Saric como alternativa cookieless ao Google Analytics. Stack Elixir + PostgreSQL + ClickHouse. Community Edition sob AGPL v3. Cloud comercial na UE. Comunidade moderada; adoção institucional pública crescente pós-2022.

**Segmento:** privacy-first. **Repositório:** `github.com/plausible/analytics`. **Site:** `plausible.io`.

---

## 2. Licença e modelo

- **Licença (Community Edition):** AGPL v3 — copyleft mais forte que GPL; qualquer serviço público derivado deve disponibilizar o código.
- **Modelo:** open core moderado — CE cobre o essencial; recursos comerciais (funil avançado, times, custom properties ilimitadas) só na Cloud paga.
- **Governança:** concentrada em Plausible Insights OÜ (Estônia).
- **Direito perpétuo:** AGPL garante uso da versão obtida. Divergência entre CE e Cloud paga cresceu desde 2023.

---

## 3. Hospedagem on-premise

**Stack (3 componentes):**

| Componente | Versão mínima | Papel |
|-----------|---------------|-------|
| Plausible (Elixir/Phoenix) | 2.1+ | Aplicação + UI + coletor + API |
| PostgreSQL | 14+ | Metadados (usuários, sites, config) |
| ClickHouse | 24.3+ | Eventos brutos |

**Distribuição:** container OCI oficial + `docker-compose.yml`; Helm chart mantido pela comunidade. **Cron:** sem processo obrigatório crítico. **Atualização:** documentada, trivial em Compose. Migrações de ClickHouse exigem atenção em grandes upgrades.

**Dimensionamento (12M PV/mês):** 2 vCPU + 4 GB RAM na aplicação; 4 vCPU + 8 GB RAM + 200 GB SSD no PG; **8 vCPU + 32 GB RAM + 500 GB SSD no ClickHouse**. Competência ClickHouse é escassa no mercado — dimensionamento incorreto degrada relatórios sob volume.

---

## 4. Funcionalidades essenciais

| Requisito | Prio | Plausible CE |
|-----------|:----:|:------------:|
| RF-01 Page views | M | ✅ |
| RF-02 Eventos customizados | M | ✅ |
| RF-05 Dimensões customizadas | S | 💲 Plano pago |
| RF-09 SPAs | M | ✅ (`plausible.js` manual) |
| RF-21 Audiência | M | ✅ |
| RF-22 UTM / campanhas | M | ✅ |
| RF-25 Geografia | S | ✅ |
| RF-26 Metas | M | ✅ |
| **RF-27 Funil** | M | ✅ Nativo |
| RF-28 Segmentação avançada | M | ⚠️ Filtros — sem segmentos compostos |
| RF-30 Tempo real | S | ✅ |
| RF-31 Coortes / retenção | C | ❌ |
| RF-32/33/34 Heatmap / replay / forms | C | ❌ |
| RF-36 Jornada | S | ❌ |
| RF-40 Multi-site + segregação | M | ✅ |
| RF-43 MFA admin | M | ❌ — só usuário/senha |
| RF-45 Tag Manager | S | ❌ |

**Cobertura ponderada (MoSCoW):** 55–74 % — nota **C05 = 3**.

Cobre WA agregado com robustez. **Não cobre** jornada, coortes, retenção, heatmap, session replay. Para portais de conteúdo (classe C) é adequado; para serviço transacional, limitado.

---

## 5. APIs e integrações

**API:**
- **Stats API** REST autenticada por Bearer token — leitura de agregados. Versionada explicitamente (`/api/v1/stats`). Bem documentada.
- **Events API** para ingestão via HTTP.
- Sem webhooks nativos.
- SDKs oficiais: só `plausible.js`. Terceiros mantêm wrappers Py/Node.

**Acesso ao dado bruto:** SQL direto ao ClickHouse — schema documentado, tabelas `events_v2`, `sessions_v2`. Vantagem estrutural sobre APIs limitadas.

**Integração com BI:** ClickHouse conecta nativamente a Grafana, Superset, Metabase; Power BI via ODBC ClickHouse. Sem conectores oficiais certificados. Sem SSO SAML/OIDC — contornável por proxy autenticador.

**Tag Manager:** ausente. Instrumentação via `<script>` direto ou via GTM.

---

## 6. Segurança e LGPD

**Conformidade por design:**
- Sem cookies.
- Sem armazenamento de IP em claro (hash com sal rotativo diário).
- Sem identificador persistente.

**Base legal:** dado coletado não é pessoal (LGPD art. 5º I) — anonimizado (art. 12). LGPD não incide. Dispensa CMP, consentimento e RIPD específico.

**Segurança:**
- Base auditável (Elixir tem base pequena e madura).
- Gestão de CVE ativa.
- **MFA ausente** — usuário/senha. Compensável por proxy autenticador ou VPN.
- RBAC básico.
- Log de auditoria limitado.

**Nota C01 (LGPD):** 5. **Nota C02 (Controle):** 5. **Nota C09 (Segurança):** 4.

---

## 7. Custos

TCO marginal SETDIG 5 anos:

| Rubrica | Valor |
|---------|------:|
| Licença | R$ 0 |
| Infra marginal (rateio ClickHouse compartilhado) | R$ 20 k |
| Equipe marginal (~0,05 FTE) | R$ 60 k |
| Implantação inicial | R$ 10 k |
| Adequação LGPD (mínima) | R$ 15 k |
| Contingência | R$ 15 k |
| **Total** | **~R$ 120 k** |

Segundo menor TCO do escopo, atrás do Umami. Vantagem sobre Matomo (~R$ 235 k) é modesta e não compensa cobertura funcional inferior.

---

## 8. Pontos fortes / fracos

**Fortes:**
- Conformidade LGPD por design — dispensa CMP.
- Cobertura funcional razoável (WA agregado + funil).
- ClickHouse escala com margem sobre o cenário de pico.
- Interface limpa, curva de aprendizado baixa.
- AGPL v3 protege bem contra apropriação privada.

**Fracos:**
- Sem jornada, coortes, retenção, heatmap, session replay, forms.
- Sem MFA nativo, sem SSO.
- Sem Tag Manager próprio.
- Ingestão síncrona sem fila — pico instantâneo é problema.
- Competência ClickHouse escassa no BR.
- Mantenedor comercial único.

---

## 9. Quando usar / quando evitar

**Usar:**
- Portais de conteúdo de altíssimo volume, baixa criticidade analítica (classe C).
- Sites que só precisam de contagem de visitantes e páginas populares.
- Ambiente onde MFA é resolvido por infra externa (VPN, proxy).

**Evitar:**
- Como plataforma padrão do parque — falha em RF-36 (jornada); RF-27 pleno exige plano Cloud pago para segmentação plena.
- No Xvia — não cobre product analytics.
- Serviços transacionais que exijam diagnóstico de jornada de conversão.

---

## 10. Notas do avaliador

Excelente para o que se propõe: métrica agregada de portal com o mínimo de sobrecarga regulatória. **Não substitui Matomo no parque** por falta de jornada e MFA. Papel declarado no ADR-002 §6.3: opção facultativa para portais classe C mediante justificativa.

Adoção crescente em municípios europeus pós-2022. Para o parque atual, cobertura funcional insuficiente.
