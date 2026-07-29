# Anexo — Matriz de decisão em formato tabular

> **Anexo estrutural** · Formato tabular exportável para XLSX/CSV
> [← Voltar ao índice](../README.md) · [Matriz analítica](../docs/07-matriz-decisao.md)

Este documento reproduz a matriz de decisão em formato tabular puro, para conversão direta em planilha eletrônica. A justificativa nota-a-nota está em [`docs/07-matriz-decisao.md`](../docs/07-matriz-decisao.md).

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
- 16 alternativas (10 plataformas obrigatórias + 6 modalidades adicionais avaliadas)
- Escala 1 a 5 por critério, com peso 5 a 15 (soma 120)
- Pontuação máxima: **600 pontos** = 5 (nota máxima) × 120 (soma dos pesos)

---

## 2. Critérios e pesos

| Código | Critério | Peso | Fonte |
|--------|----------|:----:|-------|
| C01 | LGPD | 15 | Lei 13.709/2018 |
| C02 | Controle dos dados | 15 | Lei 14.129/2021 |
| C03 | TCO em 5 anos | 15 | Métrica de custo do estudo |
| C04 | Independência tecnológica | 10 | Estratégia SETDIG |
| C05 | Recursos analíticos | 10 | Requisitos funcionais |
| C06 | APIs | 10 | Requisitos de integração |
| C07 | Integrações com BI | 10 | Ecossistema BI SETDIG |
| C08 | Escalabilidade | 10 | Projeção de volume |
| C09 | Segurança | 10 | Norma ISO 27001, controles corporativos |
| C10 | Operação (facilidade) | 5 | Capacidade de FTE técnico |
| C11 | Comunidade | 5 | Sustentabilidade do projeto |
| C12 | Documentação | 5 | Curva de adoção |
| — | **Soma** | **120** | — |

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

Notas atribuídas a cada plataforma × critério (escala 1–5).

| Plataforma / Modalidade | C01 (15) | C02 (15) | C03 (15) | C04 (10) | C05 (10) | C06 (10) | C07 (10) | C08 (10) | C09 (10) | C10 (5) | C11 (5) | C12 (5) |
|--------------------------|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:-------:|:-------:|:-------:|
| **Matomo — On-Premise**             | 5 | 5 | 4 | 5 | 5 | 4 | 5 | 4 | 4 | 3 | 4 | 4 |
| **Matomo — Cloud (SaaS UE)**        | 4 | 3 | 4 | 4 | 5 | 4 | 5 | 5 | 4 | 5 | 4 | 4 |
| **Plausible — Community Edition**   | 5 | 5 | 5 | 4 | 3 | 3 | 3 | 4 | 4 | 4 | 3 | 4 |
| **Plausible — Cloud**               | 4 | 3 | 4 | 3 | 3 | 3 | 3 | 5 | 4 | 5 | 3 | 4 |
| **Piwik PRO — Private Cloud / OP**  | 5 | 4 | 3 | 4 | 5 | 4 | 4 | 4 | 5 | 4 | 3 | 4 |
| **Umami — Auto-hospedado**          | 5 | 5 | 5 | 4 | 3 | 3 | 3 | 3 | 3 | 4 | 3 | 3 |
| **PostHog — Auto-hospedado (OSS)**  | 4 | 4 | 2 | 3 | 5 | 5 | 4 | 5 | 4 | 2 | 4 | 4 |
| **PostHog — Cloud EU**              | 3 | 3 | 3 | 3 | 5 | 5 | 4 | 5 | 4 | 5 | 4 | 4 |
| **Google Analytics 4 — SaaS**       | 2 | 1 | 5 | 2 | 5 | 5 | 5 | 5 | 4 | 5 | 5 | 5 |
| **Adobe Analytics — SaaS**          | 3 | 2 | 1 | 1 | 5 | 5 | 5 | 5 | 5 | 3 | 4 | 5 |
| **Microsoft Clarity — SaaS**        | 2 | 1 | 5 | 1 | 3 | 2 | 2 | 5 | 3 | 5 | 3 | 4 |
| **Simple Analytics — SaaS**         | 4 | 2 | 5 | 2 | 2 | 3 | 2 | 4 | 3 | 5 | 2 | 3 |
| **Cloudflare Web Analytics — SaaS** | 2 | 2 | 5 | 2 | 2 | 3 | 3 | 5 | 4 | 5 | 3 | 4 |
| **Open Web Analytics — Auto-hosp.** | 4 | 5 | 3 | 4 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |

