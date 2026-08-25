# Anexo — Matriz de decisão em formato tabular

> **Anexo estrutural** · Formato tabular exportável para XLSX/CSV
> [← Voltar ao índice](../README.md) · [Matriz analítica](../docs/07-matriz-decisao.md)

Este documento reproduz a matriz de decisão em formato tabular puro, para conversão direta em planilha eletrônica. A justificativa nota-a-nota está em [`docs/07-matriz-decisao.md`](../docs/07-matriz-decisao.md).

> **📌 Nota metodológica — Revisão 2026**
> Este anexo reflete os **pesos revisados** homologados no [ADR-002](../docs/08-adr-002.md): C03 (TCO) rebaixado a **peso 0** (informativo) e 15 pontos redistribuídos entre C05, C06, C08 (+3 cada), C09 (+2), C10 (+2) e C11 (+2). A soma dos pesos permanece 120. As notas atribuídas foram preservadas — só o peso mudou. Os totais do ADR-001 continuam impressos ao lado dos revisados para rastreabilidade e auditoria.

---

## Sumário

- [1. Estrutura da matriz](#1-estrutura-da-matriz)
- [2. Critérios e pesos](#2-critérios-e-pesos)
- [3. Escala de notas](#3-escala-de-notas)
- [4. Fórmula de agregação](#4-fórmula-de-agregação)
- [5. Matriz de notas](#5-matriz-de-notas)
- [6. Cálculo ponderado por plataforma](#6-cálculo-ponderado-por-plataforma)
- [7. Ranking final](#7-ranking-final)
- [8. Análise de sensibilidade](#8-análise-de-sensibilidade)
- [9. Como converter para CSV/XLSX](#9-como-converter-para-csvxlsx)

---

## 1. Estrutura da matriz

- 12 critérios (C01 a C12)
- 13 alternativas (10 plataformas obrigatórias + 3 modalidades adicionais avaliadas)
- Escala 1 a 5 por critério, com peso 0 a 15 (soma 120)
- Pontuação máxima: **600 pontos** = 5 (nota máxima) × 120 (soma dos pesos)

---

## 2. Critérios e pesos

| Código | Critério | Peso ADR-001 | **Peso revisado (ADR-002)** | Fonte |
|--------|----------|:------------:|:---------------------------:|-------|
| C01 | LGPD | 15 | **15** | Lei 13.709/2018 |
| C02 | Controle dos dados | 15 | **15** | Lei 14.129/2021 |
| C03 | TCO em 5 anos *(informativo)* | 15 | **0** | Métrica de custo do estudo |
| C04 | Independência tecnológica | 10 | **10** | Estratégia SETDIG |
| C05 | Recursos analíticos | 10 | **13** | Requisitos funcionais |
| C06 | APIs | 10 | **13** | Requisitos de integração |
| C07 | Integrações com BI | 10 | **10** | Ecossistema BI SETDIG |
| C08 | Escalabilidade | 10 | **13** | Projeção de volume |
| C09 | Segurança | 10 | **12** | Norma ISO 27001, controles corporativos |
| C10 | Operação (facilidade) | 5 | **7** | Capacidade de FTE técnico |
| C11 | Comunidade | 5 | **7** | Sustentabilidade do projeto |
| C12 | Documentação | 5 | **5** | Curva de adoção |
| — | **Soma** | **120** | **120** | — |

---

## 3. Escala de notas

| Nota | Interpretação |
|:----:|---------------|
| 5 | Excelente — supera claramente os requisitos |
| 4 | Bom — atende aos requisitos com folga |
| 3 | Aceitável — atende ao mínimo requerido |
| 2 | Insuficiente — atende parcialmente |
| 1 | Reprovado — não atende |

Não há meia-nota. Cada nota tem justificativa textual obrigatória em [`docs/07-matriz-decisao.md`](../docs/07-matriz-decisao.md).

---

## 4. Fórmula de agregação

```
Pontuação(plataforma) = Σ (nota_i × peso_i)  para i = C01 até C12
Percentual(plataforma) = Pontuação / 600 × 100
```

Modelo MAUT (Multi-Attribute Utility Theory) — soma ponderada linear, sem transformação não-linear das notas.

---

## 5. Matriz de notas

Notas atribuídas a cada plataforma × critério (escala 1–5). Alinhadas com [`docs/07-matriz-decisao.md §2.1`](../docs/07-matriz-decisao.md#21-notas-atribuídas). Notas **inalteradas** desde o ADR-001; apenas os pesos foram revisados.

| Plataforma / Modalidade | C01 (15) | C02 (15) | C03 (0) | C04 (10) | C05 (13) | C06 (13) | C07 (10) | C08 (13) | C09 (12) | C10 (7) | C11 (7) | C12 (5) |
|--------------------------|:--------:|:--------:|:-------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:-------:|:-------:|:-------:|
| **Matomo — On-Premise**             | 5 | 5 | 5 | 5 | 4 | 4 | 4 | 3 | 4 | 3 | 4 | 4 |
| **Plausible — Community Edition**   | 5 | 5 | 5 | 4 | 3 | 3 | 3 | 4 | 4 | 3 | 3 | 4 |
| **Matomo — Cloud (SaaS UE)**        | 4 | 3 | 3 | 4 | 5 | 4 | 4 | 4 | 4 | 5 | 4 | 4 |
| **Piwik PRO — Private Cloud / OP**  | 5 | 4 | 2 | 2 | 5 | 4 | 4 | 5 | 5 | 4 | 2 | 4 |
| **PostHog — Auto-hospedado (OSS)**  | 4 | 5 | 3 | 3 | 5 | 4 | 4 | 3 | 4 | 1 | 4 | 4 |
| **Umami — Auto-hospedado**          | 5 | 5 | 5 | 4 | 2 | 3 | 2 | 3 | 3 | 4 | 3 | 3 |
| **PostHog — Cloud EU**              | 3 | 2 | 3 | 2 | 5 | 4 | 4 | 5 | 4 | 5 | 4 | 4 |
| **Google Analytics 4 — SaaS**       | 2 | 1 | 4 | 1 | 4 | 4 | 5 | 5 | 4 | 5 | 5 | 5 |
| **Adobe Analytics — SaaS**          | 3 | 2 | 1 | 1 | 5 | 5 | 5 | 5 | 5 | 4 | 3 | 4 |
| **Microsoft Clarity — SaaS**        | 2 | 1 | 5 | 1 | 3 | 2 | 3 | 5 | 4 | 5 | 3 | 3 |
| **Simple Analytics — SaaS**         | 4 | 2 | 3 | 2 | 2 | 3 | 2 | 4 | 4 | 5 | 2 | 3 |
| **Cloudflare Web Analytics — SaaS** | 3 | 1 | 5 | 1 | 1 | 3 | 2 | 5 | 4 | 5 | 2 | 3 |
| **Open Web Analytics — Auto-hosp.** | 4 | 5 | 5 | 3 | 2 | 2 | 1 | 1 | 1 | 2 | 1 | 1 |

---

## 6. Cálculo ponderado por plataforma (revisão 2026)

Detalhamento célula a célula (nota × peso revisado) para conferência. Coluna "Produto ADR-001" preservada para auditoria.

### 6.1 Matomo — On-Premise

| Critério | Nota | Peso revisado | Produto revisado | *Peso ADR-001* | *Produto ADR-001* |
|----------|:----:|:-------------:|-----------------:|:--------------:|------------------:|
| C01 LGPD | 5 | 15 | 75 | 15 | 75 |
| C02 Controle dos dados | 5 | 15 | 75 | 15 | 75 |
| C03 TCO 5 anos *(informativo)* | 5 | 0 | 0 | 15 | 75 |
| C04 Independência tecnológica | 5 | 10 | 50 | 10 | 50 |
| C05 Recursos analíticos | 4 | 13 | 52 | 10 | 40 |
| C06 APIs | 4 | 13 | 52 | 10 | 40 |
| C07 Integrações BI | 4 | 10 | 40 | 10 | 40 |
| C08 Escalabilidade | 3 | 13 | 39 | 10 | 30 |
| C09 Segurança | 4 | 12 | 48 | 10 | 40 |
| C10 Operação | 3 | 7 | 21 | 5 | 15 |
| C11 Comunidade | 4 | 7 | 28 | 5 | 20 |
| C12 Documentação | 4 | 5 | 20 | 5 | 20 |
| **Total** | | **120** | **500** | **120** | *520* |
| **%** | | | **83,3 %** | | *86,7 %* |

### 6.2 Piwik PRO — Private Cloud / On-Premise

| Critério | Nota | Peso | Produto | *Produto ADR-001* |
|----------|:----:|:----:|--------:|------------------:|
| C01 | 5 | 15 | 75 | 75 |
| C02 | 4 | 15 | 60 | 60 |
| C03 | 2 | 0 | 0 | 30 |
| C04 | 2 | 10 | 20 | 20 |
| C05 | 5 | 13 | 65 | 50 |
| C06 | 4 | 13 | 52 | 40 |
| C07 | 4 | 10 | 40 | 40 |
| C08 | 5 | 13 | 65 | 50 |
| C09 | 5 | 12 | 60 | 50 |
| C10 | 4 | 7 | 28 | 20 |
| C11 | 2 | 7 | 14 | 10 |
| C12 | 4 | 5 | 20 | 20 |
| **Total** | | **120** | **499** | *465* |
| **%** | | | **83,2 %** | *77,5 %* |

### 6.3 Matomo — Cloud (SaaS UE)

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 | 4 | 15 | 60 |
| C02 | 3 | 15 | 45 |
| C03 | 3 | 0 | 0 |
| C04 | 4 | 10 | 40 |
| C05 | 5 | 13 | 65 |
| C06 | 4 | 13 | 52 |
| C07 | 4 | 10 | 40 |
| C08 | 4 | 13 | 52 |
| C09 | 4 | 12 | 48 |
| C10 | 5 | 7 | 35 |
| C11 | 4 | 7 | 28 |
| C12 | 4 | 5 | 20 |
| **Total** | | **120** | **485** |
| **%** | | | **80,8 %** |

### 6.4 PostHog — Auto-hospedado (OSS)

| Critério | Nota | Peso | Produto | *Produto ADR-001* |
|----------|:----:|:----:|--------:|------------------:|
| C01 | 4 | 15 | 60 | 60 |
| C02 | 5 | 15 | 75 | 75 |
| C03 | 3 | 0 | 0 | 45 |
| C04 | 3 | 10 | 30 | 30 |
| C05 | 5 | 13 | 65 | 50 |
| C06 | 4 | 13 | 52 | 40 |
| C07 | 4 | 10 | 40 | 40 |
| C08 | 3 | 13 | 39 | 30 |
| C09 | 4 | 12 | 48 | 40 |
| C10 | 1 | 7 | 7 | 5 |
| C11 | 4 | 7 | 28 | 20 |
| C12 | 4 | 5 | 20 | 20 |
| **Total** | | **120** | **464** | *455* |
| **%** | | | **77,3 %** | *75,8 %* |

### 6.5 Plausible — Community Edition

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 | 5 | 15 | 75 |
| C02 | 5 | 15 | 75 |
| C03 | 5 | 0 | 0 |
| C04 | 4 | 10 | 40 |
| C05 | 3 | 13 | 39 |
| C06 | 3 | 13 | 39 |
| C07 | 3 | 10 | 30 |
| C08 | 4 | 13 | 52 |
| C09 | 4 | 12 | 48 |
| C10 | 3 | 7 | 21 |
| C11 | 3 | 7 | 21 |
| C12 | 4 | 5 | 20 |
| **Total** | | **120** | **460** |
| **%** | | | **76,7 %** |

### 6.6 Adobe Analytics — SaaS

Total revisado: **459 / 600 (76,5 %)** · Total ADR-001: *405 / 600 (67,5 %)*.

### 6.7 PostHog — Cloud EU

Total revisado: **448 / 600 (74,7 %)** · Total ADR-001: *425 / 600 (70,8 %)*.

### 6.8 Google Analytics 4 — SaaS

Total revisado: **417 / 600 (69,5 %)** · Total ADR-001: *410 / 600 (68,3 %)*.

### 6.9 Umami — Auto-hospedado

Total revisado: **414 / 600 (69,0 %)** · Total ADR-001: *445 / 600 (74,2 %)*. Queda deriva de C05=2 (cobertura funcional ganhando peso) sem compensação de C03=5 (peso zerado).

### 6.10 Simple Analytics — SaaS

Total revisado: **359 / 600 (59,8 %)** · Total ADR-001: *355 / 600 (59,2 %)*.

### 6.11 Microsoft Clarity — SaaS

Total revisado: **334 / 600 (55,7 %)** · Total ADR-001: *355 / 600 (59,2 %)*.

### 6.12 Cloudflare Web Analytics — SaaS

Total revisado: **319 / 600 (53,2 %)** · Total ADR-001: *345 / 600 (57,5 %)*.

### 6.13 Open Web Analytics — Auto-hospedado

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 | 4 | 15 | 60 |
| C02 | 5 | 15 | 75 |
| C03 | 5 | 0 | 0 |
| C04 | 3 | 10 | 30 |
| C05 | 2 | 13 | 26 |
| C06 | 2 | 13 | 26 |
| C07 | 1 | 10 | 10 |
| C08 | 1 | 13 | 13 |
| C09 | 1 | 12 | 12 |
| C10 | 2 | 7 | 14 |
| C11 | 1 | 7 | 7 |
| C12 | 1 | 5 | 5 |
| **Total** | | **120** | **278** |
| **%** | | | **46,3 %** |

**Reprovado por triagem eliminatória** (C09 = 1). Independente da ponderação.

---

## 7. Ranking final (revisão 2026)

| # | Plataforma | Modalidade | Pontuação revisada | % | *ADR-001* |
|:-:|-----------|-----------|:------------------:|:-:|:---------:|
| 🥇 1 | **Matomo** | On-Premise | **500 / 600** | **83,3 %** | *520 / 86,7 %* |
| 🥈 2 | Piwik PRO | Private Cloud / OP | 499 / 600 | 83,2 % | *465 / 77,5 %* |
| 🥉 3 | Matomo | Cloud UE | 485 / 600 | 80,8 % | *465 / 77,5 %* |
| 4 | PostHog | Auto-hospedado (OSS) | 464 / 600 | 77,3 % | *455 / 75,8 %* |
| 5 | Plausible | Community Edition | 460 / 600 | 76,7 % | *485 / 80,8 %* |
| 6 | Adobe Analytics | SaaS | 459 / 600 | 76,5 % | *405 / 67,5 %* |
| 7 | PostHog | Cloud EU | 448 / 600 | 74,7 % | *425 / 70,8 %* |
| 8 | Google Analytics 4 | SaaS | 417 / 600 | 69,5 % | *410 / 68,3 %* |
| 9 | Umami | Auto-hospedado | 414 / 600 | 69,0 % | *445 / 74,2 %* |
| 10 | Simple Analytics | SaaS | 359 / 600 | 59,8 % | *355 / 59,2 %* |
| 11 | Microsoft Clarity | SaaS | 334 / 600 | 55,7 % | *355 / 59,2 %* |
| 12 | Cloudflare Web Analytics | SaaS | 319 / 600 | 53,2 % | *345 / 57,5 %* |
| 13 | Open Web Analytics | Auto-hospedado | 278 / 600 | 46,3 % | *330 / 55,0 %* |

Ordem consistente com o Resumo Executivo do [`README.md`](../README.md) §3.1 e com [`docs/07-matriz-decisao.md §3.1`](../docs/07-matriz-decisao.md#31-ranking-final).

---

## 8. Análise de sensibilidade

### 8.1 Impacto do peso de C01 (LGPD)

| Peso de C01 | 1º lugar | Diferença 1º–2º |
|:-----------:|----------|:---------------:|
| 10 (base −5) | Matomo OP / Piwik PRO empate | 0 |
| 15 (base revisada) | Matomo OP | +1 |
| 20 (base +5) | Matomo OP | +6 |
| 25 | Matomo OP | +11 |

**Conclusão:** liderança de Matomo OP é sensível ao peso de C01 no piso, mas permanece pelo menos empatado no cenário mais adverso.

### 8.2 Impacto de C03 (TCO)

| Peso de C03 | 1º lugar | Observação |
|:-----------:|----------|-----------|
| 0 (base revisada) | Matomo OP | Ranking oficial |
| 15 (referência histórica ADR-001) | Matomo OP | +35 sobre Plausible; ranking do ADR-001 |
| 30 (dominância TCO) | Cloudflare / Clarity sobem, Matomo OP continua líder |

**Conclusão:** reintroduzir C03 não altera a liderança do Matomo OP em nenhum cenário testado.

### 8.3 Impacto de C05 (Recursos analíticos)

| Peso de C05 | 1º lugar | Observação |
|:-----------:|----------|-----------|
| 8 (base −5) | Matomo OP | Vantagem sobe |
| 13 (base revisada) | Matomo OP | Ranking oficial (margem 1 pt sobre Piwik PRO) |
| 18 (base +5) | Piwik PRO / Matomo Cloud empatam com Matomo OP | Margem se dilui |
| 25 | Piwik PRO ultrapassa | Cenário "Capacidade analítica" |

**Conclusão:** liderança do Matomo OP é sensível a aumento adicional de C05. Consistente com o cenário "Capacidade analítica" documentado em [`docs/07-matriz-decisao.md §5`](../docs/07-matriz-decisao.md#5-análise-de-sensibilidade).

---

## 9. Como converter para CSV/XLSX

### 9.1 CSV inline — matriz de notas (inalterada)

```csv
Plataforma,C01,C02,C03,C04,C05,C06,C07,C08,C09,C10,C11,C12
Matomo — On-Premise,5,5,5,5,4,4,4,3,4,3,4,4
Plausible — Community Edition,5,5,5,4,3,3,3,4,4,3,3,4
Matomo — Cloud UE,4,3,3,4,5,4,4,4,4,5,4,4
Piwik PRO — Private/OP,5,4,2,2,5,4,4,5,5,4,2,4
PostHog — Auto-hospedado (OSS),4,5,3,3,5,4,4,3,4,1,4,4
Umami — Auto-hospedado,5,5,5,4,2,3,2,3,3,4,3,3
PostHog — Cloud EU,3,2,3,2,5,4,4,5,4,5,4,4
Google Analytics 4 — SaaS,2,1,4,1,4,4,5,5,4,5,5,5
Adobe Analytics — SaaS,3,2,1,1,5,5,5,5,5,4,3,4
Microsoft Clarity — SaaS,2,1,5,1,3,2,3,5,4,5,3,3
Simple Analytics — SaaS,4,2,3,2,2,3,2,4,4,5,2,3
Cloudflare Web Analytics — SaaS,3,1,5,1,1,3,2,5,4,5,2,3
Open Web Analytics — Auto-hospedado,4,5,5,3,2,2,1,1,1,2,1,1
```

### 9.2 CSV inline — pesos revisados (ADR-002)

```csv
Criterio,Peso_ADR001,Peso_Revisado
C01 LGPD,15,15
C02 Controle dos dados,15,15
C03 TCO 5 anos,15,0
C04 Independência tecnológica,10,10
C05 Recursos analíticos,10,13
C06 APIs,10,13
C07 Integrações BI,10,10
C08 Escalabilidade,10,13
C09 Segurança,10,12
C10 Operação,5,7
C11 Comunidade,5,7
C12 Documentação,5,5
```

### 9.3 CSV inline — totais consolidados

```csv
Plataforma,Total_Revisado,Percent_Revisado,Total_ADR001,Percent_ADR001
Matomo — On-Premise,500,83.3,520,86.7
Piwik PRO — Private/OP,499,83.2,465,77.5
Matomo — Cloud UE,485,80.8,465,77.5
PostHog — Auto-hospedado (OSS),464,77.3,455,75.8
Plausible — Community Edition,460,76.7,485,80.8
Adobe Analytics — SaaS,459,76.5,405,67.5
PostHog — Cloud EU,448,74.7,425,70.8
Google Analytics 4 — SaaS,417,69.5,410,68.3
Umami — Auto-hospedado,414,69.0,445,74.2
Simple Analytics — SaaS,359,59.8,355,59.2
Microsoft Clarity — SaaS,334,55.7,355,59.2
Cloudflare Web Analytics — SaaS,319,53.2,345,57.5
Open Web Analytics — Auto-hospedado,278,46.3,330,55.0
```
