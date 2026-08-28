# PostHog

> **Ficha técnica de plataforma** · Pontuação na matriz revisada 2026: **464/600 (77,3 %)** (auto-hospedado OSS) — 4º · 🟡 Viável
> **Situação:** ✅ **Aprovada como camada complementar no portal Xvia** ([ADR-002 §6.2](../docs/06-adr-002.md#6-decisão)). Melhor opção soberana de product analytics disponível.
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

Plataforma de product analytics lançada em 2020 (EUA/Reino Unido). Alternativa auto-hospedada a Amplitude, Mixpanel e Heap. Escopo funcional amplo: eventos, funis, coortes, retenção, session replay, feature flags, experimentos A/B, surveys, HogQL (SQL sobre ClickHouse). Stack complexa: Django + PostgreSQL + ClickHouse + Kafka + Redis + MinIO. Mantida por PostHog Inc. (bem capitalizada, série D em 2024).

**Segmento:** product analytics. **Repositório:** `github.com/PostHog/posthog`. **Site:** `posthog.com`.

---

## 2. Licença e modelo

- **Licença (core):** MIT — cobre eventos, funis, retenção, HogQL, feature flags básicos.
- **Licença (Enterprise Edition):** proprietária — SSO SAML, RBAC granular, projetos ilimitados, session replay avançado, priorização de suporte.
- **Modelo:** open core. Divergência clara entre OSS e Cloud/EE.
- **Governança:** concentrada em PostHog Inc.
- **Direito perpétuo:** MIT sobre a versão obtida do core. **Sem direito equivalente sobre EE** — recursos avançados exigem licença comercial mesmo em auto-hospedado.

> **🚨 Descontinuação do suporte oficial ao self-host (2023)**
> Em 2023 a PostHog descontinuou o suporte à implantação auto-hospedada gerenciada (Kubernetes/Helm). Modalidade remanescente: `docker-compose` **explicitamente rotulada como não suportada em produção**, adequada apenas a avaliação. Estado assume integralmente a operação sem suporte do fornecedor. Trata-se de caso em que o risco TP-08 se concretizou — não hipotético.

---

## 3. Hospedagem on-premise

**Stack (6 componentes):**

| Componente | Versão | Papel |
|-----------|--------|-------|
| PostHog Web (Django) | 1.75+ | UI + API + ingestão HTTP |
| PostHog Plugin Server (Node.js) | 1.75+ | Transformação de eventos |
| PostgreSQL | 14+ | Metadados + configuração |
| ClickHouse | 24.3+ | Eventos brutos + session replay metadata |
| Kafka | 3.4+ | Streaming de eventos entre Django e ClickHouse |
| Redis | 7+ | Cache + coordenação |
| MinIO (ou S3) | — | Blobs de session replay |
| Zookeeper (Kafka legado) | 3.8+ | Coordenação Kafka (removível com Kafka KRaft) |

**Distribuição:** `docker-compose.yml` público (não suportado); Helm chart oficial **descontinuado** em 2023 — comunidade mantém forks. **Cron:** múltiplos (limpeza, agregação, retenção).

**Dimensionamento (12M PV/mês + 20 M eventos/mês):**
- Web (Django): 2× 4 vCPU + 8 GB
- Plugin Server: 2× 4 vCPU + 8 GB
- PostgreSQL: 8 vCPU + 32 GB + 200 GB SSD
- **ClickHouse: 16 vCPU + 64 GB + 2 TB SSD NVMe**
- Kafka: 3× 4 vCPU + 16 GB + 500 GB
- Redis: 4 GB
- MinIO: 1 TB (session replay é caro em storage)

**Esforço STI:** ~0,4 FTE incremental — R-18 exige plano de capacitação em ClickHouse + Kafka. Sem contratação de suporte especializado, operação é frágil.

---

## 4. Funcionalidades essenciais

| Requisito | Prio | PostHog SH |
|-----------|:----:|:----------:|
| RF-01 Page views | M | ✅ |
| RF-02 Eventos customizados | M | ✅ Nativo com autocaptura |
| RF-05 Dimensões customizadas | S | ✅ Ilimitadas |
| RF-06 Server-side tracking | S | ✅ SDKs multi-linguagem |
| RF-09 SPAs | M | ✅ |
| RF-21 Audiência | M | ⚠️ Via evento (não é o foco) |
| RF-22 UTM / campanhas | M | ⚠️ Via evento |
| RF-25 Geografia | S | ✅ |
| RF-26 Metas | M | ✅ |
| **RF-27 Funil** | M | ✅ Nativo, identificado |
| RF-28 Segmentação avançada | M | ✅ Cohorts + HogQL |
| RF-30 Tempo real | S | ✅ |
| RF-31 Coortes / retenção | C | ✅ Nativo |
| RF-32 Heatmap | C | ⚠️ Via session replay |
| RF-33 Session replay | C | ✅ Nativo, mascaramento configurável |
| RF-34 Form analytics | C | ⚠️ Via eventos + replay |
| **RF-35 Feature flags / A/B testing** | W | ✅ Diferencial do produto |
| RF-36 Jornada | S | ✅ Path analysis |
| RF-40 Multi-site + segregação | M | ✅ Projetos |
| **RF-43 MFA admin** | M | 💲 EE |
| RF-45 Tag Manager | S | ⚠️ Terceiros (GTM) |

**Cobertura ponderada (MoSCoW):** ≥ 90 % — nota **C05 = 5**.

**Diferenciais únicos:** feature flags integradas a experimentação; HogQL para consulta SQL rica sobre eventos; session replay com mascaramento nativo.

---

## 5. APIs e integrações

**API:**
- **REST v1 estável** — leitura, escrita, admin. Autenticação por Personal API Key. Versionamento explícito. Rate limits declarados.
- **Batch ingestion** para eventos.
- **HogQL** — SQL diretamente sobre ClickHouse via API ou UI.
- **Webhooks nativos** para triggers em feature flags e eventos.
- **SDKs oficiais:** JavaScript, Python, Node.js, Go, iOS, Android, React Native, PHP, Ruby, Java, .NET, Elixir, Flutter.

**Acesso ao dado bruto:** HogQL + acesso SQL direto ao ClickHouse.

**Integração com BI:** ClickHouse conecta a Grafana, Superset, Metabase; Power BI via ODBC ClickHouse. Destinos nativos: Slack, Zapier, webhooks genéricos. SSO SAML/OIDC **só na EE**.

**Tag Manager:** sem TM próprio; instrumentação por SDK ou GTM.

---

## 6. Segurança e LGPD

**LGPD — não conforme por padrão; exige configuração ativa:**
- Cookies de identificação **habilitados por padrão** — precisa desativar (`persistence: 'memory'`) ou aceitar com consentimento.
- Autocapture pode coletar valores de campos — precisa mascaramento agressivo (`mask_all_text: true`, `mask_all_element_attributes: true`).
- Session replay armazena rendering do DOM — precisa `sessionRecording.maskAllInputs: true` e opt-in explícito.
- IP anonimizável, mas não é padrão.

**Configuração conforme LGPD exige RIPD específico** — coberto por R-11 do [ADR-002 §9](../docs/06-adr-002.md#9-riscos-assumidos). Governança do consentimento é obrigatória.

**Segurança:**
- Código auditável (core MIT).
- Gestão de CVE ativa.
- SOC 2 Type II na Cloud (não aplicável a self-host).
- MFA para admin **só na EE**.
- RBAC granular **só na EE**.
- Auditoria completa **só na EE**.

**Nota C01 (LGPD):** 4. **Nota C02 (Controle):** 5. **Nota C09 (Segurança):** 4.

---

## 7. Custos

TCO marginal SETDIG 5 anos — inclui EE mínima (SSO + RBAC obrigatórios em contexto gov):

| Rubrica | Valor |
|---------|------:|
| Licença EE (SSO + RBAC) | R$ 300 k |
| Infra marginal (Kafka + CH dedicado + MinIO **fora do padrão SETDIG**) | R$ 300 k |
| Equipe marginal (~0,4 FTE — curva ClickHouse + Kafka alta) | R$ 240 k |
| Implantação inicial + PoC | R$ 45 k |
| Adequação LGPD (RIPD replay, governança consentimento) | R$ 40 k |
| Contingência (riscos R-11, R-18, R-20) | R$ 30 k |
| **Total** | **~R$ 955 k** |

**Cerca de 4× o TCO marginal do Matomo OP.** Por isso o C03 rebaixado a peso 0 no ADR-002 é o que viabiliza a coexistência no Xvia — custo é modelado, mas não filtra.

---

## 8. Pontos fortes / fracos

**Fortes:**
- Cobertura funcional ≥ 90 % — a maior entre soberanas.
- Feature flags + experimentação estruturada em produto único (não há equivalente open source).
- Session replay com mascaramento nativo.
- HogQL para consulta SQL rica.
- SDKs modernos em várias linguagens.
- ClickHouse escala massivamente.
- Comunidade grande e ativa; cadência de releases elevada.

**Fracos:**
- **6 componentes** em stack distribuída — operação frágil.
- **Suporte oficial ao self-host descontinuado em 2023** (R-20).
- **Não conforme LGPD por padrão** — configuração ativa obrigatória.
- MFA, SSO, RBAC granular e auditoria só na EE paga.
- Custo marginal 4× o Matomo OP.
- Curva de aprendizado ClickHouse + Kafka alta; competência escassa no BR.
- Sem Tag Manager próprio.

---

## 9. Quando usar / quando evitar

**Usar:**
- **Portal Xvia**, em coexistência com Matomo — decisão ADR-002 §6.2.
- Produtos digitais transacionais que exijam funil identificado, feature flags e experimentação estruturada.
- Contexto onde equipe tem capacidade ou verba para operar ClickHouse + Kafka.

**Evitar:**
- Como plataforma padrão do parque — perfil de conteúdo não justifica o overhead.
- Sem plano formal de capacitação da STI ou contratação de suporte especializado.
- Portais com dado sensível sem RIPD específico para session replay (R-11).
- Portais onde MFA é *Must have* sem orçamento para EE.

---

## 10. Notas do avaliador

Ferramenta poderosa; melhor open source de product analytics. Descontinuação do suporte ao K8s em 2023 é sinal amarelo — indica que a PostHog Inc. deve ao longo do tempo migrar valor pro Cloud, deixando o self-host como opção "para quem quiser". Estado assume esse risco conscientemente no ADR-002.

**Coexistência com Matomo no Xvia é a solução certa** — cobre a lacuna estrutural de product analytics sem substituir a base cookieless auditada do Matomo. Gate de 12 meses na Onda 5 do roadmap valida se investimento se paga em feature flags ativas, funis identificados e replay em serviço crítico.

Se o gate falhar, contingência formal é PostHog Cloud EU com RIPD específico — mitigação de transferência internacional via região UE — ou retorno a Matomo puro no Xvia com plugins.
