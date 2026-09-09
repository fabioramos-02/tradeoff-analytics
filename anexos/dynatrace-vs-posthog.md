# Anexo — Dynatrace × PostHog: categorias distintas, não substitutos

> **Nota explicativa** · Material de apoio à leitura do [ADR-002](../docs/06-adr-002.md). **Não** revisa a decisão vigente (Matomo + PostHog em coexistência no Xvia).
> [← Voltar ao índice](../README.md) · [ADR-002](../docs/06-adr-002.md) · [Contexto — exclusões](../docs/01-contexto.md) · [Ficha PostHog](../plataformas/posthog.md)

> **📌 Enquadramento**
> Dynatrace e PostHog **não competem**. Resolvem problemas de categorias diferentes: Dynatrace é **APM** (observabilidade de aplicação e infraestrutura); PostHog é **product analytics** (comportamento de usuário). Este anexo esclarece a diferença de forma objetiva para stakeholders — não altera o escopo do estudo, que exclui APM em [`docs/01-contexto.md`](../docs/01-contexto.md) §10.

---

## Sumário

- [1. Enquadramento](#1-enquadramento)
- [2. Categorias distintas](#2-categorias-distintas)
- [3. Analogia didática](#3-analogia-didática)
- [4. Sobreposição aparente](#4-sobreposição-aparente)
- [5. Papel de cada uma na stack do Xvia](#5-papel-de-cada-uma-na-stack-do-xvia)
- [6. O que este documento NÃO responde](#6-o-que-este-documento-não-responde)

---

## 1. Enquadramento

A Secretaria-Executiva de Transformação Digital — SETDIG recebe recorrentemente a pergunta: **"Dynatrace substitui PostHog?"** ou **"Por que não usar Dynatrace no lugar de PostHog no Xvia?"**. Este documento responde de forma direta: as ferramentas resolvem problemas distintos e podem coexistir em stacks maduras — a escolha de uma **não** exclui a outra.

A decisão de web analytics do Estado está formalizada no [ADR-002](../docs/06-adr-002.md): Matomo no parque + PostHog no Xvia. Dynatrace, se avaliado no futuro, entra em outra camada arquitetural (APM), com processo decisório próprio.

---

## 2. Categorias distintas

| Dimensão | **Dynatrace** | **PostHog** |
|----------|---------------|-------------|
| Categoria | APM + Observabilidade full-stack | Product Analytics + Web Analytics |
| Pergunta que responde | *"Por que o serviço está lento? Onde está o erro?"* | *"Como o cidadão usa o serviço? Onde ele desiste?"* |
| O que instrumenta | Traces distribuídos, métricas de infra, logs de aplicação, RUM | Eventos de negócio, funis, cohorts, sessões de usuário |
| Unidade de análise | Transação técnica, request, span, host | Usuário/sessão, evento de negócio |
| Público consumidor | SRE, DevOps, time de infra, engenharia de plataforma | Produto, UX, negócio, gestão do serviço |
| Modelo comercial | SaaS proprietário (também Managed on-premise); licença comercial por host-unit / DEM | Open-core (MIT + EE proprietária); on-premise gratuito no escopo do estudo |

---

## 3. Analogia didática

> **📌 Observação**
> Pense num carro sob telemetria. **Dynatrace é o painel do motor**: temperatura, rotação, falha no injetor, código de erro do computador de bordo — para o mecânico agir. **PostHog é o GPS de viagem**: quais rotas o motorista pega, onde para, onde desiste do trajeto — para quem desenha o percurso melhorar a experiência. Ambos observam o mesmo carro; nenhum resolve o problema do outro.

---

## 4. Sobreposição aparente

Há três capacidades onde parece haver sobreposição — mas o **objetivo** de cada ferramenta muda o valor entregue:

| Capacidade | Dynatrace (objetivo: técnico) | PostHog (objetivo: comportamental) |
|------------|-------------------------------|-------------------------------------|
| **Session replay / RUM** | Reproduz sessão para investigar erro, exceção JS, gargalo de front-end | Reproduz sessão para entender jornada UX, ponto de abandono, dificuldade de fluxo |
| **Dashboards e alertas** | Métricas técnicas: latência p95, taxa de erro, saturação de CPU/memória | Métricas de negócio: conversão, retenção, adoção de funcionalidade |
| **Real User Monitoring** | Mede performance percebida (Core Web Vitals, tempo de carga por rota) | Mede engajamento e conversão por rota, feature flags, experimentos A/B |

Ferramentas diferentes podem produzir artefato visual parecido (uma "gravação de sessão"), mas servem a decisões distintas — uma vai ao time de engenharia; outra, ao time de produto e serviço.

---

## 5. Papel de cada uma na stack do Xvia

> **✅ Bloco de Decisão**
> O ADR-002 permanece vigente e íntegro: **Matomo** (audiência agregada do parque) + **PostHog** (product analytics do Xvia) atendem o eixo de **web/product analytics**. Se, em ciclo futuro, a SETDIG avaliar adoção de **APM** para o Xvia ou para o parque, Dynatrace concorreria com outras ferramentas de sua categoria (New Relic, Datadog, Grafana Stack, Elastic APM) — e conviveria com PostHog, sem substituí-lo. APM e product analytics são **camadas complementares**, não alternativas.

---

## 6. O que este documento NÃO responde

- ❌ **TCO do Dynatrace** — não há projeção de custo neste anexo; Dynatrace é SaaS licenciado, exigiria cotação formal.
- ❌ **Viabilidade contratual** — enquadramento em ata de registro de preços, compatibilidade com Lei nº 14.133/2021, análise jurídica.
- ❌ **Comparativo entre ferramentas de APM** — New Relic × Datadog × Dynatrace × Grafana × Elastic APM. É estudo próprio, fora do escopo atual.
- ❌ **Decisão de adotar APM no Estado** — este anexo é didático e explicativo; não é ADR nem recomendação.

---

## Navegação

| Referência | Onde |
|------------|------|
| Decisão vigente de web analytics | [`docs/06-adr-002.md`](../docs/06-adr-002.md) |
| Exclusões explícitas do escopo (APM, observabilidade) | [`docs/01-contexto.md`](../docs/01-contexto.md) §10 |
| Ficha técnica PostHog | [`plataformas/posthog.md`](../plataformas/posthog.md) |
| Comparativo Matomo × PostHog | [`comparativos/matomo-vs-posthog.md`](../comparativos/matomo-vs-posthog.md) |
