# Comparativo — Governança, soberania e vendor lock-in

> **Corte transversal** · Foco: controle dos dados, portabilidade, transparência, auditoria, soberania
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/07-matriz-decisao.md)

---

## Sumário

- [1. Dimensões de governança](#1-dimensões-de-governança)
- [2. Matriz sinóptica](#2-matriz-sinóptica)
- [3. Vendor lock-in](#3-vendor-lock-in)
- [4. Controle dos dados](#4-controle-dos-dados)
- [5. Portabilidade](#5-portabilidade)
- [6. Transparência](#6-transparência)
- [7. Auditabilidade](#7-auditabilidade)
- [8. Soberania de dados](#8-soberania-de-dados)
- [9. Alinhamento com princípios de Governo Digital](#9-alinhamento-com-princípios-de-governo-digital)
- [10. Recomendação](#10-recomendação)

---

## 1. Dimensões de governança

Governança aqui é entendida na perspectiva do **controlador** (Estado) — não do fornecedor. Sete dimensões:

| Dimensão | Pergunta que responde |
|----------|-----------------------|
| Vendor lock-in | Se eu quiser sair, quanto custa? |
| Controle dos dados | Quem está com meu dado neste momento? |
| Portabilidade | Consigo levar meu dado embora? |
| Transparência | Consigo auditar o que o produto faz? |
| Auditabilidade | Consigo provar depois quem acessou o quê? |
| Soberania | Sob qual jurisdição meu dado está? |
| Governo Digital | Está alinhado aos princípios da EGD e Lei 14.129/2021? |

---

## 2. Matriz sinóptica

| Plataforma | Modalidade | Lock-in | Controle | Portabilidade | Transparência | Auditabilidade | Soberania | Score global |
|-----------|-----------|:-------:|:--------:|:-------------:|:-------------:|:--------------:|:---------:|:------------:|
| **Matomo** | On-Premise | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | ★★★★★ |
| Matomo | Cloud UE | 🟠 | 🟠 | 🟢 | 🟢 | 🟠 | 🟠 | ★★★★ |
| Plausible CE | Auto-hospedado | 🟢 | 🟢 | 🟠 | 🟢 | 🟢 | 🟢 | ★★★★ |
| Plausible | Cloud | 🟠 | 🟠 | 🟠 | 🟢 | 🟠 | 🟠 | ★★★ |
| Umami | Auto-hospedado | 🟠 | 🟢 | 🟠 | 🟢 | 🟠 | 🟢 | ★★★ |
| PostHog | OSS auto-hospedado | 🟠 | 🟢 | 🟢 | 🟠 (EE fechada) | 🟠 (EE) | 🟢 | ★★★ |
| PostHog | Cloud EU | 🟠 | 🟠 | 🟢 | 🟠 | 🟢 | 🟠 | ★★★ |
| **GA4** | SaaS | 🔴 | 🔴 | 🟠 (BigQuery) | 🔴 | 🟢 (Google Workspace) | 🔴 | ★ |
| Adobe Analytics | SaaS enterprise | 🔴 | 🔴 | 🟠 | 🔴 | 🟢 | 🟠 (contratual) | ★★ |
| Simple Analytics | SaaS | 🟠 | 🟠 | 🟠 | 🟠 | 🟠 | 🟠 (UE) | ★★★ |
| **Microsoft Clarity** | SaaS | 🔴 | 🔴 | 🔴 | 🟠 (só biblioteca é MIT) | 🔴 | 🔴 | ★ |
| Cloudflare Web Analytics | SaaS | 🔴 | 🔴 | 🔴 | 🔴 | 🟢 (Cloudflare Enterprise) | 🔴 | ★ |
| Open Web Analytics | Auto-hospedado | 🟢 | 🟢 | 🟢 | 🟢 | 🟠 | 🟢 | ★★★ (mas projeto estagnado) |

Legenda: 🟢 forte / 🟠 aceitável / 🔴 fraco.

---

## 3. Vendor lock-in

### 3.1 Vetores de lock-in

| Vetor | Descrição | Plataformas afetadas |
|-------|-----------|----------------------|
| **Licença proprietária** | Código fechado, não replicável | GA4, Adobe, Clarity, Simple, Cloudflare |
| **Formato de dado proprietário** | Dado bruto não exportável em formato aberto | Clarity (sessões), Cloudflare (edge logs), Adobe (parcialmente) |
| **Dependência de sub-serviços** | Requer outros produtos do mesmo fornecedor para valor pleno | GA4 (Google Marketing Suite), Adobe (Experience Cloud), Cloudflare (CDN) |
| **Custo de saída** | Migração de histórico + reeducação de equipe + retreinamento de dashboards | Todas — mas mais grave em GA4 e Adobe |
| **Contratual** | SLA, minímos, multa por rescisão | Adobe, GA4 360, PostHog Cloud Enterprise |

### 3.2 Escala de lock-in

```
Baixo ←─────────────────────────────────────────────────────→ Alto
  │                                                              │
Matomo OP  Plausible CE  Umami  PostHog OSS  Cloudflare  Clarity  GA4  Adobe
```

### 3.3 Critérios objetivos para "baixo lock-in"

Um produto tem lock-in objetivamente baixo quando:

1. Licença **copyleft ou permissiva** (GPL, AGPL, MIT, Apache).
2. Formato de armazenamento **auditável** (banco SQL padrão ou schema publicado).
3. Existência de **fork ativo** ou possibilidade prática de fork.
4. **Ausência** de dependência de sub-serviços proprietários do fornecedor.
5. **Exportação completa** do dado bruto em formato aberto.

**Matomo On-Premise** cumpre os 5 critérios. **Plausible CE** cumpre 4 (a exportação é agregada por API — o dado bruto está no ClickHouse do próprio deployment, portanto o item 5 é cumprido *de facto* mesmo sem exportação nominal).

---

## 4. Controle dos dados

### 4.1 Definição

*Quem tem acesso administrativo ao dado neste exato momento.*

| Modalidade | Quem controla | Nível |
|-----------|---------------|:-----:|
| On-Premise em infra estatal | Estado exclusivamente | 🟢 |
| Cloud UE de fornecedor europeu (Matomo, Plausible, Simple) | Fornecedor tem acesso técnico + Estado tem acesso contratual | 🟠 |
| Cloud EU de fornecedor americano (PostHog, Cloudflare) | Fornecedor americano tem acesso, sob CLOUD Act | 🟠→🔴 |
| SaaS estrangeiro sem residência garantida (Clarity, Cloudflare) | Fornecedor exclusivamente | 🔴 |

### 4.2 Regra prática

Controle de dado é **binário e transitivo**: se o fornecedor tem acesso, o Estado **não tem controle exclusivo**. DPAs e cláusulas contratuais mitigam risco, mas não substituem custódia técnica.

---

## 5. Portabilidade

### 5.1 Tipos de portabilidade

| Tipo | Descrição |
|------|-----------|
| Dado bruto | Exportar cada evento individual |
| Agregados históricos | Exportar séries diárias/mensais |
| Configuração | Exportar sites, metas, funis, segmentos |
| Dashboards | Recriar visualizações em outra plataforma |

### 5.2 Matriz de portabilidade

| Plataforma | Bruto | Agregados | Configuração | Dashboards |
|-----------|:-----:|:---------:|:------------:|:----------:|
| Matomo | 🟢 (SQL direto) | 🟢 API | 🟢 API | 🟠 Recriar |
| Plausible CE | 🟠 (SQL ClickHouse) | 🟢 API | 🟠 Manual | 🟠 Recriar |
| Umami | 🟠 (SQL) | 🟢 API | 🟠 Manual | 🟠 Recriar |
| PostHog | 🟢 (warehouse sink) | 🟢 API | 🟠 Parcial | 🟠 Recriar |
| GA4 | 🟠 (BigQuery Export) | 🟢 API | 🔴 | 🔴 |
| Adobe | 🟢 (Data Feeds) | 🟢 | 🔴 | 🔴 |
| Simple Analytics | 🟠 (agregado) | 🟢 API | 🟠 Manual | 🔴 |
| Microsoft Clarity | 🔴 | 🟠 (limitado) | 🔴 | 🔴 |
| Cloudflare | 🔴 | 🟢 GraphQL | 🔴 | 🔴 |
| Open Web Analytics | 🟢 (SQL) | 🟠 | 🟠 | 🔴 |

### 5.3 Regra prática

**Nenhuma migração é indolor.** Portabilidade é uma escala: quanto mais estruturado o dado exportado (formato aberto, schema documentado), mais viável a saída.

---

## 6. Transparência

### 6.1 Definição

Capacidade de **auditar o comportamento do produto** — o que ele coleta, como processa, para onde envia.

| Nível | Descrição | Plataformas |
|-------|-----------|-------------|
| Total | Código-fonte inteiro público e auditável | Matomo, Plausible CE, Umami, OWA |
| Parcial | Núcleo público, complementos fechados | PostHog (OSS + EE), Clarity (só clarity-js MIT) |
| Nula | Serviço totalmente proprietário | GA4, Adobe, Cloudflare Web Analytics, Simple Analytics |

### 6.2 Por que importa para o Estado

Sem transparência, o controlador não consegue **provar em auditoria** o que exatamente o produto fez com o dado. Em cenário de incidente ou investigação da ANPD, isso pode tornar a defesa impraticável.

---

## 7. Auditabilidade

### 7.1 Dois níveis

| Nível | Do que se trata |
|-------|-----------------|
| **Código** | Consigo auditar o que o software faz internamente? |
| **Operação** | Consigo provar depois quem fez o quê no dashboard/API? |

### 7.2 Matriz

| Plataforma | Auditoria de código | Trilha de acesso administrativo |
|-----------|:-------------------:|:-------------------------------:|
| Matomo | 🟢 (100% GPL) | 🟢 Plugin `ActivityLog` (comunidade) |
| Plausible CE | 🟢 (100% AGPL) | 🟠 Básico |
| Umami | 🟢 (100% MIT) | 🟠 Básico |
| PostHog | 🟠 (core MIT + EE proprietária) | 🟢 (EE) |
| GA4 | 🔴 | 🟢 (Google Workspace audit logs) |
| Adobe | 🔴 | 🟢 |
| Simple Analytics | 🔴 | 🟠 |
| Microsoft Clarity | 🔴 (só biblioteca) | 🟠 |
| Cloudflare | 🔴 | 🟢 (Enterprise SIEM export) |
| Open Web Analytics | 🟢 (GPL) | 🔴 Rudimentar |

---

## 8. Soberania de dados

### 8.1 Definição

Sob **qual sistema jurídico** o dado está sujeito a requisição legal? Independente da residência técnica, a jurisdição do **operador** é relevante.

| Plataforma | Residência técnica | Jurisdição do operador |
|-----------|:------------------:|:----------------------:|
| Matomo On-Premise | Brasil | Brasil |
| Matomo Cloud | Alemanha | Nova Zelândia (InnoCraft — mantenedor comercial do Matomo) |
| Plausible CE | Brasil | Brasil |
| Plausible Cloud | Alemanha | Estônia (UE) |
| Umami OP | Brasil | Brasil |
| Umami Cloud | EUA | EUA |
| PostHog OP | Brasil | Brasil |
| PostHog Cloud EU | Alemanha | EUA (empresa) |
| GA4 | Múltiplas | EUA |
| Adobe | Múltiplas | EUA |
| Simple Analytics | Amsterdam | Países Baixos |
| Microsoft Clarity | Global | EUA |
| Cloudflare | Global | EUA |
| Open Web Analytics | Brasil | Brasil |

### 8.2 Regime aplicável por jurisdição

| Jurisdição | Regime primário | Impacto |
|-----------|----------------|---------|
| Brasil | LGPD + Marco Civil | Controle pleno pelo Estado brasileiro |
| UE | GDPR | Regime rígido; mecanismos de transferência exigidos |
| EUA | CLOUD Act + FISA 702 | Requisição extraterritorial possível |

> **🚨 Alerta — jurisdição não é residência**
> Cloud EU de empresa americana **não** neutraliza o CLOUD Act. Somente empresa constituída em jurisdição *sem* extraterritorialidade equivalente (por exemplo, empresa europeia ou hospedagem em infraestrutura estatal brasileira) elimina esse vetor.

---

## 9. Alinhamento com princípios de Governo Digital

### 9.1 Princípios aplicáveis (Lei 14.129/2021 + EGD)

| Princípio | Aplicação em analytics |
|-----------|-----------------------|
| Interoperabilidade | APIs abertas; formatos padrão |
| Transparência ativa | Publicação de painel público de audiência |
| Proteção de dado pessoal | LGPD-first |
| Compartilhamento de dados | Dado bruto acessível ao ecossistema de BI do Estado |
| Custo justificável | Estimativa fundamentada de TCO |
| Otimização de infraestrutura | Reuso do parque Kubernetes SETDIG |
| Segurança | Auditabilidade, criptografia, controle de acesso |
| Uso preferencial de software público / open-source | Diretriz federal (IN SGD/ME nº 1/2019) |

### 9.2 Matriz de alinhamento

| Plataforma | Interop | Transp. | LGPD | Compart. | Custo | Infra | Segur. | OSS pref. |
|-----------|:-------:|:-------:|:----:|:--------:|:-----:|:-----:|:------:|:---------:|
| Matomo OP | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 |
| Plausible CE | 🟢 | 🟢 | 🟢 | 🟠 | 🟢 | 🟢 | 🟢 | 🟢 |
| Umami | 🟢 | 🟢 | 🟢 | 🟠 | 🟢 | 🟢 | 🟠 | 🟢 |
| PostHog OSS | 🟢 | 🟠 | 🟢 | 🟢 | 🔴 | 🔴 | 🟢 | 🟠 |
| GA4 | 🟠 | 🔴 | 🔴 | 🟠 | 🟠 | 🟢 | 🟢 | 🔴 |
| Adobe | 🟠 | 🔴 | 🟠 | 🟠 | 🔴 | 🟢 | 🟢 | 🔴 |
| Clarity | 🔴 | 🔴 | 🔴 | 🔴 | 🟢 | 🟢 | 🟠 | 🔴 |
| Cloudflare | 🟠 | 🔴 | 🟠 | 🔴 | 🟢 | 🟢 | 🟢 | 🔴 |

---

## 10. Recomendação

> **✅ Bloco de Decisão — perfil de governança adequado ao Estado**
>
> Priorizar plataformas que combinem simultaneamente:
>
> 1. **Licença copyleft** (GPL/AGPL) — reduz risco de fechamento posterior.
> 2. **Auto-hospedagem em infra estatal** — maximiza controle e soberania.
> 3. **Formato de dado auditável** (SQL padrão ou schema publicado).
> 4. **Trilha de auditoria** exportável para SIEM corporativo.
> 5. **Alinhamento com Lei 14.129/2021 e IN SGD/ME 1/2019** (preferência por software público / open-source).
>
> **Cumprimento simultâneo dos 5:** apenas **Matomo On-Premise** (GPL v3) e **Plausible Community Edition** (AGPL v3).
>
> **Umami** cumpre 4 dos 5 (perde em item 1 pela licença MIT permissiva — risco de fechamento futuro).
>
> Plataformas com **falha estrutural** na dimensão governança (não recomendadas como primárias): **GA4, Adobe Analytics, Microsoft Clarity, Cloudflare Web Analytics, Simple Analytics**.

Fundamentos legais: [`comparativos/lgpd.md`](lgpd.md).
Decisão registrada: [`docs/08-adr.md`](../docs/08-adr.md).
Riscos residuais: [`docs/11-riscos.md`](../docs/11-riscos.md).
