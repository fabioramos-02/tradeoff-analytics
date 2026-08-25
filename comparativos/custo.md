# Comparativo — Custo (TCO em 5 anos) — INFORMATIVO

> **Corte transversal** · Base: portal médio, ~500 mil *page views*/mês, ~50 propriedades/sites
> **Regime de custo:** **marginal para o Estado** — ver §1.4
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/07-matriz-decisao.md) · [ADR-002](../docs/08-adr-002.md)

> **📌 Nota metodológica — Custo passou a ser critério informativo (revisão 2026)**
> A partir do [ADR-002](../docs/08-adr-002.md), o critério C03 (TCO) foi rebaixado a **peso 0** na matriz de decisão. Este comparativo permanece publicado e mantido atualizado, mas com finalidade **estritamente informativa**:
>
> - Serve como referência de **dimensionamento de infraestrutura** on-premise.
> - Serve como base para **prestação de contas** ao TCE-MS e à CGE-MS, demonstrando economicidade das decisões arquiteturais.
> - Reproduz o cenário de sensibilidade **"Custo pleno (referência histórica)"** definido em [`../docs/03-criterios-de-avaliacao.md §4.4`](../docs/03-criterios-de-avaliacao.md#44-pesos-alternativos-testados), permitindo auditoria da revisão.
>
> **O que este documento NÃO faz mais:** não é insumo para ranquear plataformas nem para eliminar alternativas. Sob a premissa on-premise consolidada (P1/P2/R4), o custo marginal é absorvido pela infra do Estado; ferramentas que atendem melhor o caso de uso (funcional, técnico, LGPD) devem prevalecer, mesmo com TCO superior. Ver justificativa em [`../docs/03-criterios-de-avaliacao.md §4.3`](../docs/03-criterios-de-avaliacao.md#43-justificativa-da-atribuição-de-pesos).

---

## Sumário

- [1. Metodologia](#1-metodologia)
- [2. Rubricas do TCO](#2-rubricas-do-tco)
- [3. Panorama comparativo](#3-panorama-comparativo)
- [4. Detalhamento por plataforma](#4-detalhamento-por-plataforma)
- [5. Análise de sensibilidade](#5-análise-de-sensibilidade)
- [6. Custos ocultos por categoria](#6-custos-ocultos-por-categoria)
- [7. Recomendação de contratação](#7-recomendação-de-contratação)

---

## 1. Metodologia

### 1.1 Premissas

| Premissa | Valor |
|----------|-------|
| Horizonte | 5 anos |
| Câmbio de referência | R$ 5,40 / US$ 1,00 (média projetada 2026–2030) |
| Volume mensal (baseline) | 500 mil *page views* / 50 propriedades |
| Volume anual (baseline) | 6 milhões de eventos ingeridos |
| Custo hora-técnico interno | R$ 120,00 (padrão SETDIG) |
| Inflação anual de infraestrutura | 5 % a.a. |
| Portais a instrumentar (parque total 5 anos) | ~200 |
| Perfil de equipe marginal | ~0,1 FTE em regime permanente (após P2/R5) |

### 1.2 Fórmula

```
TCO_marginal = Σᵢ (Licenciamentoᵢ + Infra_marginalᵢ + Equipe_marginalᵢ + Implantaçãoᵢ + Adequação_LGPDᵢ + Riscosᵢ)
              para i = ano 1 até ano 5
```

### 1.3 Regra de evidência

Todo valor deste comparativo é rastreável a:

- Tabela de preços oficial do fornecedor ([`docs/12-referencias.md`](../docs/12-referencias.md)); ou
- Estimativa da equipe de arquitetura baseada em desempenho de plataformas equivalentes já operadas pela SETDIG; ou
- Custo interno de hora-técnico e infraestrutura conforme padrão de referência SETDIG.

Valores estimados são marcados com `*` na tabela.

### 1.4 Regime de custo — marginal para o Estado

> **📌 Observação — por que custo marginal, não pleno**
> Este comparativo modela **custo marginal** — o adicional efetivo sobre os contratos e a infraestrutura já em operação no Estado. Não é o custo greenfield de mercado.
>
> **Fundamento normativo:**
> - **P1** ([`docs/01-contexto.md §7`](../docs/01-contexto.md#7-premissas)) — Estado dispõe de infra própria capaz de hospedar aplicação web + banco relacional com HA.
> - **P2** ([`docs/01-contexto.md §7`](../docs/01-contexto.md#7-premissas)) — equipe STI tem competência em Linux, PHP/containers, MySQL, observabilidade.
> - **R4** ([`docs/01-contexto.md §8`](../docs/01-contexto.md#8-restrições)) — solução deve operar sobre infra ou contratos de nuvem já disponíveis.
> - **R5** ([`docs/01-contexto.md §8`](../docs/01-contexto.md#8-restrições)) — sem previsão de ampliação de quadro STI.
>
> **O que muda em regime marginal:**
> - **Infraestrutura compartilhada** (nós K8s, DBaaS, storage) — rateio no parque existente ≈ 0 se cabe na capacidade ociosa, até ~R$ 30k/ano se demandar expansão de quota.
> - **Operação de kernel** (backup, patching de SO, monitoração, upgrades de K8s) — já contratual; custo marginal = 0.
> - **Equipe marginal** — dos 0,3 FTE de esforço intrínseco do Matomo (regra C10), parcela incremental sobre o time SETDIG é ~0,1 FTE (~R$ 15k/ano).
> - **Implantação por portal (snippet)** — trivial em WordPress: ~0,5 h por portal × 200 portais = 100 h ≈ R$ 12k, arredondado R$ 10k para o parque em 5 anos.
>
> **O que NÃO muda:**
> - Licenciamento de plugins comerciais, adequação LGPD, capacitação de equipe em Matomo, riscos específicos do produto (tuning `archive.php`, upgrades majors).
>
> **Assimetria intencional — SaaS estrangeiras:** GA4, Adobe, Clarity, Simple, Cloudflare, Umami Cloud, PostHog Cloud **não têm rubrica absorvível**. O custo delas é integralmente incremental. Não é viés contra SaaS — é reflexo fiel da realidade contratual.
>
> **Coluna dupla nas fichas:** cada `plataformas/<x>.md §12` apresenta "Custo marginal SETDIG" (base da nota C03) + "Custo pleno de referência" (greenfield). A nota da matriz usa a coluna marginal.

---

## 2. Rubricas do TCO

| Rubrica | Definição | Aplicabilidade |
|---------|-----------|----------------|
| **Licenciamento** | Pagamento ao fornecedor por uso/assinatura/plugin/EE | Toda plataforma |
| **Infra marginal** | Adicional real sobre o parque K8s + DBaaS + storage do Estado | Auto-hospedadas |
| **Equipe marginal** | Horas-técnico incrementais além do baseline contratual da STI | Auto-hospedadas |
| **Implantação** | Inserção do snippet + configuração inicial por portal | Todas |
| **Adequação LGPD** | DPIA, banner, revisão jurídica, adequação contratual | Todas |
| **Riscos** | Provisão para eventos previsíveis (câmbio, upgrade forçado, migração) | Todas |

---

## 3. Panorama comparativo

### 3.1 TCO consolidado — 5 anos, portal médio (regime marginal)

| # | Plataforma | Modalidade | Licenciamento | Infra marginal | Equipe marginal | Implantação | LGPD | Riscos | **TCO 5 anos** |
|---|-----------|-----------|--------------:|---------------:|----------------:|------------:|-----:|-------:|---------------:|
| 1 | **Matomo** | On-Premise | R$ 0 | R$ 30.000 | R$ 75.000 | R$ 10.000 | R$ 30.000 | R$ 30.000 | **R$ 175.000** |
| 2 | Matomo | On-Premise + plugins (Heatmaps, Funnels, Form) | R$ 90.000 | R$ 30.000 | R$ 75.000 | R$ 10.000 | R$ 30.000 | R$ 30.000 | **R$ 265.000** |
| 3 | Matomo | Cloud (SaaS UE) | R$ 240.000 | R$ 0 | R$ 45.000 | R$ 10.000 | R$ 45.000 | R$ 25.000 | **R$ 365.000** |
| 4 | Plausible | Community Edition | R$ 0 | R$ 20.000 | R$ 60.000 | R$ 10.000 | R$ 15.000 | R$ 15.000 | **R$ 120.000** |
| 5 | Plausible | Cloud (SaaS UE) | R$ 250.000 | R$ 0 | R$ 30.000 | R$ 10.000 | R$ 30.000 | R$ 25.000 | **R$ 345.000** |
| 6 | Umami | Auto-hospedado | R$ 0 | R$ 15.000 | R$ 45.000 | R$ 10.000 | R$ 15.000 | R$ 15.000 | **R$ 100.000** |
| 7 | Umami | Cloud | R$ 180.000 | R$ 0 | R$ 30.000 | R$ 10.000 | R$ 30.000 | R$ 20.000 | **R$ 270.000** |
| 8 | PostHog | Auto-hospedado (OSS + EE mínima) | R$ 300.000 | R$ 300.000 | R$ 240.000 | R$ 15.000 | R$ 40.000 | R$ 60.000 | **R$ 955.000** |
| 9 | PostHog | Cloud EU | R$ 700.000 * | R$ 0 | R$ 60.000 | R$ 15.000 | R$ 40.000 | R$ 40.000 | **R$ 855.000** |
| 10 | Google Analytics 4 | SaaS gratuito | R$ 0 | R$ 0 | R$ 60.000 | R$ 10.000 | R$ 90.000 | R$ 60.000 | **R$ 220.000** |
| 11 | Google Analytics 4 | 360 (Enterprise) | R$ 2.500.000+ | R$ 0 | R$ 120.000 | R$ 10.000 | R$ 90.000 | R$ 60.000 | **R$ 2.780.000+** |
| 12 | Adobe Analytics | SaaS enterprise | R$ 4.000.000+ | R$ 0 | R$ 240.000 | R$ 10.000 | R$ 90.000 | R$ 100.000 | **R$ 4.440.000+** |
| 13 | Simple Analytics | SaaS | R$ 16.000 | R$ 0 | R$ 12.000 | R$ 10.000 | R$ 12.000 | R$ 2.000 | **R$ 52.000** |
| 14 | Microsoft Clarity | SaaS gratuito | R$ 0 | R$ 0 | R$ 15.000 | R$ 10.000 | R$ 27.000 | R$ 15.000 | **R$ 67.000** |
| 15 | Cloudflare Web Analytics | SaaS gratuito | R$ 0 | R$ 0 | R$ 6.000 | R$ 5.000 | R$ 7.000 | R$ 5.000 | **R$ 23.000** |
| 16 | Open Web Analytics | Auto-hospedado | R$ 0 | R$ 20.000 | R$ 90.000 * | R$ 10.000 | R$ 15.000 | R$ 30.000 | **R$ 165.000** |

Valores `*` são estimativas. `+` indica valor mínimo — teto depende de negociação enterprise.

### 3.2 Ranking por TCO puro (regime marginal)

| # | Plataforma | TCO 5 anos | Faixa C03 | Nota C03 | Cobertura funcional |
|---|-----------|-----------:|:---------:|:--------:|---------------------|
| 1 | Cloudflare Web Analytics | R$ 23.000 | ≤ 250k | **5** | 🔴 Insuficiente |
| 2 | Simple Analytics | R$ 52.000 | ≤ 250k | **5** | 🔴 Insuficiente |
| 3 | Microsoft Clarity | R$ 67.000 | ≤ 250k | **5** | 🟠 Nicho (UX diagnostics) |
| 4 | Umami (auto-hospedado) | R$ 100.000 | ≤ 250k | **5** | 🟠 Limitada — sem funil |
| 5 | Plausible Community Edition | R$ 120.000 | ≤ 250k | **5** | 🟠 Média — sem heatmap/replay/A-B |
| 6 | Open Web Analytics | R$ 165.000 | ≤ 250k | **5** | 🟠 Média — projeto estagnado (reprovado por segurança) |
| 7 | **Matomo On-Premise (núcleo)** | **R$ 175.000** | ≤ 250k | **5** | 🟢 Ampla — recomendação primária |
| 8 | Google Analytics 4 gratuito | R$ 220.000 | ≤ 250k | 5 | 🟢 Ampla — mas LGPD/soberania problemáticas |
| 9 | Matomo On-Premise + plugins pagos | R$ 265.000 | 250k–600k | **4** | 🟢 Muito ampla |
| 10 | Umami Cloud | R$ 270.000 | 250k–600k | 4 | 🟠 Limitada |
| 11 | Plausible Cloud | R$ 345.000 | 250k–600k | 4 | 🟠 Média |
| 12 | Matomo Cloud | R$ 365.000 | 250k–600k | 3 | 🟢 Ampla |
| 13 | PostHog Cloud EU | R$ 855.000 | 600k–1,2M | 3 | 🟢 Muito ampla |
| 14 | PostHog Auto-hospedado (OSS+EE) | R$ 955.000 | 600k–1,2M | **3** | 🟢 Muito ampla |
| 15 | Google Analytics 4 360 | R$ 2.780.000+ | > 3M | 1 | 🟢 Muito ampla |
| 16 | Adobe Analytics | R$ 4.440.000+ | > 3M | 1 | 🟢 Muito ampla |

> **📌 Observação — TCO isolado é enganoso**
> A comparação por TCO puro coloca Cloudflare em 1º e Matomo em 7º. Isso não é razão para escolher Cloudflare — a comparação **precisa** ser feita entre plataformas com **cobertura funcional equivalente**. Fazendo esse recorte, o Matomo On-Premise é a mais econômica entre as plataformas que atendem aos requisitos *must-have* completos.

---

## 4. Detalhamento por plataforma

### 4.1 Matomo On-Premise (núcleo, sem plugins pagos)

| Rubrica | Ano 1 | Ano 2 | Ano 3 | Ano 4 | Ano 5 | Total marginal | Total pleno referência |
|---------|------:|------:|------:|------:|------:|---------------:|-----------------------:|
| Licenciamento | R$ 0 | R$ 0 | R$ 0 | R$ 0 | R$ 0 | **R$ 0** | R$ 0 |
| Infra marginal (rateio K8s+DBaaS) | R$ 6.000 | R$ 6.000 | R$ 6.000 | R$ 6.000 | R$ 6.000 | **R$ 30.000** | R$ 210.000 |
| Equipe marginal (~0,1 FTE) | R$ 25.000 | R$ 12.500 | R$ 12.500 | R$ 12.500 | R$ 12.500 | **R$ 75.000** | R$ 300.000 |
| Implantação (snippet ~200 portais) | R$ 5.000 | R$ 2.000 | R$ 1.500 | R$ 1.000 | R$ 500 | **R$ 10.000** | R$ 60.000 |
| Adequação LGPD | R$ 20.000 | R$ 2.500 | R$ 2.500 | R$ 2.500 | R$ 2.500 | **R$ 30.000** | R$ 30.000 |
| Riscos (upgrades, tuning archive) | R$ 6.000 | R$ 6.000 | R$ 6.000 | R$ 6.000 | R$ 6.000 | **R$ 30.000** | R$ 30.000 |
| **Total** | **R$ 62.000** | **R$ 29.000** | **R$ 28.500** | **R$ 28.000** | **R$ 27.500** | **R$ 175.000** | **R$ 630.000** |

**Custos ocultos assumidos:** tuning periódico de `archive.php` (~8 h/mês), upgrades majors semestrais com validação de plugins.

**Nota C03 resultante (faixa ≤ 250k):** **5**.

### 4.2 Matomo On-Premise + plugins comerciais essenciais

Plugins do Marketplace: *Heatmaps & Session Recording* + *Funnels* + *Form Analytics*.

Licenciamento adicional: ~R$ 18.000/ano/instância (preço público do Marketplace). Total 5 anos: **R$ 90.000**.

TCO marginal consolidado: **R$ 265.000** — cruza a faixa 5 pra 4 apenas nesta configuração enriquecida.

### 4.3 Matomo Cloud

Preço público (2026): a partir de € 259/mês (~R$ 1.400) para 1 M ações/mês, escalando com volume. Para volume de referência: ~€ 750/mês.

TCO marginal: **R$ 365.000**. Trade-off: perde soberania de dado, ganha operação zero.

### 4.4 Plausible Community Edition

| Rubrica | Ano 1 | Ano 2 | Ano 3 | Ano 4 | Ano 5 | Total marginal | Total pleno referência |
|---------|------:|------:|------:|------:|------:|---------------:|-----------------------:|
| Licenciamento | R$ 0 | R$ 0 | R$ 0 | R$ 0 | R$ 0 | **R$ 0** | R$ 0 |
| Infra marginal (1 VM + ClickHouse compartilhado) | R$ 4.000 | R$ 4.000 | R$ 4.000 | R$ 4.000 | R$ 4.000 | **R$ 20.000** | R$ 90.000 |
| Equipe marginal | R$ 20.000 | R$ 10.000 | R$ 10.000 | R$ 10.000 | R$ 10.000 | **R$ 60.000** | R$ 180.000 |
| Implantação (snippet) | R$ 5.000 | R$ 2.000 | R$ 1.500 | R$ 1.000 | R$ 500 | **R$ 10.000** | R$ 60.000 |
| Adequação LGPD (baixa — cookieless) | R$ 10.000 | R$ 1.250 | R$ 1.250 | R$ 1.250 | R$ 1.250 | **R$ 15.000** | R$ 15.000 |
| Riscos | R$ 3.000 | R$ 3.000 | R$ 3.000 | R$ 3.000 | R$ 3.000 | **R$ 15.000** | R$ 15.000 |
| **Total** | **R$ 42.000** | **R$ 20.250** | **R$ 19.750** | **R$ 19.250** | **R$ 18.750** | **R$ 120.000** | **R$ 360.000** |

**Nota C03 resultante (faixa ≤ 250k):** **5**.

### 4.5 PostHog auto-hospedado (OSS + EE mínima)

TCO detalhado em [`plataformas/posthog.md §12`](../plataformas/posthog.md#12-custos). Fator dominante: **infraestrutura fora do padrão SETDIG** (Kafka + ClickHouse dedicado + MinIO exigem stack que o parque atual não cobre — abatimento parcial, não total).

TCO marginal: **R$ 955.000**. Nota C03 (faixa 600k–1,2M): **3**.

### 4.6 Google Analytics 4 (SaaS gratuito)

TCO nominal baixo. **Custo real** concentrado em:

- Adequação LGPD (R$ 90k): DPIA aprofundada, banner de consentimento avançado, DPA Google, configuração *server-side* para reduzir *fingerprinting*.
- Riscos (R$ 60k): sanções regulatórias em precedente europeu (CNIL, Garante, DPA Áustria declararam GA em desconformidade com GDPR em 2022–2023 sem *server-side proxy*).

Trade-off crítico: **soberania de dado inexistente** — dado bruto em infra Google (EUA/UE), sob CLOUD Act.

### 4.7 Adobe Analytics

TCO estimado a partir de **R$ 4,44 M** em 5 anos. Contratação enterprise (preço confidencial; referência US$ 100k–500k anuais). Fora do envelope orçamentário estadual para essa finalidade.

### 4.8 Microsoft Clarity

TCO baixo (R$ 67k). Utilidade limitada — nicho de UX diagnostics. Não substitui plataforma primária. **Sem custo absorvível** (SaaS puro).

### 4.9 Cloudflare Web Analytics

TCO **mais baixo** do estudo (R$ 23k), mas **entrega funcional insuficiente**. Serve como complemento a portais atrás da CDN Cloudflare.

### 4.10 Simple Analytics

TCO baixo (R$ 52k), mas **cobertura funcional insuficiente** e cobrança em dólar via cartão internacional.

### 4.11 Umami auto-hospedado

TCO marginal: **R$ 100.000**. Cobertura limitada (sem funil), mas viável para portais pequenos.

### 4.12 Open Web Analytics

TCO marginal: **R$ 165.000**. TCO baixo, mas **reprovado por segurança** (projeto estagnado, dependências desatualizadas). Não recomendado apesar do custo favorável.

---

## 5. Análise de sensibilidade

### 5.1 Sensibilidade ao regime de custo (marginal vs. pleno)

| Plataforma | TCO marginal | TCO pleno | Δ | Nota C03 marginal | Nota C03 pleno |
|-----------|-------------:|----------:|--:|:-----------------:|:--------------:|
| Matomo On-Premise | R$ 175k | R$ 630k | +R$ 455k | 5 | 4 |
| Plausible CE | R$ 120k | R$ 360k | +R$ 240k | 5 | 5 (limite) |
| Umami | R$ 100k | R$ 270k | +R$ 170k | 5 | 4 |
| PostHog OSS+EE | R$ 955k | R$ 1.540k | +R$ 585k | 3 | 2 |
| OWA | R$ 165k | R$ 375k | +R$ 210k | 5 | 4 |
| SaaS estrangeiras | inalterado | inalterado | 0 | — | — |

**Conclusão:** o regime marginal favorece consistentemente as auto-hospedadas, mas não altera o ranking de topo — Matomo OP continua líder com margem ampliada. O regime pleno seria defensável em cenário greenfield (Estado sem parque de infra); não é o caso.

### 5.2 Sensibilidade ao volume

Como o TCO da plataforma primária muda se o volume dobrar (1 M pv/mês)?

| Plataforma | TCO 5 anos @ 500k | TCO 5 anos @ 1 M | Variação |
|-----------|:-----------------:|:----------------:|---------:|
| Matomo On-Premise (marginal) | R$ 175.000 | R$ 200.000 | +14 % |
| Matomo Cloud | R$ 365.000 | R$ 580.000 | +59 % |
| Plausible Community (marginal) | R$ 120.000 | R$ 135.000 | +13 % |
| Plausible Cloud | R$ 345.000 | R$ 530.000 | +54 % |
| GA4 gratuito | R$ 220.000 | R$ 220.000 | 0 % |
| PostHog OSS+EE (marginal) | R$ 955.000 | R$ 1.010.000 | +6 % |
| PostHog Cloud EU | R$ 855.000 | R$ 1.450.000 | +70 % |

> **📌 Observação — SaaS escala em custo, auto-hospedado escala em infra**
> Auto-hospedadas absorvem volume com pouco impacto no TCO. SaaS crescem quase linearmente com o volume. Argumento estrutural para auto-hospedagem em cenário de crescimento previsto — como é o parque estadual.

### 5.3 Sensibilidade ao câmbio (SaaS estrangeiros)

| Plataforma | R$ 5,00 / US$ | R$ 5,40 / US$ | R$ 6,00 / US$ | R$ 7,00 / US$ |
|-----------|--------------:|--------------:|--------------:|--------------:|
| Simple Analytics | R$ 48.500 | R$ 52.000 | R$ 57.500 | R$ 66.500 |
| PostHog Cloud EU | R$ 790.000 | R$ 855.000 | R$ 960.000 | R$ 1.120.000 |
| GA4 360 | R$ 2.570.000 | R$ 2.780.000 | R$ 3.100.000 | R$ 3.630.000 |
| Adobe Analytics | R$ 4.110.000 | R$ 4.440.000 | R$ 4.940.000 | R$ 5.770.000 |

Auto-hospedadas em regime marginal são **imunes** ao câmbio no licenciamento (R$ 0). Impacto apenas em infra internacional, se aplicável — não é o caso do parque SETDIG.

---

## 6. Custos ocultos por categoria

### 6.1 Custos ocultos por plataforma

| Plataforma | Custos ocultos identificados |
|-----------|-------------------------------|
| **Matomo** | Tuning periódico de arquivamento; upgrades majors com quebra de plugins; plugins comerciais opcionais para heatmap/funnel/A-B |
| **Plausible CE** | Sem funil visual (produto complementar para requisitos avançados); ClickHouse exige competência específica |
| **Umami** | Sem funil, sem heatmap — cobre requisito básico apenas |
| **PostHog** | Kafka + ClickHouse dedicado + MinIO = infra fora do padrão SETDIG; EE necessária para SSO/RBAC |
| **GA4 gratuito** | Custo regulatório LGPD; risco de sanção; server-side tagging |
| **Adobe Analytics** | Consultoria de implantação e treinamento como multiplicador 1,5×–2× sobre licenciamento |
| **Simple Analytics** | Contratação em dólar via cartão — fricção contratual pública |
| **Microsoft Clarity** | Adequação LGPD para transferência internacional; retenção fixa de 30 dias |
| **Cloudflare** | Dependência estrutural da CDN Cloudflare |
| **Open Web Analytics** | Estagnação do projeto — risco de precisar migrar dentro do horizonte de 5 anos |

### 6.2 Custos ocultos por categoria transversal

| Categoria | Onde tende a aparecer |
|-----------|-----------------------|
| Consultoria externa de implantação | Adobe, PostHog, Matomo com plugins avançados |
| Câmbio | Toda plataforma SaaS estrangeira |
| Treinamento de equipe | PostHog (curva alta), Adobe (produto complexo) |
| Adequação regulatória | GA4, Clarity, Cloudflare — jurisdição EUA |
| Migração / retorno atrás | OWA (estagnação), plataformas MIT sem cláusula de reciprocidade |
| Plugins pagos | Matomo (Heatmaps, Funnels, Form Analytics, A/B, Media, Custom Reports) |

---

## 7. Recomendação de contratação

### 7.1 Cenário-base

> **✅ Bloco de Decisão — cenário-base para orçamento**
>
> Provisionar **R$ 265.000** ao longo de 5 anos para operação do **Matomo On-Premise com plugins comerciais essenciais** (Heatmaps & Session Recording, Funnels, Form Analytics), em regime de custo marginal SETDIG. Acrescido de:
>
> - Camada complementar Plausible CE para portais de baixa criticidade (~R$ 120k marginal em 5 anos, ou compartilhamento de infraestrutura reduzindo esse número em 40 %);
> - Provisão condicionada de licenças pontuais para complementos conforme necessidade real (Clarity/Cloudflare — abaixo de R$ 100k em 5 anos).

### 7.2 Cenário conservador

Manter Matomo On-Premise **sem plugins comerciais** (soluções OSS de heatmap ou dispensa do recurso). TCO marginal reduzido a **R$ 175.000** em 5 anos, com renúncia às capacidades avançadas.

### 7.3 Cenário SaaS (contingência)

Migração para Matomo Cloud (hospedagem UE, DPA conforme GDPR). TCO **R$ 365.000** em 5 anos, com perda de soberania e ganho de operação zero. Aplicável em cenário de restrição de FTE técnico ou incidente prolongado.

---

Referências primárias de preço: [`docs/12-referencias.md`](../docs/12-referencias.md).
Detalhamento por plataforma: [`plataformas/`](../plataformas/).
Regra metodológica: [`docs/03-criterios-de-avaliacao.md §5.C03`](../docs/03-criterios-de-avaliacao.md#c03--tco-peso-15).
