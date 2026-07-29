# Comparativo — Custo (TCO em 5 anos)

> **Corte transversal** · Base de comparação: portal médio, ~500 mil *page views*/mês, ~50 propriedades/sites
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/07-matriz-decisao.md)

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
| Perfil de equipe | 1 analista (0,5 FTE) + 1 SRE (0,2 FTE) para plataformas auto-hospedadas |

### 1.2 Fórmula

```
TCO = Σᵢ (Licenciamentoᵢ + Infraestruturaᵢ + Equipeᵢ + Adequação_LGPDᵢ + Riscos_operacionaisᵢ)
     para i = ano 1 até ano 5
```

### 1.3 Regra de evidência

Todo valor deste comparativo é rastreável a:

- Tabela de preços oficial do fornecedor (data de consulta em [`docs/12-referencias.md`](../docs/12-referencias.md)); ou
- Estimativa da equipe de arquitetura baseada em desempenho de plataformas equivalentes já operadas pela SETDIG; ou
- Custo interno de hora-técnico e infraestrutura conforme padrão de referência da SETDIG.

Valores estimados são marcados com `*` na tabela.

---

## 2. Rubricas do TCO

| Rubrica | Definição | Aplicabilidade |
|---------|-----------|----------------|
| **Licenciamento** | Pagamento ao fornecedor por uso/assinatura/plugin/EE | Toda plataforma |
| **Infraestrutura** | VMs, storage, banco, backup, rede, monitoração | Auto-hospedadas |
| **Equipe** | Horas-técnico dedicadas a implantação, configuração, operação e treinamento | Todas |
| **Adequação LGPD** | DPIA, banner de consentimento, revisão jurídica, adequação contratual | Todas — variável em intensidade |
| **Riscos operacionais** | Provisão para eventos previsíveis (câmbio, upgrade forçado, migração, incidente) | Todas — variável |

---

## 3. Panorama comparativo

### 3.1 TCO consolidado — 5 anos, portal médio

| # | Plataforma | Modalidade | Licenciamento | Infraestrutura | Equipe | LGPD | Riscos | **TCO 5 anos** |
|---|-----------|-----------|--------------:|---------------:|-------:|-----:|-------:|---------------:|
| 1 | **Matomo** | On-Premise | R$ 0 | R$ 210.000 | R$ 300.000 | R$ 30.000 | R$ 30.000 | **R$ 570.000** |
| 2 | Matomo | On-Premise + plugins pagos (Heatmaps, Funnels, Form) | R$ 90.000 | R$ 210.000 | R$ 300.000 | R$ 30.000 | R$ 30.000 | **R$ 660.000** |
| 3 | Matomo | Cloud (SaaS EU) | R$ 240.000 | R$ 0 | R$ 90.000 | R$ 45.000 | R$ 25.000 | **R$ 400.000** |
| 4 | Plausible | Community Edition (auto-hospedado) | R$ 0 | R$ 90.000 | R$ 180.000 | R$ 15.000 | R$ 15.000 | **R$ 300.000** |
| 5 | Plausible | SaaS (nuvem UE) | R$ 250.000 | R$ 0 | R$ 60.000 | R$ 30.000 | R$ 25.000 | **R$ 365.000** |
| 6 | Umami | Auto-hospedado | R$ 0 | R$ 60.000 | R$ 180.000 | R$ 15.000 | R$ 15.000 | **R$ 270.000** |
| 7 | Umami | Cloud | R$ 180.000 | R$ 0 | R$ 60.000 | R$ 30.000 | R$ 20.000 | **R$ 290.000** |
| 8 | PostHog | Auto-hospedado (OSS + EE mínima) | R$ 300.000 | R$ 480.000 | R$ 660.000 | R$ 40.000 | R$ 60.000 | **R$ 1.540.000** |
| 9 | PostHog | Cloud EU | R$ 700.000 * | R$ 0 | R$ 120.000 | R$ 40.000 | R$ 40.000 | **R$ 900.000** |
| 10 | Google Analytics 4 | SaaS gratuito | R$ 0 | R$ 0 | R$ 120.000 | R$ 90.000 | R$ 60.000 | **R$ 270.000** |
| 11 | Google Analytics 4 | 360 (Enterprise) | R$ 2.500.000+ | R$ 0 | R$ 180.000 | R$ 90.000 | R$ 60.000 | **R$ 2.830.000+** |
| 12 | Adobe Analytics | SaaS enterprise | R$ 4.000.000+ | R$ 0 | R$ 480.000 | R$ 90.000 | R$ 100.000 | **R$ 4.670.000+** |
| 13 | Simple Analytics | SaaS | R$ 16.000 | R$ 0 | R$ 18.000 | R$ 12.000 | R$ 2.000 | **R$ 48.000** |
| 14 | Microsoft Clarity | SaaS gratuito | R$ 0 | R$ 0 | R$ 24.000 | R$ 27.000 | R$ 15.000 | **R$ 66.000** |
| 15 | Cloudflare Web Analytics | SaaS gratuito | R$ 0 | R$ 0 | R$ 12.000 | R$ 7.000 | R$ 5.000 | **R$ 24.000** |
| 16 | Open Web Analytics | Auto-hospedado | R$ 0 | R$ 90.000 | R$ 240.000 * | R$ 15.000 | R$ 30.000 | **R$ 375.000** |

