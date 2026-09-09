# Anexo — Dynatrace × PostHog: sobreposição real, decisão pelo modelo

> **Nota explicativa** · Material de apoio à leitura do [ADR-002](../docs/06-adr-002.md). **Não** revisa a decisão vigente (Matomo + PostHog em coexistência no Xvia).
> [← Voltar ao índice](../README.md) · [ADR-002](../docs/06-adr-002.md) · [Contexto — exclusões](../docs/01-contexto.md) · [Ficha PostHog](../plataformas/posthog.md)

> **📌 Enquadramento**
> Dynatrace **tem sim** módulos que cobrem território do PostHog — não é apenas APM. Via **Digital Experience Monitoring (DEM)** e o add-on **Business Analytics**, cobre RUM, session replay, funis, cohorts e conversão. A pergunta correta não é *"faz o mesmo?"* e sim *"em que modelo comercial, com que soberania de dado, e a que custo?"*. É nesse eixo que a decisão do ADR-002 se sustenta.

---

## Sumário

- [1. Enquadramento](#1-enquadramento)
- [2. O que cada ferramenta é — visão de módulos](#2-o-que-cada-ferramenta-é--visão-de-módulos)
- [3. Sobreposição real (não aparente)](#3-sobreposição-real-não-aparente)
- [4. Onde a decisão do ADR-002 realmente se apoia](#4-onde-a-decisão-do-adr-002-realmente-se-apoia)
- [5. Analogia didática](#5-analogia-didática)
- [6. Papel de cada uma na stack do Xvia](#6-papel-de-cada-uma-na-stack-do-xvia)
- [7. O que este documento NÃO responde](#7-o-que-este-documento-não-responde)

---

## 1. Enquadramento

A Secretaria-Executiva de Transformação Digital — SETDIG recebe recorrentemente a pergunta: **"Dynatrace não tem um módulo que faz o mesmo que Matomo/PostHog?"**. Resposta honesta: **tem**. Dynatrace evoluiu de APM puro para uma plataforma de observabilidade que inclui web analytics (RUM), session replay e product analytics de negócio (Business Analytics). O que muda em relação ao PostHog não é *função*, é *modelo* — licenciamento, soberania de dado, cultura de uso e custo.

Este anexo mapeia essa sobreposição de forma objetiva e mostra por que o ADR-002 continua íntegro **mesmo reconhecendo** que Dynatrace atenderia parte dos requisitos funcionais do Xvia.

---

## 2. O que cada ferramenta é — visão de módulos

### 2.1 Dynatrace — plataforma modular

| Módulo | Propósito | Categoria |
|--------|-----------|-----------|
| **APM / OneAgent** | Traces distribuídos, métricas de aplicação, dependência entre serviços | Observabilidade técnica |
| **Infrastructure Monitoring** | Host, container, K8s, cloud | Observabilidade técnica |
| **Log Management** | Ingestão, correlação e busca de logs | Observabilidade técnica |
| **Real User Monitoring (RUM)** | Page views, sessões, dispositivo, geo, Core Web Vitals | **Web analytics** |
| **Session Replay** | Gravação e reprodução de sessão | **UX / debug** |
| **Synthetic Monitoring** | Probes agendadas de disponibilidade e performance | Observabilidade técnica |
| **Digital Experience Monitoring (DEM)** | Guarda-chuva que embala RUM + Session Replay + Synthetic | **Experiência do usuário** |
| **Business Analytics** *(add-on)* | Funis, conversão, cohorts, eventos de negócio via **DQL** sobre **Grail** | **Product analytics** |

### 2.2 PostHog — suite unificada de product analytics

| Módulo | Propósito |
|--------|-----------|
| **Product Analytics** | Eventos, funis, cohorts, retention, paths |
| **Web Analytics** | Page views, referenciadores, campanhas, dispositivos |
| **Session Replay** | Gravação consentida de sessão para UX |
| **Feature Flags** | Rollout progressivo e kill switches |
| **A/B Testing / Experimentos** | Experimentação estatística integrada aos eventos |
| **Surveys** | Coleta de feedback in-app |
| **HogQL** | SQL sobre a base de eventos (ClickHouse) |

Nota: PostHog não faz APM, não faz observabilidade de infra, não faz traces distribuídos. É especialista em comportamento de usuário.

---

## 3. Sobreposição real (não aparente)

Onde as duas cobrem o **mesmo requisito funcional**:

| Capacidade | Dynatrace | PostHog |
|------------|-----------|---------|
| Web analytics (page views, sessões, dispositivo) | RUM | Web Analytics |
| Session replay | Session Replay (módulo dedicado) | Session Replay |
| Funis, conversão, cohorts | Business Analytics (add-on) | Product Analytics (nativo) |
| Query analítica sobre eventos | DQL sobre Grail | HogQL sobre ClickHouse |
| Métricas de experiência (Core Web Vitals) | RUM | Web Analytics |
| Dashboards de negócio | Business Analytics | Product Analytics |

Onde **só uma** cobre:

| Capacidade | Dynatrace | PostHog |
|------------|-----------|---------|
| Traces distribuídos, APM | ✅ | ❌ |
| Observabilidade de infra e K8s | ✅ | ❌ |
| Log management correlacionado | ✅ | ❌ |
| Synthetic monitoring | ✅ | ❌ |
| Feature flags integrados a eventos | ❌ | ✅ |
| Experimentação A/B nativa | ❌ | ✅ |
| Surveys in-app | ❌ | ✅ |

**Leitura:** para o eixo *comportamento de cidadão*, há sobreposição real. Para o eixo *observabilidade técnica*, só Dynatrace. Para o eixo *engenharia de produto* (flags, experimentos, surveys), só PostHog.

---

## 4. Onde a decisão do ADR-002 realmente se apoia

A escolha por PostHog no Xvia **não é** por Dynatrace "não conseguir fazer" — é pelos quatro eixos abaixo:

| Eixo | Dynatrace | PostHog |
|------|-----------|---------|
| **Modelo comercial** | SaaS proprietário; add-ons pagos por DDU (Davis Data Unit); Business Analytics é licença adicional | Open-core (MIT); on-premise gratuito no core |
| **Soberania de dado** | Dado do cidadão hospedado no SaaS do fornecedor (fora do datacenter do Estado, salvo Managed) | Dado do cidadão dentro da infra do Estado (self-host) |
| **Escopo do estudo** | Fora do escopo (SaaS proprietário; APM é camada explicitamente excluída em [`docs/01-contexto.md`](../docs/01-contexto.md) §10) | Dentro do escopo (on-premise gratuito) |
| **Cultura de uso** | Consumido por SRE/DevOps; product analytics é extensão da observabilidade | Consumido por produto e negócio; observabilidade não é o foco |
| **Custo marginal** | Licença comercial não cotada neste estudo | ~R$ 955 k em 5 anos, marginal ao Xvia (ver [`plataformas/posthog.md`](../plataformas/posthog.md)) |

> **⚠️ Bloco de Risco**
> Comparar Dynatrace com PostHog **apenas por funcionalidade** leva à conclusão errada de que são substituíveis. São, no papel. Não são, quando se pesa soberania de dado do cidadão, modelo de licenciamento e o eixo escolhido (on-premise gratuito) do ADR-002.

---

## 5. Analogia didática

> **📌 Observação**
> Ambas as ferramentas conseguem "gravar a viagem do cidadão pelo portal". **Dynatrace** grava porque a observabilidade técnica dele já roda ali e o módulo de negócio (Business Analytics) foi encaixado por cima — dado sai para o SaaS do fornecedor, sob licença comercial. **PostHog** grava porque nasceu para isso — dado fica no datacenter do Estado, sob licença aberta. Mesma gravação, contratos e custos diferentes.

---

## 6. Papel de cada uma na stack do Xvia

> **✅ Bloco de Decisão**
> O ADR-002 permanece vigente. **Matomo** (audiência agregada do parque) + **PostHog** (product analytics do Xvia) atendem o eixo *web + product analytics* dentro de **on-premise gratuito com soberania de dado**. Se, em ciclo futuro, a SETDIG avaliar **APM** para o Xvia, Dynatrace concorreria com outras ferramentas da categoria (New Relic, Datadog, Grafana Stack, Elastic APM) — e a decisão de manter ou não o PostHog em paralelo dependeria de custo total, sobreposição de módulos e integração de dado. Não é decisão deste estudo.

---

## 7. O que este documento NÃO responde

- ❌ **TCO do Dynatrace** — sem projeção; exige cotação formal (OneAgent + DEM + Business Analytics são licenças separadas).
- ❌ **Viabilidade contratual** — enquadramento em ata de registro de preços, Lei nº 14.133/2021, análise jurídica.
- ❌ **Comparativo entre ferramentas de APM** — New Relic × Datadog × Dynatrace × Grafana × Elastic APM. Estudo próprio, fora do escopo atual.
- ❌ **Decisão de adotar APM no Estado** — este anexo é didático e explicativo; não é ADR nem recomendação.
- ❌ **Substituir o ADR-002** — reafirma a decisão, não a revê.

---

## Navegação

| Referência | Onde |
|------------|------|
| Decisão vigente de web analytics | [`docs/06-adr-002.md`](../docs/06-adr-002.md) |
| Exclusões explícitas do escopo (APM, observabilidade) | [`docs/01-contexto.md`](../docs/01-contexto.md) §10 |
| Ficha técnica PostHog | [`plataformas/posthog.md`](../plataformas/posthog.md) |
| Comparativo Matomo × PostHog | [`comparativos/matomo-vs-posthog.md`](../comparativos/matomo-vs-posthog.md) |
