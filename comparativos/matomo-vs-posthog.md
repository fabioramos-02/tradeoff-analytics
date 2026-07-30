# Comparativo bilateral — Matomo On-Premise × PostHog auto-hospedado

> **Corte transversal** · Duelo direto entre os dois candidatos auto-hospedados de maior pontuação
> **Motivação:** dirimir viés de "usei PostHog Cloud, quero adotar auto-hospedado"
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/07-matriz-decisao.md) · [Matomo](../plataformas/matomo.md) · [PostHog](../plataformas/posthog.md)

---

## Sumário

- [1. Contexto e escopo](#1-contexto-e-escopo)
- [2. Resposta direta — PostHog Cloud ≠ PostHog OSS auto-hospedado](#2-resposta-direta--posthog-cloud--posthog-oss-auto-hospedado)
- [3. Comparativo funcional lado a lado](#3-comparativo-funcional-lado-a-lado)
- [4. Comparativo operacional](#4-comparativo-operacional)
- [5. Comparativo de custo (regime marginal SETDIG)](#5-comparativo-de-custo-regime-marginal-setdig)
- [6. Segurança, LGPD e soberania](#6-segurança-lgpd-e-soberania)
- [7. Escalabilidade](#7-escalabilidade)
- [8. APIs, SDKs e integrações](#8-apis-sdks-e-integrações)
- [9. Governança e lock-in](#9-governança-e-lock-in)
- [10. Matriz-resumo — quem vence em cada critério](#10-matriz-resumo--quem-vence-em-cada-critério)
- [11. Quando escolher qual](#11-quando-escolher-qual)
- [12. Recomendação circunstanciada](#12-recomendação-circunstanciada)
- [13. Antipadrão a evitar — extrapolar experiência de Cloud para OSS](#13-antipadrão-a-evitar--extrapolar-experiência-de-cloud-para-oss)

---

## 1. Contexto e escopo

Este documento responde a uma pergunta específica: **"O PostHog é atraente na modalidade Cloud. Se auto-hospedado, mantém o mesmo valor entregue?"**

Escopo:

- Comparação **bilateral** — Matomo On-Premise × PostHog **OSS auto-hospedado**.
- Regime de custo: **marginal SETDIG** (P1/P2/R4). Ver [`custo.md §1.4`](custo.md#14-regime-de-custo--marginal-para-o-estado).
- Modalidade PostHog Cloud entra apenas como **referência** para evidenciar a diferença OSS × Cloud.
- Não é revisão do ADR — o resultado da matriz permanece: **Matomo OP recomendado, PostHog não selecionado**. Este documento **fundamenta o "não"** de forma detalhada.

---

## 2. Resposta direta — PostHog Cloud ≠ PostHog OSS auto-hospedado

> **🚨 Alerta — a experiência da Cloud não se replica integralmente no OSS**
>
> **Não.** PostHog auto-hospedado **não entrega a mesma experiência** da modalidade Cloud. Três razões objetivas:
>
> 1. **Recursos Enterprise (EE) são pagos e separados.** SSO SAML, RBAC granular, políticas de retenção configuráveis e auditoria completa **não estão no core MIT**. Requerem licença EE comercial — mesmo em deployment auto-hospedado. Fonte: [`plataformas/posthog.md §2`](../plataformas/posthog.md#2-licenciamento).
> 2. **Docker Compose é oficialmente desencorajado em produção.** A própria PostHog declara: *"the hobby deploy is for evaluation only; for production, use Kubernetes with our Helm chart"*. Isso muda o custo operacional real. Fonte: [`plataformas/posthog.md §3`](../plataformas/posthog.md#3-hospedagem).
> 3. **Divergência crescente entre Cloud e OSS.** Recursos novos aparecem primeiro (e às vezes exclusivamente) na Cloud. A comunidade e a documentação concentram-se na Cloud; o *self-hosting* recebe menos atenção do fornecedor. Fonte: [`docs/07-matriz-decisao.md §4.6`](../docs/07-matriz-decisao.md#46-posthog-auto-hospedado--455-pontos) — C04 justificativa.

### 2.1 O que você tem no OSS

Core em MIT, integralmente funcional em modo auto-hospedado:

- Product analytics (eventos, funis, cohorts, retenção, jornada)
- Session replay
- Feature flags e experimentos (A/B testing)
- Surveys
- Data warehouse sync
- HogQL (SQL sobre ClickHouse)
- LLM observability
- SDKs em todas as linguagens principais
- Ingestão pelo mesmo `posthog-js` da Cloud

### 2.2 O que você NÃO tem sem EE paga

- **SSO SAML** (SETDIG exige por norma corporativa) → requer EE
- **RBAC granular** por projeto/dashboard/feature → requer EE
- **Políticas de retenção configuráveis** por evento → requer EE
- **Auditoria completa** com trilha exportável → requer EE
- **Custom event ingestion** avançada → parcialmente EE

### 2.3 O que você NÃO tem em modo nenhum auto-hospedado

- **Suporte comercial** ao stack auto-hospedado (o fornecedor prioriza Cloud)
- **SLA financeiro**
- **Managed upgrades** — cada release major requer intervenção manual em Kafka/ClickHouse
- **Managed backups** com restore-drill contratual
- **Managed scaling** — expansão de ClickHouse é operação manual

---

## 3. Comparativo funcional lado a lado

| Funcionalidade | Matomo OP + plugins | PostHog OSS (core MIT) | PostHog Cloud |
|---------------|:-------------------:|:----------------------:|:-------------:|
| **Analytics de audiência (page views, sessões, visitantes)** | 🟢 Nativo, primeira classe | 🟠 Via eventos `$pageview` | 🟠 Via eventos `$pageview` |
| **Funis de conversão** | 🟢 Plugin *Funnels* (pago) | 🟢 Nativo | 🟢 Nativo |
| **Cohorts** | 🟠 Segmentação | 🟢 Nativo | 🟢 Nativo |
| **Retenção** | 🟠 Limitada | 🟢 Nativo | 🟢 Nativo |
| **User journey / paths** | 🟢 *Users Flow* nativo | 🟢 Path analysis | 🟢 Path analysis |
| **Heatmaps** | 🟢 Plugin *Heatmaps & Session Recording* (pago) | 🟢 Nativo (via session replay) | 🟢 Nativo |
| **Session recording** | 🟢 Mesmo plugin (pago) | 🟢 Nativo | 🟢 Nativo |
| **A/B testing / experimentation** | 🟠 Plugin *A/B Testing* (pago) | 🟢 Nativo com significância estatística | 🟢 Nativo |
| **Feature flags** | ❌ | 🟢 Nativo | 🟢 Nativo |
| **Surveys in-app** | ❌ | 🟢 Nativo | 🟢 Nativo |
| **Form analytics** | 🟢 Plugin *Form Analytics* (pago) | 🟠 Via eventos customizados | 🟠 Via eventos customizados |
| **Tag Manager próprio** | 🟢 *Matomo Tag Manager* nativo | ❌ | ❌ |
| **Real time** | 🟢 | 🟢 | 🟢 |
| **LLM observability** | ❌ | 🟢 Nativo | 🟢 Nativo |
| **HogQL (SQL analítico)** | 🟠 SQL direto no MariaDB | 🟢 SQL colunar ClickHouse | 🟢 Nativo |
| **Data warehouse sync** | 🟠 Manual via export | 🟢 Nativo (S3, BigQuery, Snowflake) | 🟢 Nativo |
| **Consent Manager próprio** | ❌ | ❌ | ❌ |
| **Painel público de estatísticas** | 🟢 Nativo (widget) | ❌ | ❌ |
| **SSO SAML** | 🟠 Plugin comercial | ❌ (só na EE paga) | 🟢 (EE incluída) |
| **RBAC granular** | 🟢 Nativo (por site + por permissão) | ❌ (só na EE paga) | 🟢 (EE incluída) |
| **Auditoria exportável para SIEM** | 🟠 Plugin comunidade | ❌ (só na EE paga) | 🟢 (EE incluída) |

**Leitura:** funcionalmente, o PostHog OSS **ganha** em product analytics (funis, cohorts, retenção, replay, feature flags, surveys, LLM obs) e **perde** em governança corporativa (SSO, RBAC, auditoria). O Matomo entrega cobertura ampla, mas empurra recursos avançados para plugins pagos.

### 3.1 A pergunta certa não é "quem tem mais features"

**A pergunta certa é: quais features você realmente vai usar?**

- Portais **institucionais e de conteúdo** (grande parte do parque estadual): audiência, campanhas, tempo de permanência, origem do tráfego, funis simples de serviço. Matomo é purpose-built pra isso.
- Serviços digitais **transacionais complexos** (raros no parque): feature flags, A/B testing com significância, cohorts de comportamento, retenção de longo prazo. PostHog é purpose-built pra isso.

Adotar PostHog para o primeiro caso é **matar mosquito com bazuca**: paga complexidade de plataforma que não converte em valor operacional.

---

## 4. Comparativo operacional

### 4.1 Stack de dependências

| Componente | Matomo OP | PostHog OSS |
|-----------|:---------:|:-----------:|
| Linguagem principal | PHP 8+ | Python (Django) + Rust (plugin-server) |
| Servidor web | Nginx / Apache | Ingress + múltiplos serviços |
| Banco relacional (metadados) | — (tudo no MySQL) | PostgreSQL 15+ |
| Banco analítico | MySQL 8 / MariaDB 10.5+ | **ClickHouse** (obrigatório) |
| Cache | Redis (opcional) | Redis (obrigatório) |
| Streaming / fila | Redis + QueuedTracking (plugin) | **Kafka + Zookeeper** (obrigatório) |
| Object storage | Opcional (só para exports) | **MinIO / S3** (session replay) |
| Job scheduler | Cron (`console core:archive`) | Múltiplos workers + Celery |
| **Total de componentes obrigatórios** | **2** (PHP + MySQL) | **6** (Django + PG + CH + Kafka + Redis + MinIO) |

### 4.2 Modalidade de implantação recomendada

| Modalidade | Matomo OP | PostHog OSS |
|-----------|:---------:|:-----------:|
| Docker Compose | 🟢 Produção viável | 🚨 **Desencorajado oficialmente** para produção |
| Docker + orquestração manual | 🟢 | 🟠 |
| Kubernetes + Helm | 🟢 (chart comunidade) | 🟢 **Único caminho produtivo suportado** |
| Cluster dedicado | 🟠 Só em volume muito alto | 🟢 Recomendado |

### 4.3 Esforço de operação intrínseco (regra C10)

| Aspecto | Matomo OP | PostHog OSS |
|---------|-----------|-------------|
| Setup inicial | 8–24 h | 40–120 h |
| Curva de aprendizado técnico | 2–3 semanas (Linux + PHP + MySQL) | 4–8 semanas (Kubernetes + ClickHouse + Kafka) |
| FTE intrínseco em regime permanente | ~0,3 FTE | **~0,8 FTE** |
| Complexidade de backup | 🟢 Baixa (dump MySQL) | 🔴 Alta (coordenar dump PG + snapshot CH + backup MinIO) |
| Complexidade de DR | 🟢 Baixa | 🔴 Alta (replicação cross-region do ClickHouse) |
| Cadência de upgrades majors | Semestral | Trimestral (releases mais frequentes) |
| Suporte comercial disponível | 🟢 InnoCraft (contratos) | 🔴 **Não** para auto-hospedado |
| **Nota C10 no estudo** | **3** | **1** |

**Consequência direta:** PostHog OSS demanda equipe **especializada** em ClickHouse + Kafka + Kubernetes. Isso viola a Restrição R5 (sem ampliação de quadro STI) mesmo em regime de custo marginal.

---

## 5. Comparativo de custo (regime marginal SETDIG)

Detalhamento em [`custo.md §4.1`](custo.md#41-matomo-on-premise-núcleo-sem-plugins-pagos) e [`custo.md §4.5`](custo.md#45-posthog-auto-hospedado-oss--ee-mínima).

| Rubrica (5 anos) | Matomo OP + plugins | PostHog OSS + EE mínima |
|------------------|--------------------:|------------------------:|
| Licenciamento | R$ 60.000 (plugins Marketplace) | **R$ 300.000** (EE para SSO+RBAC) |
| Infra marginal | R$ 30.000 (dentro do parque K8s+DBaaS SETDIG) | **R$ 300.000** (Kafka+CH+MinIO fora do padrão) |
| Equipe marginal | R$ 75.000 (~0,1 FTE) | **R$ 240.000** (~0,4 FTE) |
| Implantação (snippet) | R$ 10.000 | R$ 15.000 |
| Adequação LGPD | R$ 30.000 | R$ 40.000 |
| Riscos operacionais | R$ 30.000 | R$ 60.000 |
| **TCO 5 anos (marginal)** | **R$ 235.000** | **R$ 955.000** |
| **Razão de custo** | **1,0×** | **≈ 4,1× o Matomo** |

### 5.1 Por que o PostHog OSS tem abatimento parcial, não total

- **Kafka:** o parque SETDIG não opera Kafka gerenciado hoje. Provisionar Kafka em produção com HA é custo real de infra.
- **ClickHouse dedicado:** SETDIG tem ClickHouse para outros projetos, mas mesclar cargas heterogêneas é operacionalmente arriscado. Instância dedicada é o cenário realista.
- **MinIO / S3 para session replay:** volume grande (~150 GB/mês com replay ativo em 100k sessões). O parque cobre object storage, mas o volume marginal é significativo.
- **Curva de expertise ClickHouse + Kafka:** o time SETDIG tem baseline MySQL/PostgreSQL. Adquirir competência em ClickHouse operacional é custo real de capacitação.

### 5.2 Sensibilidade

Se o Estado já operasse Kafka + ClickHouse gerenciados como serviço interno padrão (não é o caso), o TCO marginal do PostHog cairia para ~R$ 600 k — ainda 2,5× o Matomo.

---

## 6. Segurança, LGPD e soberania

| Item | Matomo OP | PostHog OSS |
|------|:---------:|:-----------:|
| **Auto-hospedado no Estado** | 🟢 Sim | 🟢 Sim |
| **Sem transferência internacional** | 🟢 | 🟢 |
| **Base legal viável sem consentimento** | 🟢 Precedente CNIL para configuração cookieless | 🟠 Produto é orientado a identificação — padrão de fábrica não é privacy-first |
| **MFA nativo** | 🟢 TOTP nativo | 🟠 Básico |
| **SSO SAML** | 🟠 Plugin comercial | 🔴 **Só na EE paga** |
| **RBAC granular** | 🟢 Nativo | 🔴 **Só na EE paga** |
| **Auditoria exportável** | 🟠 Plugin comunidade | 🔴 **Só na EE paga** |
| **Retenção configurável** | 🟢 Configuração nativa | 🟢 (mais granular na EE) |
| **Anonimização de IP** | 🟢 `anonymize_ip=full` nativo | 🟠 Requer trabalho ativo |
| **Session replay com dado sensível** | 🟠 Plugin, configuração explícita | 🟠 Ativo por padrão; requer mascaramento manual |
| **DPIA — esforço** | 🟢 Baixo (cookieless) | 🟠 Médio (product analytics gera identificação) |
| **Precedente favorável de DPA europeia** | 🟢 Sim (CNIL) | ❌ Não |
| **Nota C09 (Segurança) no estudo** | **4** | **4** |

Nas dimensões LGPD críticas do Estado, **Matomo tem vantagem estrutural**: precedente formal + configuração default privacy-first + SSO viável sem custo adicional de EE.

---

## 7. Escalabilidade

| Aspecto | Matomo OP | PostHog OSS |
|---------|:---------:|:-----------:|
| Banco (modelo) | MySQL/MariaDB — row-based | ClickHouse — colunar OLAP |
| Volume comprovado | > 1 bilhão de ações/mês (Comissão Europeia) | > 10 bilhões de eventos/mês (cases do fornecedor) |
| Volume projetado do Estado no ano 5 | ~60 M ações/mês | Mesmo |
| Gargalo estrutural | Arquivamento (`archive.php`) sob alto volume | Complexidade Kafka + ClickHouse |
| Escala horizontal | 🟠 Requer construção (réplicas, LB, sessão externalizada) | 🟢 Nativa em K8s |
| Nota C08 no estudo | 3 | 3 |

**Conclusão de escala:** para o envelope do parque estadual (~60 M ações/mês no ano 5), **ambos escalam com folga**. Diferença material só apareceria acima de ~500 M eventos/mês — cenário federal, não estadual.

**Escalar não é o critério de escolha entre os dois.** Ambos cabem no problema. A escolha é ditada por custo, operação e cobertura funcional apropriada ao caso de uso governamental.

---

## 8. APIs, SDKs e integrações

| Dimensão | Matomo OP | PostHog OSS |
|----------|:---------:|:-----------:|
| REST API | 🟢 Reporting API estável | 🟢 v1 estável |
| GraphQL | ❌ | 🟠 HogQL (SQL, não GraphQL formal) |
| SDKs oficiais | JS, PHP, iOS, Android, Java | JS, Python, Node, Ruby, Go, PHP, iOS, Android, Flutter, RN, .NET |
| Webhooks | 🟠 Via plugin | 🟢 Nativos |
| Acesso SQL direto ao dado bruto | 🟢 MariaDB (padrão do mercado) | 🟢 ClickHouse via HogQL |
| Rate limits | Configuráveis (sem hard limit) | 240 req/min por token |
| Documentação API | 🟢 Extensa | 🟢 Extensa |
| **Conectores nativos de BI** | 🟢 Conector oficial Power BI + acesso direto MySQL | 🟠 Warehouse sink (S3/BQ/Snowflake) + acesso direto ClickHouse |
| **Nota C06 (APIs) no estudo** | **4** | **4** |
| **Nota C07 (Integrações) no estudo** | **4** | **4** |

**Empate técnico em API superficial.** Vantagem funcional do PostHog em SDKs modernos (mais linguagens oficiais). Vantagem do Matomo em conectores nativos de BI e familiaridade MySQL do time SETDIG.

---

## 9. Governança e lock-in

| Dimensão | Matomo OP | PostHog OSS |
|----------|:---------:|:-----------:|
| Licença do core | **GPL v3** (copyleft forte) | **MIT** (permissiva) + **EE proprietária** |
| Fork viável | 🟢 Sim, código integralmente aberto | 🟠 Fork do core possível; EE fechada |
| Governança | InnoCraft (mantenedor comercial único) | PostHog Inc. (mantenedor comercial único) |
| Fornecedor concentrado em qual modalidade? | Equilibrado (Cloud + On-Prem tratados) | **Concentrado na Cloud**; auto-hospedado deprioriazado |
| Adoção institucional pública | 🟢 Comissão Europeia (Europa Analytics) | ⚠️ Muito escassa — produto focado em SaaS/startup |
| Portabilidade de dado | 🟢 SQL direto MariaDB | 🟢 Warehouse sink |
| Custo estimado de saída | R$ 20 k | R$ 60 k (mais componentes, mais integrações) |
| **Nota C02 (Controle dos dados)** | **5** | **5** |
| **Nota C04 (Independência tecnológica)** | **5** | **3** |

**Diferença crítica em C04:** GPL v3 vs MIT+EE. A licença MIT permite (e o histórico do mercado mostra) que versões futuras se tornem proprietárias. GPL v3 impede juridicamente esse movimento.

Precedente relevante: Elastic, Redis, MongoDB e outros fizeram exatamente esse movimento de MIT/Apache → licenças proprietárias nos últimos anos. **PostHog está exposta ao mesmo vetor**; Matomo não.

---

## 10. Matriz-resumo — quem vence em cada critério

| Critério | Peso | Matomo OP | PostHog OSS | Vencedor |
|----------|:----:|:---------:|:-----------:|----------|
| C01 — LGPD | 15 | 5 | 4 | 🟢 Matomo (+15) |
| C02 — Controle dos dados | 15 | 5 | 5 | Empate |
| C03 — TCO (marginal) | 15 | 5 | 3 | 🟢 Matomo (+30) |
| C04 — Independência tecnológica | 10 | 5 | 3 | 🟢 Matomo (+20) |
| C05 — Recursos analíticos | 10 | 4 | 5 | 🔵 PostHog (−10) |
| C06 — APIs | 10 | 4 | 4 | Empate |
| C07 — Integrações | 10 | 4 | 4 | Empate |
| C08 — Escalabilidade | 10 | 3 | 3 | Empate |
| C09 — Segurança | 10 | 4 | 4 | Empate |
| C10 — Operação | 5 | 3 | 1 | 🟢 Matomo (+10) |
| C11 — Comunidade | 5 | 4 | 4 | Empate |
| C12 — Documentação | 5 | 4 | 4 | Empate |
| **Total** | 120 | **520** | **455** | 🟢 **Matomo (+65)** |

**Leitura:** o PostHog OSS vence isoladamente em **um único critério** (C05, cobertura funcional avançada), com margem de 10 pontos. O Matomo vence em **quatro critérios** — três deles de peso máximo (15) — com saldo líquido de +65 pontos.

Cobertura funcional superior do PostHog **é real**, mas seu peso (10) é insuficiente para compensar as perdas em C01, C03, C04 e C10.

---

## 11. Quando escolher qual

### 11.1 Escolha Matomo On-Premise quando

- Portais **institucionais e de conteúdo** (news, transparência, notícias, editais).
- Requisito primário é **analytics de audiência** (page views, origem, tempo, geolocalização).
- SSO federado com IdP corporativo é **obrigatório** e você não quer pagar EE de PostHog.
- Equipe SETDIG opera baseline MySQL/PostgreSQL (é o caso).
- Precisa de **precedente jurídico favorável** de DPA europeia para isenção de consentimento.
- Preferência por **licença copyleft** (GPL v3) para eliminar risco de fechamento futuro.
- Necessidade de **Tag Manager nativo** e painel público de estatísticas.

### 11.2 Escolha PostHog OSS quando

- Você opera **produto SaaS interno** (app do cidadão, superapp) com forte necessidade de *product analytics*.
- **Feature flags corporativos** são requisito real (não só desejável).
- **Experimentação A/B com significância estatística** é parte do processo de release.
- **LLM observability** é necessária (produto com IA generativa).
- Equipe já opera **Kubernetes + ClickHouse + Kafka** como padrão de plataforma.
- Você tem **orçamento e mandato** para pagar EE (SSO SAML + RBAC + auditoria).
- Aceita **risco de deprecação futura** do modelo self-hosted pelo fornecedor.

### 11.3 Escolha PostHog Cloud (não auto-hospedado) quando

- Startup ou empresa privada (não órgão público brasileiro tratando dado do cidadão).
- Aceita transferência internacional para EUA ou UE.
- Aceita jurisdição do operador nos EUA (CLOUD Act).
- Valoriza **operação zero** acima de soberania.

**Nenhum desses cenários corresponde ao caso central da SETDIG.**

---

## 12. Recomendação circunstanciada

> **✅ Bloco de Decisão — resposta ao viés declarado**
>
> A boa impressão da modalidade Cloud do PostHog é técnica e legítima. É um produto excelente para o que ele se propõe a fazer.
>
> **Mas a modalidade Cloud não é opção para o Estado** (soberania, CLOUD Act, custo em dólar por evento), e a modalidade **OSS auto-hospedada não replica a experiência da Cloud**:
>
> - Perde SSO SAML, RBAC granular e auditoria (recuperáveis com EE paga: R$ 300k / 5 anos).
> - Perde suporte comercial oficial ao stack self-hosted.
> - Ganha stack de 6 componentes (Kafka + ClickHouse + PG + Redis + MinIO + Django) contra 2 do Matomo (PHP + MySQL).
> - Requer 0,4 FTE marginal contra 0,1 FTE marginal do Matomo.
>
> **TCO 4,1× superior ao Matomo On-Premise** em regime marginal SETDIG, para **entregar valor incremental** (feature flags, experimentation, LLM obs) **que não é requisito prioritário** do parque de portais estaduais.
>
> **Recomendação:** manter o Matomo On-Premise como plataforma primária (ADR-001). Reservar o PostHog para **estudo específico futuro** se e quando o Estado desenvolver **produto digital transacional complexo** que justifique product analytics de SaaS moderno — cenário em que o PostHog seria naturalmente o candidato mais forte.

### 12.1 Cenário híbrido teórico (não recomendado neste momento)

**Poderia** existir arquitetura híbrida: Matomo On-Premise para portais institucionais + PostHog OSS para um serviço digital transacional específico (ex.: superapp cidadão). Não é recomendado agora porque:

1. Adiciona plataforma sem caso de uso concreto justificando o custo operacional adicional.
2. Duplica esforço de capacitação e operação.
3. O Plausible CE (recomendação RC-1 já aprovada) já cobre a camada complementar prevista.

O híbrido volta a ser considerado se o Estado formalizar iniciativa de superapp com requisitos de product analytics.

---

## 13. Antipadrão a evitar — extrapolar experiência de Cloud para OSS

**Este é um padrão comum e enviesado:** experimentar a modalidade gerenciada de um produto SaaS, ficar impressionado com a experiência, e assumir que a versão auto-hospedada entrega o mesmo.

Casos análogos documentados na indústria (todos para referência, sem inclusão como fonte primária):

| Produto | Cloud gerenciada | OSS auto-hospedada |
|---------|-------------------|---------------------|
| GitLab | Toda funcionalidade | Enterprise Edition paga para features corporativas |
| Elastic | Managed service completo | Stack complexo; Elastic License restringe uso |
| MongoDB | Atlas com todas as features | Server Community sem features Enterprise |
| Confluent (Kafka) | Managed com Schema Registry, ksqlDB | Apache Kafka puro + operação manual |
| PostHog | Todos os módulos ativos + EE incluída | Core MIT + EE paga separada |

**Padrão comum:** a experiência da Cloud embute (a) todas as features EE, (b) operação zero do fornecedor, (c) upgrades transparentes, (d) SLA financeiro. O OSS entrega o **motor**, não o **carro completo**.

**Regra prática:** ao avaliar produto open-core para produção institucional, testar a **modalidade auto-hospedada real** (com o esforço operacional que ela demanda), não a Cloud. Do contrário, o TCO estimado subestima em 2× a 5× o custo verdadeiro.

---

## Referências cruzadas

- Ficha completa Matomo: [`../plataformas/matomo.md`](../plataformas/matomo.md)
- Ficha completa PostHog: [`../plataformas/posthog.md`](../plataformas/posthog.md)
- Justificativa nota-a-nota: [`../docs/07-matriz-decisao.md §4.1 e §4.6`](../docs/07-matriz-decisao.md#41-matomo-on-premise--520-pontos)
- Trade-offs Matomo: [`../docs/06-tradeoffs.md §3`](../docs/06-tradeoffs.md#3-matomo-on-premise)
- Custo comparado: [`custo.md §3.1`](custo.md#31-tco-consolidado--5-anos-portal-médio-regime-marginal)
- Infraestrutura por plataforma: [`infraestrutura.md`](infraestrutura.md)
- ADR-001: [`../docs/08-adr.md`](../docs/08-adr.md)