Valores marcados com `*` são estimativas. `+` indica valor mínimo — o teto depende de negociação enterprise e uso.

### 3.2 Ranking por TCO puro

| # | Plataforma | TCO 5 anos | Nota — cobertura funcional |
|---|-----------|-----------:|---------------------------|
| 1 | Cloudflare Web Analytics | R$ 24.000 | 🔴 Insuficiente |
| 2 | Simple Analytics | R$ 48.000 | 🔴 Insuficiente |
| 3 | Microsoft Clarity | R$ 66.000 | 🟠 Nicho (UX diagnostics) |
| 4 | Umami (auto-hospedado) | R$ 270.000 | 🟠 Limitada — sem funil |
| 5 | Google Analytics 4 gratuito | R$ 270.000 | 🟢 Ampla — mas LGPD/soberania problemáticas |
| 6 | Umami Cloud | R$ 290.000 | 🟠 Limitada — sem funil |
| 7 | **Plausible Community Edition** | **R$ 300.000** | 🟠 Média — sem heatmap/replay/A-B |
| 8 | Plausible Cloud | R$ 365.000 | 🟠 Média |
| 9 | Open Web Analytics | R$ 375.000 | 🟠 Média — plataforma estagnada |
| 10 | Matomo Cloud | R$ 400.000 | 🟢 Ampla |
| 11 | **Matomo On-Premise (núcleo)** | **R$ 570.000** | 🟢 Ampla — recomendação primária |
| 12 | Matomo On-Premise + plugins pagos | R$ 660.000 | 🟢 Muito ampla |
| 13 | PostHog Cloud EU | R$ 900.000 | 🟢 Muito ampla |
| 14 | PostHog Auto-hospedado (OSS+EE) | R$ 1.540.000 | 🟢 Muito ampla |
| 15 | Google Analytics 4 360 | R$ 2.830.000+ | 🟢 Muito ampla |
| 16 | Adobe Analytics | R$ 4.670.000+ | 🟢 Muito ampla |

> **📌 Observação — TCO isolado é enganoso**
> A comparação por TCO puro coloca Cloudflare Web Analytics em primeiro lugar e Matomo em 11º. Isso não é razão para escolher Cloudflare — a comparação **precisa** ser feita entre plataformas com **cobertura funcional equivalente**. Fazendo esse recorte, o Matomo On-Premise é a mais econômica entre as plataformas que atendem aos requisitos *must-have* completos.

---

## 4. Detalhamento por plataforma

### 4.1 Matomo On-Premise (núcleo, sem plugins pagos)

| Rubrica | Ano 1 | Ano 2 | Ano 3 | Ano 4 | Ano 5 | Total |
|---------|------:|------:|------:|------:|------:|------:|
| Licenciamento | R$ 0 | R$ 0 | R$ 0 | R$ 0 | R$ 0 | **R$ 0** |
| Infraestrutura (2 VMs médias + DB gerenciado) | R$ 42.000 | R$ 42.000 | R$ 42.000 | R$ 42.000 | R$ 42.000 | **R$ 210.000** |
| Equipe (implantação Y1, sustentação Y2–5) | R$ 120.000 | R$ 45.000 | R$ 45.000 | R$ 45.000 | R$ 45.000 | **R$ 300.000** |
| Adequação LGPD | R$ 20.000 | R$ 2.500 | R$ 2.500 | R$ 2.500 | R$ 2.500 | **R$ 30.000** |
| Riscos (upgrades majors, ajuste arquivamento) | R$ 6.000 | R$ 6.000 | R$ 6.000 | R$ 6.000 | R$ 6.000 | **R$ 30.000** |
| **Total** | **R$ 188.000** | **R$ 95.500** | **R$ 95.500** | **R$ 95.500** | **R$ 95.500** | **R$ 570.000** |

**Custos ocultos assumidos:** ajuste de configuração do arquivamento (`archive.php`) sob alto volume, upgrades majors com quebra de compatibilidade de plugins.

### 4.2 Matomo On-Premise + plugins comerciais essenciais

Plugins do Marketplace considerados: *Heatmaps & Session Recording*, *Funnels*, *Form Analytics*.

Licenciamento adicional: ~R$ 18.000/ano/instância (referência preço público do Marketplace). Total 5 anos: **R$ 90.000**.