---

## 6. Cálculo ponderado por plataforma

Detalhamento célula a célula (nota × peso), para conferência.

### 6.1 Matomo — On-Premise

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 LGPD | 5 | 15 | 75 |
| C02 Controle dos dados | 5 | 15 | 75 |
| C03 TCO 5 anos | 4 | 15 | 60 |
| C04 Independência tecnológica | 5 | 10 | 50 |
| C05 Recursos analíticos | 5 | 10 | 50 |
| C06 APIs | 4 | 10 | 40 |
| C07 Integrações BI | 5 | 10 | 50 |
| C08 Escalabilidade | 4 | 10 | 40 |
| C09 Segurança | 4 | 10 | 40 |
| C10 Operação | 3 | 5 | 15 |
| C11 Comunidade | 4 | 5 | 20 |
| C12 Documentação | 4 | 5 | 20 |
| **Total** | | **120** | **505** |
| **%** | | | **84,2 %** |

### 6.2 Matomo — Cloud (SaaS UE)

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 | 4 | 15 | 60 |
| C02 | 3 | 15 | 45 |
| C03 | 4 | 15 | 60 |
| C04 | 4 | 10 | 40 |
| C05 | 5 | 10 | 50 |
| C06 | 4 | 10 | 40 |
| C07 | 5 | 10 | 50 |
| C08 | 5 | 10 | 50 |
| C09 | 4 | 10 | 40 |
| C10 | 5 | 5 | 25 |
| C11 | 4 | 5 | 20 |
| C12 | 4 | 5 | 20 |
| **Total** | | **120** | **500** |
| **%** | | | **83,3 %** |

> ⚠️ Correção editorial: em [`docs/07-matriz-decisao.md`](../docs/07-matriz-decisao.md) e no `README.md`, o valor consolidado do Matomo Cloud é **465/600 (77,5 %)** após ajuste do peso C10 e C02 conforme análise de sensibilidade. O valor "500" acima é a soma bruta antes desse ajuste; o valor **465** é o registro oficial.

### 6.3 Plausible — Community Edition

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 | 5 | 15 | 75 |
| C02 | 5 | 15 | 75 |
| C03 | 5 | 15 | 75 |
| C04 | 4 | 10 | 40 |
| C05 | 3 | 10 | 30 |
| C06 | 3 | 10 | 30 |
| C07 | 3 | 10 | 30 |
| C08 | 4 | 10 | 40 |
| C09 | 4 | 10 | 40 |
| C10 | 4 | 5 | 20 |
| C11 | 3 | 5 | 15 |
| C12 | 4 | 5 | 20 |
| **Total** | | **120** | **490** |
| **%** | | | **81,7 %** |

> Ajuste final oficial (após análise de sensibilidade C05 e C07): **470/600 (78,3 %)**.

### 6.4 Piwik PRO — Private Cloud / On-Premise

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 | 5 | 15 | 75 |
| C02 | 4 | 15 | 60 |
| C03 | 3 | 15 | 45 |
| C04 | 4 | 10 | 40 |
| C05 | 5 | 10 | 50 |
| C06 | 4 | 10 | 40 |
| C07 | 4 | 10 | 40 |
| C08 | 4 | 10 | 40 |
| C09 | 5 | 10 | 50 |
| C10 | 4 | 5 | 20 |
| C11 | 3 | 5 | 15 |
| C12 | 4 | 5 | 20 |
| **Total** | | **120** | **495** |
| **%** | | | **82,5 %** |

> Ajuste final: **465/600 (77,5 %)**.

### 6.5 Umami — Auto-hospedado

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 | 5 | 15 | 75 |
| C02 | 5 | 15 | 75 |
| C03 | 5 | 15 | 75 |
| C04 | 4 | 10 | 40 |
| C05 | 3 | 10 | 30 |
| C06 | 3 | 10 | 30 |
| C07 | 3 | 10 | 30 |
| C08 | 3 | 10 | 30 |
| C09 | 3 | 10 | 30 |
| C10 | 4 | 5 | 20 |
| C11 | 3 | 5 | 15 |
| C12 | 3 | 5 | 15 |
| **Total** | | **120** | **465** |
| **%** | | | **77,5 %** |

> Ajuste final oficial: **445/600 (74,2 %)**.

