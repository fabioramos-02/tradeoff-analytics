---
marp: true
theme: default
paginate: true
size: 16:9
header: "PostHog complementar no Xvia · SETDIG (ADR-002)"
footer: "Fonte: estudo tradeoff-analytics · ADR-002 (2026-08) · Ver README.md"
style: |
  section { font-family: system-ui, sans-serif; color: #30302e; }
  h1 { color: #004f9f; border-bottom: 3px solid #004f9f; padding-bottom: 8px; }
  h2 { color: #004f9f; }
  strong { color: #003a76; }
  table { font-size: 0.85em; }
  th { background: #004f9f; color: white; }
  tr:nth-child(even) { background: #f0f4fa; }
  .anchor { font-size: 1.4em; font-weight: 700; color: #003a76; margin: 0.4em 0; }
  .number { font-size: 2.6em; font-weight: 800; color: #004f9f; line-height: 1; }
  .caption { color: #555; font-size: 0.9em; }
  .warn { background: #fff4e5; border-left: 6px solid #e58900; padding: 10px 16px; }
  .ok { background: #e8f4ff; border-left: 6px solid #004f9f; padding: 10px 16px; }
---

# PostHog complementar no portal Xvia

**Decisão homologada — ADR-002 (2026-08).**
Matomo continua padrão do parque. Xvia ganha PostHog em paralelo.

<div class="caption">
Secretaria-Executiva de Transformação Digital — SETDIG · agosto/2026<br>
Base: estudo formal de 13 plataformas (TOGAF + ATAM + MAUT + ADR-002 supersedes ADR-001)
</div>

---

## O que mudou desde o ADR-001

<div class="ok">
Duas premissas foram atualizadas em agosto/2026.
</div>

| Dimensão | ADR-001 (jul/26) | **ADR-002 (ago/26)** |
|----------|------------------|----------------------|
| Peso do TCO na matriz | 15 (12,5 %) | **0 — critério informativo** |
| Recomendação para o parque | Matomo + Plausible | **Matomo (Plausible opcional)** |
| Recomendação para o Xvia | Cenário C "requer justificativa" | **Matomo + PostHog em paralelo** |

**Por que o custo saiu?** Premissa on-premise consolidada (P1/P2/R4). Infra do Estado absorve custo marginal. TCO deixa de filtrar decisão arquitetural — mantido como informativo para prestação de contas.

---

## Ranking revisado (pesos ADR-002)

| # | Plataforma | Score | Δ vs ADR-001 |
|--:|-----------|------:|--------------|
| 🥇 1 | **Matomo On-Premise** | **500** | −20 |
| 🥈 2 | Piwik PRO | 499 | +34 |
| 🥉 3 | Matomo Cloud | 485 | +20 |
| 4 | **PostHog auto-hospedado** | **464** | +9 |
| 5 | Plausible CE | 460 | −25 |
| 6 | Adobe Analytics | 459 | +54 |

**Matomo mantém liderança** (1 pt sobre Piwik, desempate por C02).
**PostHog sobe para #4** — melhor opção soberana de product analytics.
**Adobe** salta 54 pts — descarte agora se fundamenta em lock-in (C04=1), não em custo.

---

## Dois perfis, duas arquiteturas

<div class="ok">
Decisão híbrida deliberada — não é incoerência arquitetural.
</div>

| Perfil | Arquitetura | Justificativa |
|--------|-------------|---------------|
| **Parque existente** (EDS + sites gov MS) | Matomo puro | Continuidade; conteúdo institucional; cookieless CNIL |
| **Novo portal Xvia** (superapp cidadão) | Matomo + PostHog em paralelo | Jornada identificada, funil transacional, feature flags, session replay consentido |

Diferença justificada pelo **perfil de uso**, não por preferência tecnológica.

---

## Papéis complementares no Xvia

| Pergunta | Ferramenta |
|----------|-----------|
| Quantos visitaram o Xvia? De onde vieram? | **Matomo** |
| Página com maior tempo médio? | **Matomo** |
| Qual serviço tem maior conversão? | **PostHog** |
| Onde o cidadão abandona o pedido? | **PostHog** |
| A nova versão do formulário aumentou conclusão? | **PostHog** (feature flag + A/B) |
| Cidadão voltou após 30 dias? | **PostHog** (retenção identificada) |
| Grave sessão deste serviço crítico | **PostHog** (opt-in + mascarado) |

Regra: **cada capability instrumentada em uma única ferramenta**. Detalhamento em `coexistencia-matomo-posthog.md`.

---

## O overhead — assumido conscientemente

| Métrica | Matomo | PostHog | **Ambos** | Alvo |
|---------|:------:|:-------:|:---------:|:----:|
| JS gzip | 22 KB | 55 KB | **~77 KB** | ≤ 100 KB |
| LCP 3G (p75) | < 50 ms | 50–200 ms | **100–250 ms** | ≤ 75 ms |
| LCP 4G+ (p75) | Desprezível | 20–50 ms | **30–80 ms** | ≤ 75 ms |

<div class="warn">
Usuário do Xvia chega com intenção específica — taxa de abandono por LCP é qualitativamente menor que em portal de conteúdo. Aceite formal, monitorado pelo <strong>gatilho G9</strong>: LCP &gt; 75 ms sustentado por 2 meses aciona reavaliação em 60 dias.
</div>

---

## Governança de session replay

<div class="warn">
Risco R-11: captura acidental de dado sensível.
Sem essas regras, session replay é vetado.
</div>

1. **Desabilitado por padrão.** Ativado apenas após opt-in explícito.
2. **Mascaramento agressivo padrão.** CPF, senha, dado de saúde, valores financeiros: máscara `***`.
3. **Ativação por serviço**, com DPIA específico + homologação do time de segurança.
4. **Retenção curta:** 30 dias (vs 12 meses de eventos brutos).
5. **Auditoria mensal por amostragem** pelo time de segurança.

Ativação fora dessas regras = incidente de conformidade.

---

## Roadmap — Onda 5 (Xvia)

**Período:** mês 12 a 24, paralela às Ondas 3 e 4.

| # | Marco | Duração |
|---|-------|--------|
| 5.1 | DPIA do session replay aprovado pelo DPO | 3 sem |
| 5.2 | Stack PostHog em produção sob custódia do Estado | 6 sem |
| 5.3 | STI capacitada em ClickHouse + Kafka (80–120 h) | 8 sem |
| 5.4 | Segmentação de eventos documentada | 3 sem |
| 5.5 | Xvia instrumentado com ambos os SDKs | 6 sem |
| 5.12 | **Gate de 12 meses** ao Comitê | 4 sem |

Falha no gate aciona G12 — reavaliação em 60 dias.

---

## Critérios do gate de 12 meses

Continuidade da coexistência exige **todos** os critérios abaixo:

1. LCP p75 3G do Xvia ≤ 75 ms por pelo menos 10 dos 12 meses.
2. ≥ 3 feature flags em uso produtivo (não apenas configuradas).
3. ≥ 5 funis identificados em uso pelo SGD.
4. Session replay em ≥ 1 serviço crítico sem incidente de captura acidental.
5. Divergência Matomo × PostHog ≤ 15 % por 9 dos 12 meses.
6. STI opera stack de forma autônoma (ou suporte especializado formal).

Falha em qualquer critério → cenários A (ajuste), B (Cloud EU), C (reversão a Matomo puro).

---

## Novos riscos assumidos (ADR-002)

| ID | Risco | Estratégia |
|----|-------|-----------|
| R-11 | Captura acidental em session replay | Mitigar (opt-in + mascaramento + auditoria) |
| **R-17** | LCP degradado > 75 ms | Monitorar (G9) |
| **R-18** | STI não sustenta stack PostHog | Mitigar (capacitação + reserva de suporte) |
| **R-19** | Divergência Matomo × PostHog | Mitigar (governança de fonte + reconciliação) |
| **R-20** | Descontinuidade PostHog OSS | Monitorar (G11 + fallback Cloud EU) |

Detalhamento em `docs/11-riscos.md`.

---

## Contingências formais registradas

<div class="ok">
Ativação exige novo ADR (ADR-003), não decisão operacional.
</div>

**Para o parque (contingência do Matomo On-Premise):**
1. Matomo Cloud (agora #3 no ranking, faixa "Recomendada").
2. Piwik PRO Private Cloud (agora #2, "Recomendada").

**Para o Xvia (contingência do PostHog auto-hospedado):**
1. PostHog Cloud EU (com RIPD específico).
2. Matomo puro estendido com plugins (cobre 60–70 % do caso de uso).

---

## Decisão em uma frase

<div class="anchor">
Matomo continua padrão do parque. PostHog entra no Xvia em paralelo, com governança rigorosa e gate em 12 meses.
</div>

Não é substituição. Não é preferência tecnológica.
É reconhecimento de que web analytics e product analytics são eixos distintos, e o Xvia precisa de ambos.

---

## Referências

- **[ADR-002](../docs/08-adr-002.md)** — decisão vigente, supersedes ADR-001.
- **[07 — Matriz de decisão](../docs/07-matriz-decisao.md)** — ranking revisado.
- **[09 — Recomendação](../docs/09-recomendacao.md)** — parque vs Xvia.
- **[10 — Roadmap, Onda 5](../docs/10-roadmap.md#7bis-onda-5--portal-xvia-posthog-complementar)** — plano de execução.
- **[11 — Riscos](../docs/11-riscos.md)** — R-11, R-17 a R-20.
- **[Coexistência Matomo + PostHog no Xvia](../comparativos/coexistencia-matomo-posthog.md)** — modelo operacional.
- **[Complementaridade Matomo × PostHog](../comparativos/matomo-vs-posthog.md)** — papéis.

**Homologação:** aguardando manifestação do Comitê de Arquitetura, DPO, STI e time do portal Xvia.
