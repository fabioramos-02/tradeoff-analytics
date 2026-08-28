# Umami

> **Ficha técnica de plataforma** · Pontuação na matriz revisada 2026: **414/600 (69,0 %)** — 6º · 🟠 Condicionada
> **Situação:** ❌ **Não selecionada** — falha em requisito *Must have* RF-27 (funil de conversão)
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

Analytics privacy-first minimalista, lançado em 2020 por Mike Cao. Stack Next.js + PostgreSQL/MySQL. Licença MIT. Reescrita completa (v2) em 2023 com interface renovada e Umami Software Inc. constituída para operar o Umami Cloud.

**Segmento:** privacy-first. **Repositório:** `github.com/umami-software/umami`. **Site:** `umami.is`.

---

## 2. Licença e modelo

- **Licença:** MIT (permissiva).
- **Modelo:** open core — código integral do produto sob MIT; comercial vive no Umami Cloud.
- **Governança:** concentrada na Umami Software Inc. Base de contribuidores cresce lentamente.
- **Direito perpétuo:** MIT garante uso e modificação da versão obtida indefinidamente. Permite fechar versões futuras — risco de bifurcação declarado.

---

## 3. Hospedagem on-premise

**Stack (2 componentes):**

| Componente | Versão mínima | Papel |
|-----------|---------------|-------|
| Umami (Next.js) | 2.14+ | Aplicação + UI + coletor + API |
| PostgreSQL ou MySQL | PG 12+ / MySQL 8.0+ | Persistência |

**Distribuição:** container OCI oficial + `docker-compose.yml` de referência; Helm chart mantido pela comunidade. **Atualização:** trivial (`docker compose pull && up -d`). **Cron:** sem processo obrigatório.

**Dimensionamento estimado (12M PV/mês):** 1 vCPU + 2 GB RAM na aplicação; 4 vCPU + 8 GB RAM + 200 GB SSD no PG. Escala vertical suficiente até volume moderado; ClickHouse é modalidade nuvem, menos documentado no self-host.

---

## 4. Funcionalidades essenciais

| Requisito | Prio | Umami |
|-----------|:----:|:-----:|
| RF-01 Page views | M | ✅ |
| RF-02 Eventos customizados | M | ✅ |
| RF-05 Dimensões customizadas | S | ⚠️ Data payload livre, sem UI de segmentação |
| RF-09 SPAs | M | ✅ |
| RF-21 Audiência | M | ✅ |
| RF-22 UTM / campanhas | M | ✅ |
| RF-25 Geografia | S | ✅ |
| RF-26 Metas | M | ⚠️ Via evento nomeado |
| **RF-27 Funil** | **M** | **❌ Não tem** |
| RF-28 Segmentação avançada | M | ⚠️ Filtros básicos |
| RF-30 Tempo real | S | ✅ |
| RF-31 Coortes / retenção | C | ❌ |
| RF-32/33/34 Heatmap / replay / forms | C | ❌ |
| RF-36 Jornada | S | ❌ |
| RF-40 Multi-site + segregação | M | ✅ |
| RF-43 MFA admin | M | ❌ |
| RF-45 Tag Manager | S | ❌ |

**Cobertura ponderada (MoSCoW):** 35–54 % — nota **C05 = 2**.

**Reprovação em RF-27** é o motivo estrutural para não usar como plataforma padrão do parque nem do Xvia.

---

## 5. APIs e integrações

**API:**
- REST autenticada por Bearer token, para leitura e ingestão.
- Documentação enxuta; sem versionamento explícito.
- Sem webhooks nativos.
- Sem SDKs oficiais além do JS de tracking.

**Acesso ao dado bruto:** SQL direto ao PostgreSQL/MySQL — schema conhecido, tabelas `website`, `session`, `website_event`. Vantagem estrutural sobre APIs limitadas.

**Integração com BI:** via SQL direto (Metabase, Superset, Grafana, Power BI via ODBC). Sem conectores nativos, sem SSO SAML/OIDC.

**Tag Manager:** ausente. Instrumentação exige inserir `<script>` do umami em cada portal ou orquestrar via GTM.

---

## 6. Segurança e LGPD

**Conformidade por design:**
- Sem cookies.
- IP não armazenado em claro — hash com sal rotativo por sessão.
- Sem identificador persistente.

**Base legal:** dado coletado não é pessoal (LGPD art. 5º I), passa a anonimizado (art. 12). LGPD não incide — dispensa consentimento e RIPD específico.

**Segurança:**
- Base de código pequena e auditável.
- Gestão de CVE presente porém cadência irregular.
- **MFA ausente** — só usuário/senha. Contornável por proxy autenticador (Authelia, Cloudflare Access).
- RBAC básico (admin vs. usuário; sem granularidade fina).
- Log de auditoria ausente.

**Nota C01 (LGPD):** 5. **Nota C02 (Controle):** 5. **Nota C09 (Segurança):** 3.

---

## 7. Custos

TCO marginal SETDIG em 5 anos (regime C03 do [`../docs/03-criterios.md`](../docs/03-criterios.md#5-definição-operacional)):

| Rubrica | Valor |
|---------|------:|
| Licença | R$ 0 |
| Infra marginal (rateio PG compartilhado) | R$ 15 k |
| Equipe marginal (~0,04 FTE incremental) | R$ 50 k |
| Implantação inicial | R$ 5 k |
| Adequação LGPD (mínima — design conforme) | R$ 15 k |
| Contingência | R$ 15 k |
| **Total** | **~R$ 100 k** |

Menor TCO do escopo. Insuficiente para compensar reprovação em RF-27.

---

## 8. Pontos fortes / fracos

**Fortes:**
- Stack mais simples do escopo (2 componentes).
- Menor esforço operacional (~0,04 FTE).
- Menor TCO.
- Conformidade LGPD por design — dispensa CMP e RIPD.
- MIT permite uso irrestrito.

**Fracos:**
- **Sem funil (RF-27) — Must have não atendido.**
- Sem jornada, coortes, retenção, heatmap, session replay, forms.
- Sem MFA nativo, sem SSO, RBAC básico, sem log de auditoria.
- Sem Tag Manager.
- Sem SDKs oficiais além do JS.
- Comunidade menor; mercado brasileiro praticamente inexistente.
- Governança concentrada em um mantenedor comercial.

---

## 9. Quando usar / quando evitar

**Usar:**
- Sites temporários (campanha, evento) sem exigência de funil.
- Instrumentação simples de contagem em portais classe D do [`../docs/07-recomendacao.md §8`](../docs/07-recomendacao.md#8-estratégia-por-classe-de-portal).
- Referência de leveza para prototipagem.

**Evitar:**
- Como plataforma padrão do parque — falha em Must have.
- No Xvia — perfil transacional exige funil identificado.
- Portais com exigência de MFA para administradores.
- Contextos que exijam Tag Manager próprio.

---

## 10. Notas do avaliador

Ferramenta muito boa para o que se propõe: mensurar acesso a site pequeno com o mínimo de fricção. **Não é candidata a substituir Matomo**. A força do Umami — simplicidade radical — é o oposto do que o Estado precisa: funil, jornada, retenção, MFA, log de auditoria. Trocar Matomo por Umami no parque seria regressão funcional.

**Papel residual:** referência de leveza + opção declarada para portais sem exigência analítica.