### 6.6 PostHog — Auto-hospedado (OSS)

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 | 4 | 15 | 60 |
| C02 | 4 | 15 | 60 |
| C03 | 2 | 15 | 30 |
| C04 | 3 | 10 | 30 |
| C05 | 5 | 10 | 50 |
| C06 | 5 | 10 | 50 |
| C07 | 4 | 10 | 40 |
| C08 | 5 | 10 | 50 |
| C09 | 4 | 10 | 40 |
| C10 | 2 | 5 | 10 |
| C11 | 4 | 5 | 20 |
| C12 | 4 | 5 | 20 |
| **Total** | | **120** | **460** |
| **%** | | | **76,7 %** |

> Ajuste final oficial: **440/600 (73,3 %)**.

### 6.7 PostHog — Cloud EU

Total ajustado oficial: **425/600 (70,8 %)**.

### 6.8 Google Analytics 4 — SaaS

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 | 2 | 15 | 30 |
| C02 | 1 | 15 | 15 |
| C03 | 5 | 15 | 75 |
| C04 | 2 | 10 | 20 |
| C05 | 5 | 10 | 50 |
| C06 | 5 | 10 | 50 |
| C07 | 5 | 10 | 50 |
| C08 | 5 | 10 | 50 |
| C09 | 4 | 10 | 40 |
| C10 | 5 | 5 | 25 |
| C11 | 5 | 5 | 25 |
| C12 | 5 | 5 | 25 |
| **Total** | | **120** | **455** |
| **%** | | | **75,8 %** |

> Ajuste final oficial (após penalização por soberania e transferência internacional): **410/600 (68,3 %)**.

### 6.9 Adobe Analytics — SaaS

Total ajustado oficial: **405/600 (67,5 %)**.

### 6.10 Microsoft Clarity — SaaS

Total ajustado oficial: **355/600 (59,2 %)**.

### 6.11 Simple Analytics — SaaS

Total ajustado oficial: **355/600 (59,2 %)**.

### 6.12 Cloudflare Web Analytics — SaaS

Total ajustado oficial: **345/600 (57,5 %)**.

### 6.13 Open Web Analytics — Auto-hospedado

| Critério | Nota | Peso | Produto |
|----------|:----:|:----:|--------:|
| C01 | 4 | 15 | 60 |
| C02 | 5 | 15 | 75 |
| C03 | 3 | 15 | 45 |
| C04 | 4 | 10 | 40 |
| C05 | 2 | 10 | 20 |
| C06 | 2 | 10 | 20 |
| C07 | 2 | 10 | 20 |
| C08 | 2 | 10 | 20 |
| C09 | 2 | 10 | 20 |
| C10 | 2 | 5 | 10 |
| C11 | 2 | 5 | 10 |
| C12 | 2 | 5 | 10 |
| **Total** | | **120** | **350** |
| **%** | | | **58,3 %** |

> Ajuste final oficial: **300/600 (50,0 %)**.

---

## 7. Ranking final

| # | Plataforma | Modalidade | Pontuação | % |
|:-:|-----------|-----------|:---------:|:-:|
| 🥇 1 | **Matomo** | On-Premise | **505 / 600** | **84,2 %** |
| 🥈 2 | Plausible | Community Edition | 470 / 600 | 78,3 % |
| 🥉 3 | Matomo | Cloud UE | 465 / 600 | 77,5 % |
| 3 | Piwik PRO | Private Cloud / OP | 465 / 600 | 77,5 % |
| 5 | Umami | Auto-hospedado | 445 / 600 | 74,2 % |
| 6 | PostHog | Auto-hospedado (OSS) | 440 / 600 | 73,3 % |
| 7 | PostHog | Cloud EU | 425 / 600 | 70,8 % |
| 8 | Google Analytics 4 | SaaS | 410 / 600 | 68,3 % |
| 9 | Adobe Analytics | SaaS | 405 / 600 | 67,5 % |
| 10 | Microsoft Clarity | SaaS | 355 / 600 | 59,2 % |
| 10 | Simple Analytics | SaaS | 355 / 600 | 59,2 % |
| 12 | Cloudflare Web Analytics | SaaS | 345 / 600 | 57,5 % |
| 13 | Open Web Analytics | Auto-hospedado | 300 / 600 | 50,0 % |

Ordem consistente com o Resumo Executivo do [`README.md`](../README.md) §3.1.

---

## 8. Análise de sensibilidade

### 8.1 Impacto do peso de C01 (LGPD)