TCO consolidado: **R$ 660.000**.

### 4.3 Matomo Cloud

Preço público (2026): a partir de € 259/mês para ~1 M ações/mês, escalando com volume. Para volume de referência: ~€ 750/mês.

TCO consolidado: **R$ 400.000**. Trade-off: perde soberania de dado (hospedagem UE, sob DPA), ganha operação zero.

### 4.4 Plausible Community Edition (auto-hospedado)

| Rubrica | Ano 1 | Ano 2 | Ano 3 | Ano 4 | Ano 5 | Total |
|---------|------:|------:|------:|------:|------:|------:|
| Licenciamento | R$ 0 | R$ 0 | R$ 0 | R$ 0 | R$ 0 | **R$ 0** |
| Infraestrutura (1 VM média + ClickHouse gerenciado) | R$ 18.000 | R$ 18.000 | R$ 18.000 | R$ 18.000 | R$ 18.000 | **R$ 90.000** |
| Equipe | R$ 60.000 | R$ 30.000 | R$ 30.000 | R$ 30.000 | R$ 30.000 | **R$ 180.000** |
| Adequação LGPD (baixa — cookieless) | R$ 10.000 | R$ 1.250 | R$ 1.250 | R$ 1.250 | R$ 1.250 | **R$ 15.000** |
| Riscos | R$ 3.000 | R$ 3.000 | R$ 3.000 | R$ 3.000 | R$ 3.000 | **R$ 15.000** |
| **Total** | **R$ 91.000** | **R$ 52.250** | **R$ 52.250** | **R$ 52.250** | **R$ 52.250** | **R$ 300.000** |

### 4.5 PostHog (auto-hospedado, OSS + EE mínima)

TCO detalhado em [`plataformas/posthog.md`](../plataformas/posthog.md) §12.
Fator dominante: infraestrutura (Kubernetes + ClickHouse + Kafka) e equipe (curva alta).

### 4.6 Google Analytics 4 (SaaS gratuito)

TCO nominal muito baixo. **Custo real** concentrado em:

- Adequação LGPD (R$ 90.000): DPIA aprofundada, banner de consentimento avançado, contrato de operador (DPA Google), configuração *server-side* para reduzir *fingerprinting*.
- Riscos (R$ 60.000): sanções regulatórias em precedente europeu (CNIL, Garante Italiano, DPA Austríaca declararam GA em desconformidade com GDPR em 2022–2023 sem *server-side proxy*).

Trade-off crítico: **soberania de dado inexistente** — dado bruto reside em infraestrutura Google (EUA/UE), sob CLOUD Act.

### 4.7 Adobe Analytics

TCO estimado a partir de **R$ 4,67 M** em 5 anos. Contratação enterprise com preço confidencial (referência: US$ 100k–500k anuais em contratos comparáveis). Fora do envelope orçamentário típico da administração estadual para essa finalidade.

### 4.8 Microsoft Clarity

TCO baixo (R$ 66.000). **Utilidade limitada** — nicho de UX diagnostics. Não substitui plataforma primária.

### 4.9 Cloudflare Web Analytics

TCO **mais baixo** do estudo (R$ 24.000), mas **entrega funcional insuficiente**. Serve como complemento a portais já atrás da CDN Cloudflare.

### 4.10 Simple Analytics

TCO baixo (R$ 48.000), mas **cobertura funcional insuficiente** (sem funil, heatmap, form analytics) e cobrança em dólar via cartão internacional.

### 4.11 Open Web Analytics

TCO **acima** de Matomo Cloud, com **cobertura funcional inferior** e **projeto estagnado** (releases esparsas). Não recomendado.

---

## 5. Análise de sensibilidade

### 5.1 Sensibilidade ao volume

Como o TCO da plataforma primária muda se o volume dobrar (1 M pv/mês)?

| Plataforma | TCO 5 anos @ 500k | TCO 5 anos @ 1 M | Variação |
|-----------|:-----------------:|:----------------:|---------:|
| Matomo On-Premise | R$ 570.000 | R$ 630.000 | +11 % |
| Matomo Cloud | R$ 400.000 | R$ 620.000 | +55 % |
| Plausible Community | R$ 300.000 | R$ 330.000 | +10 % |
| Plausible Cloud | R$ 365.000 | R$ 555.000 | +52 % |
| GA4 gratuito | R$ 270.000 | R$ 270.000 | 0 % |
| PostHog OSS+EE | R$ 1.540.000 | R$ 1.620.000 | +5 % |
| PostHog Cloud EU | R$ 900.000 | R$ 1.500.000 | +67 % |

> **📌 Observação — SaaS escala em custo, auto-hospedado escala em infraestrutura**
> Auto-hospedadas absorvem volume com pouco impacto no TCO (o custo marginal é apenas mais infraestrutura). SaaS crescem quase linearmente com o volume. Esse é um argumento estrutural para **auto-hospedagem em cenário de crescimento previsto** — como é o caso do parque de portais estaduais.

