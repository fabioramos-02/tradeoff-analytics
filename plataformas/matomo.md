# Matomo

> **Ficha técnica de plataforma** · Pontuação na matriz revisada 2026: **500/600 (83,3 %)** — 🥇 1º · 🟢 Recomendada
> **Situação:** ✅ **Plataforma padrão do parque** ([ADR-002 §6.1](../docs/06-adr-002.md#6-decisão)) · **Co-instância no Xvia** para audiência anônima
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

Plataforma de Web Analytics lançada em 2007 como Piwik pelo francês Matthieu Aubry; renomeada Matomo em 2018. Alternativa livre e auto-hospedada ao Google Analytics. Stack PHP + MySQL/MariaDB. Comunidade grande e ativa há 15+ anos; **adoção institucional documentada** — Europa Analytics da Comissão Europeia opera sobre Matomo; setor público FR/DE recomenda em guias RGPD.

**Segmento:** Web Analytics tradicional. **Mantenedor:** InnoCraft (NZ). **Repositório:** `github.com/matomo-org/matomo`. **Site:** `matomo.org`.

### Precedente CNIL

Autoridade francesa de proteção de dados (CNIL) reconhece configuração específica do Matomo (cookieless, IP anonimizado, sem cruzamento com outros tratamentos) como **isenta de consentimento prévio** — publicada como *exemption de consentement*. Único produto da matriz com precedente favorável de DPA. Aplicável por analogia perante a ANPD.

---

## 2. Licença e modelo

- **Licença:** GPL v3 — copyleft forte; modificações distribuídas devem ser abertas.
- **Modelo:** open core moderado — núcleo GPL cobre todas capacidades essenciais; **plugins premium (Funnels, Heatmaps & Session Recording, Form Analytics, A/B Testing, Media Analytics, Roll-Up, Custom Reports) são pagos** e vendidos pela InnoCraft (marketplace).
- **Governança:** InnoCraft + comunidade ampla. Base de contribuidores diversa; risco de relicenciamento baixo.
- **Direito perpétuo:** GPL v3 garante direito sobre a versão obtida indefinidamente. Fork tecnicamente viável.

---

## 3. Hospedagem on-premise

**Stack (2 componentes + 1 recomendado):**

| Componente | Versão mínima | Papel |
|-----------|---------------|-------|
| Matomo (PHP) | 5.x + PHP 8.1+ | Aplicação + UI + coletor + API |
| MySQL / MariaDB | 5.7+ / 10.3+ | Persistência (dado bruto + agregado) |
| Redis (recomendado p/ QueuedTracking) | 6+ | Fila de ingestão assíncrona |

**Distribuição:** container OCI oficial + tarball; Helm chart mantido pela comunidade. **Cron obrigatório:** `console core:archive` (arquivamento periódico agrega bruto em relatórios pré-calculados). **Atualização:** interface + CLI, versionamento previsível. Suporte LTS.

**Dimensionamento (12M PV/mês) — arquitetura de referência em [`../docs/07-recomendacao.md §9`](../docs/07-recomendacao.md#9-dimensionamento):**
- Coleta stateless: 2× 4 vCPU + 8 GB (escala horizontal)
- UI relatórios: 1× 4 vCPU + 16 GB
- Host arquivamento dedicado: 1× 8 vCPU + 16 GB
- Redis: 1× 2 vCPU + 8 GB
- MySQL primário: 1× 8 vCPU + 32 GB + 1 TB SSD NVMe (`buffer_pool_size` ≈ 24 GB)
- MySQL réplica leitura: mesmo perfil

**Esforço STI:** ~0,3 FTE permanente.

**Lacunas conhecidas (mitigáveis):** L1 ingestão síncrona (mitigado por QueuedTracking + Redis); L3 competição de BI com coleta (mitigado por réplica de leitura); L1 gargalo de arquivamento em pico (mitigado por host dedicado + paralelização). Ver AA-1 a AA-12 em [`../docs/07-recomendacao.md §5`](../docs/07-recomendacao.md#5-arquitetura-alvo-de-referência).

---

## 4. Funcionalidades essenciais

| Requisito | Prio | Matomo OP |
|-----------|:----:|:---------:|
| RF-01 Page views | M | ✅ |
| RF-02 Eventos customizados | M | ✅ |
| RF-05 Dimensões customizadas | S | ✅ Visita + ação |
| RF-06 Server-side tracking | S | ✅ API HTTP direta |
| RF-09 SPAs | M | ✅ |
| RF-21 Audiência | M | ✅ |
| RF-22 UTM / campanhas | M | ✅ |
| RF-23 Comportamento (fluxo) | M | ✅ |
| RF-25 Geografia | S | ✅ |
| RF-26 Metas | M | ✅ Com valor e atribuição |
| **RF-27 Funil** | M | ✅ **Plugin pago Funnels** |
| RF-28 Segmentação avançada | M | ✅ |
| RF-29 Comparação de períodos | S | ✅ |
| RF-30 Tempo real | S | ✅ Widget nativo |
| RF-31 Coortes / retenção | C | ⚠️ Coortes existem; retenção limitada |
| RF-32 Heatmap | C | 💲 Plugin *Heatmaps & Session Recording* |
| RF-33 Session replay | C | 💲 Plugin *Heatmaps & Session Recording* |
| RF-34 Form analytics | C | 💲 Plugin *Form Analytics* |
| RF-35 A/B testing | W | 💲 Plugin *A/B Testing* |
| RF-36 Jornada | S | ✅ Users Flow |
| RF-40 Multi-site + segregação | M | ✅ Multi-site nativo |
| RF-41 RBAC | M | ✅ Granular por site |
| RF-43 MFA admin | M | ✅ TOTP nativo |
| RF-44 UI pt-BR | S | ✅ |
| RF-45 Tag Manager | S | ✅ **Nativo** — independente do GTM |

**Cobertura ponderada (MoSCoW):** 75–89 % — nota **C05 = 4**.

Plugins premium fecham as lacunas em `C` mediante custo modesto (~R$ 60 k / 5 anos para o pacote típico). Cobertura de `M` e `S` no núcleo GPL é integral.

---

## 5. APIs e integrações

**API:**
- **Reporting API** — HTTP GET/POST completo, autenticação por `token_auth`. Saída em **JSON, XML, CSV, TSV, HTML, RSS**. Todas as métricas da UI acessíveis programaticamente.
- **Tracking API** — HTTP direto para tracking server-side (`matomo.php` recebe eventos via HTTP mesmo sem JS).
- **API de administração** — CRUD de sites, usuários, metas, dimensões.
- Sem webhooks nativos (limitação); sem GraphQL.
- SDKs oficiais: **PHP + móveis (iOS/Android)**. SDKs Python e Node.js só comunitários.

**Acesso ao dado bruto:** SQL direto ao MySQL — schema documentado. Tabelas críticas: `matomo_log_visit` (sessões), `matomo_log_link_visit_action` (eventos/PV), `matomo_log_action` (dicionário), `matomo_log_conversion` (metas), `matomo_site` (dimensão). Padrão recomendado para ETL: usar **views SQL estáveis** entre schema e pipeline para isolar mudanças de versão.

**Integração com BI:** SQL direto habilita **Metabase, Superset, Power BI (via ODBC MySQL), Grafana** sem intermediar API — vantagem estrutural sobre qualquer SaaS.

**Integrações prontas:** GTM, WordPress (plugin oficial), Drupal, Wix, Joomla, Shopify. **Consent management** via CMP externa. **SSO** por plugins pagos (SAML, OIDC).

---

## 6. Segurança e LGPD

**LGPD — conformidade sob configuração ativa (procedimento em [`../docs/07-recomendacao.md §6`](../docs/07-recomendacao.md#6-padrão-de-conformidade-lgpd)):**
- Anonimização de IP: até 4 bytes mascarados.
- Cookieless configurável (`disableCookies()`).
- Retenção definida pelo Estado; exclusão física verificável via SQL.
- Opt-out nativo por iframe + API.
- Respeita Do Not Track.
- Registro de operações via log administrativo.

Configuração conforme = dado **não pessoal** (art. 5º I) → anonimizado (art. 12). LGPD não incide: dispensa consentimento, CMP e RIPD específico (mas RIPD geral da plataforma continua obrigatório).

**Segurança:**
- Gestão de vulnerabilidades ativa; programa formal de divulgação.
- **MFA TOTP nativo** para administradores.
- **RBAC granular** por site e por permissão.
- Log de auditoria nativo, exportável para SIEM.
- HTTPS obrigatório configurável.
- Content Security Policy compatível.

**Nota C01 (LGPD):** 5. **Nota C02 (Controle):** 5. **Nota C09 (Segurança):** 4 (ausência de certificação de terceiro — inaplicável a self-host).

---

## 7. Custos

TCO marginal SETDIG 5 anos (regime C03 do [`../docs/03-criterios.md`](../docs/03-criterios.md#5-definição-operacional)):

| Rubrica | Valor |
|---------|------:|
| Licença núcleo | R$ 0 |
| **Plugins premium** (Funnels + Heatmaps & Session Recording + Form Analytics + A/B Testing) | R$ 60 k |
| Infra marginal (rateio K8s + DBaaS do parque SETDIG existente) | R$ 30 k |
| Equipe marginal (~0,1 FTE incremental sobre baseline STI) | R$ 75 k |
| Implantação por portal (~R$ 125/portal × 80) | R$ 10 k |
| Adequação LGPD | R$ 30 k |
| Contingência (riscos) | R$ 30 k |
| **Total** | **~R$ 235 k** |

**Custo insensível a volume** — pico de 30× em evento institucional não altera fatura. Comparado a SaaS por PV/evento, previsibilidade orçamentária é qualitativamente superior.

---

## 8. Pontos fortes / fracos

**Fortes:**
- Cobertura funcional integral dos *Must have* + jornada + Tag Manager próprio.
- **Precedente CNIL** — único da matriz com auditoria de DPA.
- Auditoria verificável em 4 camadas (código, banco, tráfego, log).
- SQL direto ao bruto habilita BI sem intermediar API.
- GPL v3 + comunidade diversa → risco de relicenciamento baixo.
- MFA + RBAC granular + auditoria nativos.
- Adoção institucional pública documentada.
- Suporte LTS previsível.

**Fracos:**
- Recursos avançados (heatmap, replay, forms, A/B, roll-up) são **plugins pagos**.
- Ingestão síncrona por padrão (mitigado por QueuedTracking + Redis).
- Gargalo de arquivamento em pico (mitigado por host dedicado + paralelização).
- SSO só via plugins pagos.
- Poucos profissionais no mercado brasileiro.
- Documentação pt-BR parcial.

---

## 9. Quando usar / quando evitar

**Usar:**
- **Plataforma padrão do parque** (EDS + sites gov MS) — recomendação principal.
- Portais institucionais e serviços digitais com exigência de funil, jornada e Tag Manager.
- **Co-instância no Xvia** para audiência anônima cookieless.
- Contextos com veto do DPO sobre transferência internacional.

**Evitar:**
- Portais que exijam product analytics identificado (feature flags, experimentação) — usar PostHog em paralelo.
- Contextos sem capacidade de operar PHP + MySQL + Redis + cron (Matomo Cloud é contingência formal).
- Portais transacionais no Xvia — Matomo cobre a audiência; PostHog cobre o funil identificado.

---

## 10. Notas do avaliador

Melhor combinação disponível de **soberania + conformidade + cobertura funcional** para setor público brasileiro. Precedente CNIL é ativo institucional relevante — dá cobertura à ANPD por analogia. **Sete lacunas técnicas** (L1–L7) do parque atual são mitigáveis por arquitetura, sem trocar produto — trabalho da Onda 2 do roadmap.

Não é a plataforma tecnicamente "mais avançada" — Adobe supera em analítica profunda; PostHog supera em product analytics; GA4 supera em ecossistema. Mas é a que combina os atributos que importam ao setor público **sem deficiência crítica não mitigável**.

**Manter Matomo no parque não é conservadorismo — é a decisão sustentada pela matriz** sob qualquer ponderação testada (4 de 5 cenários com liderança). ADR-002 confirma sob pesos revisados.
