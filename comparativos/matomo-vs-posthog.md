# Comparativo — Matomo × PostHog: complementaridade, não competição

> **Corte transversal** · Reposicionamento pós-[ADR-002](../docs/06-adr-002.md): as duas plataformas atendem eixos distintos e são adotadas em coexistência no portal Xvia.
> [← Voltar ao índice](../README.md) · [ADR-002](../docs/06-adr-002.md) · [Coexistência no Xvia](coexistencia-matomo-posthog.md) · [Matomo](../plataformas/matomo.md) · [PostHog](../plataformas/posthog.md)

> **📌 Enquadramento**
> Matomo e PostHog cobrem **domínios distintos** (web analytics tradicional × product analytics) e são adotadas **em coexistência no portal Xvia**. Comparativo binário. Análise histórica de custo (Matomo ~R$ 235 k × PostHog ~R$ 955 k marginais em 5 anos) preservada em `../anexos/historico/comparativos/custo.md`.

---

## Sumário

- [1. Contexto](#1-contexto)
- [2. Papéis complementares](#2-papéis-complementares)
- [3. Sobreposição funcional](#3-sobreposição-funcional)
- [4. Modelo de coexistência no Xvia](#4-modelo-de-coexistência-no-xvia)
- [5. Governança de dado duplicado](#5-governança-de-dado-duplicado)
- [6. Matriz de decisão — qual dado responder onde](#6-matriz-de-decisão--qual-dado-responder-onde)
- [7. O que este comparativo NÃO responde](#7-o-que-este-comparativo-não-responde)

---

## 1. Contexto

O parque de sites do Governo do Estado de MS adota **Matomo On-Premise** como plataforma padrão desde o ADR-001, confirmado no ADR-002 sob pesos revisados (500/600, 83,3 %, líder). O novo portal Xvia — superapp cidadão — adiciona um perfil de uso que Matomo cobre parcialmente: jornada de usuário autenticado, funil de serviços transacionais, feature flags, experimentação estruturada e session replay consentido em serviços de alta complexidade.

Esse perfil é o núcleo do domínio de **product analytics**, no qual o PostHog é referência de mercado. O ADR-002 §6.2 formaliza a adoção do PostHog auto-hospedado como camada complementar do Xvia — não como substituto do Matomo.

**Este documento explica por que as duas plataformas convivem, o que cada uma responde, e como coordenar a operação de ambas sem duplicação de esforço ou de dado.**

---

## 2. Papéis complementares

### 2.1 Matomo — web analytics tradicional

**O que faz melhor:**

- Audiência agregada e anônima (page views, visitantes únicos, sessões, referenciadores, campanhas UTM).
- Métricas de conteúdo (páginas mais vistas, tempo médio, taxa de rejeição por página).
- SEO e canais de aquisição (busca orgânica, redes sociais, tráfego direto).
- Base legal cookieless simplificada, com precedente CNIL — dispensa consentimento em configuração conforme.
- Roll-up multi-site (agregação de dezenas de portais em uma visão consolidada).
- Tag Manager nativo, independente do GTM do Google.

**Público-alvo:** SGD, equipes de comunicação, gestores de portal, órgãos setoriais que precisam mensurar alcance.

### 2.2 PostHog — product analytics

**O que faz melhor:**

- Jornada identificada de usuário autenticado (funil por etapa, tempo entre eventos, onde abandona).
- Coortes de usuários (comportamento comparado entre segmentos identificados).
- Retenção (usuários que voltam após N dias).
- Feature flags e experimentação (A/B testing estruturado, rollout gradual).
- Session replay consentido em serviços de alta complexidade (com mascaramento agressivo por padrão).
- HogQL — consulta SQL direta sobre ClickHouse para análise ad-hoc rápida.
- SDKs modernos (JavaScript, Python, Node, Go, iOS, Android, React Native).

**Público-alvo:** time do produto Xvia, engenharia de serviços digitais, DPO (para RIPD do session replay), gestores de serviços transacionais.

### 2.3 Resumo dos papéis

| Pergunta que se quer responder | Ferramenta |
|--------------------------------|-----------|
| "Quantas pessoas visitaram o portal Xvia este mês?" | Matomo |
| "De onde vem o tráfego do Xvia?" | Matomo |
| "Quais páginas do Xvia têm maior tempo médio?" | Matomo |
| "Qual serviço tem a maior taxa de conversão no Xvia?" | PostHog |
| "Onde o cidadão abandona o pedido de segunda-via?" | PostHog |
| "A nova versão do formulário aumentou conclusão?" | PostHog (feature flag + experimentação) |
| "Este cidadão voltou ao Xvia após 30 dias?" | PostHog (retenção identificada) |
| "Grave a sessão deste serviço crítico para diagnóstico" | PostHog (com consentimento e mascaramento) |
| "Qual é o dado consolidado de audiência dos 80 portais?" | Matomo (roll-up) |

---

## 3. Sobreposição funcional

Nem tudo é papel exclusivo — há sobreposição em algumas capacidades. Nesses casos, a regra é **"quem responde é quem tem a fonte primária do evento":**

| Capacidade | Matomo | PostHog | Regra no Xvia |
|-----------|:------:|:-------:|---------------|
| Page views | ✅ | ✅ | **Matomo** — mais barato e é o número oficial de audiência |
| Sessões | ✅ | ✅ | **Matomo** |
| Eventos customizados | ✅ (plugin) | ✅ (nativo) | **PostHog** — SDK mais rico, HogQL, integração com feature flags |
| Metas / conversões | ✅ | ✅ | **Ambos**, sem duplicação: Matomo para metas de audiência; PostHog para conversões identificadas |
| Funis | ✅ (plugin pago) | ✅ (nativo) | **PostHog** para funis identificados; **Matomo** para funis anônimos de conteúdo |
| Heatmaps | ✅ (plugin pago) | ⚠️ (via session replay) | **Matomo** — mais barato e não exige consentimento adicional |
| Session recording | ✅ (plugin pago) | ✅ (nativo) | **PostHog no Xvia** (consentido + mascarado); **vedado no parque** por padrão |
| A/B testing | ✅ (plugin pago) | ✅ (nativo) | **PostHog** — feature flags integradas |
| Coortes / retenção | ⚠️ (limitada) | ✅ | **PostHog** |
| Retenção anônima | ✅ | ⚠️ | **Matomo** |

---

## 4. Modelo de coexistência no Xvia

Detalhamento operacional em [`coexistencia-matomo-posthog.md`](coexistencia-matomo-posthog.md). Resumo:

1. **Ambos os SDKs carregados em paralelo** no bootstrap do portal Xvia — carregamento async/defer para minimizar impacto no LCP.
2. **Segmentação de eventos por SDK** — cada capability de tracking é instrumentada em uma única ferramenta (evita eventos duplicados e reconciliação impossível).
3. **Política de retenção coordenada** — mesma janela de retenção (12 meses de dado bruto, 60 meses de agregados) em ambos os produtos, para não gerar viés temporal.
4. **Consent Manager único** — coleta de consentimento centralizada; PostHog session replay só ativa após opt-in explícito por serviço.
5. **Reconciliação mensal** — SGD compara métricas equivalentes (page views, sessões) entre as duas plataformas; divergência ≤ 15 % é aceita, > 15 % aciona análise.

---

## 5. Governança de dado duplicado

O maior risco de operar 2 ferramentas em paralelo é a **duplicidade descontrolada**. Estratégia:

| Regra | Detalhe |
|-------|---------|
| **Fonte primária declarada por métrica** | Documento de segmentação define qual ferramenta é fonte oficial para cada métrica; a outra é auxiliar |
| **Sem replicação cega** | Não replicar todos os eventos em ambas as ferramentas — a duplicação só existe onde a análise em ambas agrega valor |
| **Retenção sincronizada** | Mesma janela em ambas para evitar consulta em plataforma com dado mais antigo ou mais recente que a outra |
| **DPIA único integrado** | Um DPIA que cobre a coexistência, não dois RIPD separados |
| **Dashboards consolidados no BI corporativo** | Cards do dashboard executivo marcam a fonte (Matomo / PostHog) e a data da última atualização |
| **Governança operacional** | Comitê de analytics (Onda 4 do roadmap) inclui representantes do time Xvia; reuniões trimestrais revisam divergências e ajustes de segmentação |

---

## 6. Matriz de decisão — qual dado responder onde

Referência rápida para times do Xvia decidirem em qual ferramenta instrumentar uma nova capability:

```
┌─────────────────────────────────────────────────────────┐
│ O evento é sobre usuário autenticado?                   │
│                                                          │
│  SIM  →  PostHog (funil identificado, cohort, retenção) │
│  NÃO  →  Continua abaixo                                │
└─────────────────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────┐
│ O evento vai ser usado em feature flag ou experimento?  │
│                                                          │
│  SIM  →  PostHog (integração nativa)                    │
│  NÃO  →  Continua abaixo                                │
└─────────────────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────┐
│ O evento é agregação de audiência (page view, sessão,   │
│ referenciador, campanha)?                               │
│                                                          │
│  SIM  →  Matomo (fonte oficial de audiência)            │
│  NÃO  →  Continua abaixo                                │
└─────────────────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────┐
│ É evento customizado de negócio (clique em CTA, download, │
│ scroll profundo)?                                       │
│                                                          │
│  SIM  →  PostHog (SDK mais rico + HogQL)                │
│  NÃO  →  Consultar Arquitetura antes de instrumentar    │
└─────────────────────────────────────────────────────────┘
```

---

## 7. O que este comparativo NÃO responde

- **Custo comparativo.** Não é mais critério de decisão. Referência em `../anexos/historico/comparativos/custo.md`.
- **Escolha entre Matomo Cloud e PostHog Cloud EU.** Contingências formais em [ADR-002 §6.5](../docs/06-adr-002.md#6-decisão), acionadas se P2 falhar.
- **Operação isolada.** Coberta nas fichas [`../plataformas/matomo.md`](../plataformas/matomo.md) e [`../plataformas/posthog.md`](../plataformas/posthog.md).
- **Overhead de LCP no Xvia.** [ADR-002 §5.T1](../docs/06-adr-002.md#5-trade-offs) e risco R-17 em [`../docs/07-recomendacao.md`](../docs/07-recomendacao.md#10-riscos).

---

## Navegação

| Documento | Papel |
|-----------|-------|
| [ADR-002](../docs/06-adr-002.md) | Decisão formal da coexistência no Xvia |
| [Coexistência Matomo + PostHog no Xvia](coexistencia-matomo-posthog.md) | Detalhamento operacional |
| [Matomo](../plataformas/matomo.md) | Ficha técnica |
| [PostHog](../plataformas/posthog.md) | Ficha técnica |