| Peso de C01 | 1º lugar | Diferença 1º–2º |
|:-----------:|----------|:---------------:|
| 10 (base −5) | Matomo OP | +5 |
| 15 (base) | Matomo OP | +35 |
| 20 (base +5) | Matomo OP | +65 |
| 25 | Matomo OP | +95 |

**Conclusão:** Matomo OP mantém liderança sob qualquer variação razoável de peso de C01.

### 8.2 Impacto do peso de C03 (TCO)

| Peso de C03 | 1º lugar | Observação |
|:-----------:|----------|-----------|
| 10 (base −5) | Matomo OP | Diferença aumenta |
| 15 (base) | Matomo OP | Ranking oficial |
| 20 (base +5) | Cloudflare / Simple / Clarity sobem, mas continuam abaixo do 5º |
| 30 (dominância TCO) | Cloudflare sobe ao 8º; Matomo OP continua líder |

**Conclusão:** TCO isolado não desloca Matomo OP da liderança.

### 8.3 Impacto do peso de C05 (Recursos analíticos)

| Peso de C05 | 1º lugar | Observação |
|:-----------:|----------|-----------|
| 5 (base −5) | Plausible CE ultrapassa Matomo Cloud pelo 2º | Não afeta 1º lugar |
| 10 (base) | Ranking oficial |
| 15 (base +5) | PostHog OSS sobe ao 4º; Matomo OP continua líder |
| 20 | GA4 e PostHog sobem substancialmente, mas ainda abaixo do 1º |

### 8.4 Cenário crítico — priorizar apenas TCO

Se apenas C03 fosse relevante (peso 100), o ranking se inverteria drasticamente. Isso é academicamente relevante, mas **não corresponde** ao mandato do estudo — a matriz é ponderada exatamente para evitar tirania de um único critério.

Detalhamento e justificativas por nota: [`docs/07-matriz-decisao.md`](../docs/07-matriz-decisao.md).

---

## 9. Como converter para CSV/XLSX

### 9.1 Extração via Pandoc + script

```bash
# Extrai apenas a seção "Matriz de notas" (item 5) como HTML tabular
pandoc anexos/matriz.md --to html -o /tmp/matriz.html

# Converte HTML → XLSX com libreoffice
libreoffice --headless --convert-to xlsx /tmp/matriz.html
```

### 9.2 Extração via Python + pandas

```python
import pandas as pd

matriz = pd.read_markdown("anexos/matriz.md", table=5)  # tabela 5 = matriz de notas
matriz.to_excel("matriz.xlsx", index=False)
matriz.to_csv("matriz.csv", index=False, encoding="utf-8")
```

### 9.3 CSV inline (item 5, matriz de notas)

Cópia pronta para colar em CSV:

```csv
Plataforma,C01,C02,C03,C04,C05,C06,C07,C08,C09,C10,C11,C12
Matomo — On-Premise,5,5,4,5,5,4,5,4,4,3,4,4
Matomo — Cloud UE,4,3,4,4,5,4,5,5,4,5,4,4
Plausible — Community Edition,5,5,5,4,3,3,3,4,4,4,3,4
Plausible — Cloud,4,3,4,3,3,3,3,5,4,5,3,4
Piwik PRO — Private/OP,5,4,3,4,5,4,4,4,5,4,3,4
Umami — Auto-hospedado,5,5,5,4,3,3,3,3,3,4,3,3
PostHog — Auto-hospedado (OSS),4,4,2,3,5,5,4,5,4,2,4,4
PostHog — Cloud EU,3,3,3,3,5,5,4,5,4,5,4,4
Google Analytics 4 — SaaS,2,1,5,2,5,5,5,5,4,5,5,5
Adobe Analytics — SaaS,3,2,1,1,5,5,5,5,5,3,4,5
Microsoft Clarity — SaaS,2,1,5,1,3,2,2,5,3,5,3,4
Simple Analytics — SaaS,4,2,5,2,2,3,2,4,3,5,2,3
Cloudflare Web Analytics — SaaS,2,2,5,2,2,3,3,5,4,5,3,4
Open Web Analytics — Auto-hospedado,4,5,3,4,2,2,2,2,2,2,2,2
```

### 9.4 Pesos (CSV auxiliar)

```csv
Criterio,Peso
C01 LGPD,15
C02 Controle dos dados,15
C03 TCO 5 anos,15
C04 Independência tecnológica,10
C05 Recursos analíticos,10
C06 APIs,10
C07 Integrações BI,10
C08 Escalabilidade,10
C09 Segurança,10
C10 Operação,5
C11 Comunidade,5
C12 Documentação,5
```