### 5.2 Sensibilidade ao câmbio (para SaaS estrangeiros)

| Plataforma | R$ 5,00 / US$ | R$ 5,40 / US$ | R$ 6,00 / US$ | R$ 7,00 / US$ |
|-----------|--------------:|--------------:|--------------:|--------------:|
| Simple Analytics | R$ 44.500 | R$ 48.000 | R$ 53.500 | R$ 62.500 |
| PostHog Cloud EU | R$ 830.000 | R$ 900.000 | R$ 1.010.000 | R$ 1.180.000 |
| GA4 360 | R$ 2.620.000 | R$ 2.830.000 | R$ 3.150.000 | R$ 3.680.000 |
| Adobe Analytics | R$ 4.330.000 | R$ 4.670.000 | R$ 5.200.000 | R$ 6.070.000 |

Auto-hospedadas (Matomo On-Premise, Plausible CE, Umami, PostHog OSS) são **imunes** ao câmbio em componentes de licenciamento (custam R$ 0). Sofrem impacto apenas em infraestrutura, se hospedada em cloud pública internacional.

---

## 6. Custos ocultos por categoria

### 6.1 Custos ocultos por plataforma

| Plataforma | Custos ocultos identificados |
|-----------|-------------------------------|
| **Matomo** | Ajuste de arquivamento em alto volume; upgrades majors quebrando plugins; plugins comerciais opcionais para heatmap/funnel/A-B |
| **Plausible CE** | Sem funil visual (necessita produto complementar para requisitos avançados); banco ClickHouse exige competência específica |
| **Umami** | Sem funil, sem heatmap — cobre requisito básico apenas |
| **PostHog** | Kubernetes + Kafka + ClickHouse = operação cara; EE necessária para SSO e RBAC |
| **GA4 gratuito** | Custo regulatório LGPD; risco de sanção; adaptação a *server-side tagging* |
| **Adobe Analytics** | Consultoria de implantação e treinamento representam multiplicador de 1,5× a 2× sobre o licenciamento |
| **Simple Analytics** | Contratação em dólar via cartão — fricção contratual pública |
| **Microsoft Clarity** | Adequação LGPD para transferência internacional; retenção fixa de 30 dias |
| **Cloudflare** | Dependência estrutural da CDN Cloudflare |
| **Open Web Analytics** | Divergência de projeto (estagnação) — risco de precisar migrar antes do horizonte de 5 anos |

### 6.2 Custos ocultos por categoria transversal

| Categoria | Onde tende a aparecer |
|-----------|-----------------------|
| Consultoria externa de implantação | Adobe Analytics, PostHog, Matomo com plugins avançados |
| Câmbio | Toda plataforma SaaS estrangeira |
| Treinamento de equipe | PostHog (curva alta), Adobe (produto complexo) |
| Adequação regulatória | GA4, Clarity, Cloudflare — jurisdição EUA |
| Migração / retorno atrás | Open Web Analytics (estagnação), plataformas MIT sem cláusula de reciprocidade |
| Plugins pagos | Matomo (Heatmaps, Funnels, Form Analytics, A/B, Media, Custom Reports) |

---

## 7. Recomendação de contratação

### 7.1 Cenário-base

> **✅ Bloco de Decisão — cenário-base para orçamento**
>
> Provisionar **R$ 660.000** ao longo de 5 anos para operação do **Matomo On-Premise com plugins comerciais essenciais** (Heatmaps & Session Recording, Funnels, Form Analytics), acrescido de:
>
> - Camada complementar Plausible Community Edition para portais de baixa criticidade (~R$ 300.000 se em instância dedicada, ou compartilhamento de infraestrutura reduzindo esse número em 40 %);
> - Provisão condicionada de licenças pontuais para **complementos** conforme necessidade real (Clarity/Cloudflare — custos abaixo de R$ 100.000 em 5 anos).

### 7.2 Cenário conservador

Manter Matomo On-Premise **sem plugins comerciais** (usar solução própria de heatmap OSS ou dispensar o recurso). TCO reduzido a **R$ 570.000** em 5 anos, com renúncia às capacidades avançadas.

### 7.3 Cenário SaaS (contingência)

Migração para Matomo Cloud (hospedagem UE, DPA em conformidade GDPR). TCO **R$ 400.000** em 5 anos, com perda de soberania de dado e ganho de operação zero. Aplicável em cenário de restrição de FTE técnico ou incidente de infraestrutura estadual prolongado.

---

Referências primárias de preço: [`docs/12-referencias.md`](../docs/12-referencias.md).
Detalhamento por plataforma: pastas [`plataformas/`](../plataformas/).
