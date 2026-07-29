# 07 — Matriz de Decisão Ponderada

> **Método:** MAUT — Multi-Attribute Utility Theory
> **Anterior:** [06 — Trade-offs](06-tradeoffs.md) · **Próximo:** [08 — ADR](08-adr.md)

---

## Sumário

- [1. Parâmetros da matriz](#1-parâmetros-da-matriz)
- [2. Matriz consolidada](#2-matriz-consolidada)
- [3. Resultado e ranking](#3-resultado-e-ranking)
- [4. Justificativa nota a nota](#4-justificativa-nota-a-nota)
- [5. Análise de sensibilidade](#5-análise-de-sensibilidade)
- [6. Análise de dominância](#6-análise-de-dominância)
- [7. Conclusão da matriz](#7-conclusão-da-matriz)

---

## 1. Parâmetros da matriz

| Parâmetro | Valor |
|-----------|-------|
| Critérios | 12 |
| Soma dos pesos | 120 |
| Escala de notas | 1 a 5 (inteiros) |
| Pontuação máxima | 600 |
| Fórmula | $S_p = \sum_{i=1}^{12} w_i \cdot n_{p,i}$ |
| Plataformas avaliadas | 13 (10 obrigatórias + Matomo Cloud + PostHog Cloud + Piwik PRO) |
| Definição operacional das notas | [`03-criterios-de-avaliacao.md`, seção 5](03-criterios-de-avaliacao.md#5-definição-operacional-de-cada-critério) |

### 1.1 Pesos aplicados

| Código | Critério | Peso |
|:------:|----------|-----:|
| C01 | LGPD | 15 |
| C02 | Controle dos dados | 15 |
| C03 | TCO | 15 |
| C04 | Independência tecnológica | 10 |
| C05 | Recursos analíticos | 10 |
| C06 | APIs | 10 |
| C07 | Integrações | 10 |
| C08 | Escalabilidade | 10 |
| C09 | Segurança | 10 |
| C10 | Operação | 5 |
| C11 | Comunidade | 5 |
| C12 | Documentação | 5 |
| | **Total** | **120** |

---

## 2. Matriz consolidada

### 2.1 Notas atribuídas

| Plataforma | C01<br/>LGPD<br/>`15` | C02<br/>Controle<br/>`15` | C03<br/>TCO<br/>`15` | C04<br/>Independ.<br/>`10` | C05<br/>Recursos<br/>`10` | C06<br/>APIs<br/>`10` | C07<br/>Integr.<br/>`10` | C08<br/>Escala<br/>`10` | C09<br/>Segur.<br/>`10` | C10<br/>Oper.<br/>`5` | C11<br/>Comun.<br/>`5` | C12<br/>Docum.<br/>`5` |
|-----------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Matomo On-Premise** | **5** | **5** | 4 | **5** | 4 | 4 | 4 | 3 | 4 | 3 | 4 | 4 |
| **Plausible CE** | **5** | **5** | 4 | 4 | 3 | 3 | 3 | 4 | 4 | 3 | 3 | 4 |
| **Matomo Cloud** | 4 | 3 | 3 | 4 | **5** | 4 | 4 | 4 | 4 | **5** | 4 | 4 |
| **Piwik PRO** | **5** | 4 | 2 | 2 | **5** | 4 | 4 | **5** | **5** | 4 | 2 | 4 |
| **Umami** | **5** | **5** | **5** | 4 | 2 | 3 | 2 | 3 | 3 | 4 | 3 | 3 |
| **PostHog auto-hospedado** | 4 | **5** | 2 | 3 | **5** | 4 | 4 | 3 | 4 | 1 | 4 | 4 |
| **PostHog Cloud EU** | 3 | 2 | 3 | 2 | **5** | 4 | 4 | **5** | 4 | **5** | 4 | 4 |
| **Google Analytics 4** | 2 | 1 | 4 | 1 | 4 | 4 | **5** | **5** | 4 | **5** | **5** | **5** |
| **Adobe Analytics** | 3 | 2 | 1 | 1 | **5** | **5** | **5** | **5** | **5** | 4 | 3 | 4 |
| **Microsoft Clarity** | 2 | 1 | **5** | 1 | 3 | 2 | 3 | **5** | 4 | **5** | 3 | 3 |
| **Simple Analytics** | 4 | 2 | 3 | 2 | 2 | 3 | 2 | 4 | 4 | **5** | 2 | 3 |
| **Cloudflare Web Analytics** | 3 | 1 | **5** | 1 | 1 | 3 | 2 | **5** | 4 | **5** | 2 | 3 |
| **Open Web Analytics** | 4 | **5** | 3 | 3 | 2 | 2 | 1 | 1 | **1** | 2 | 1 | 1 |

### 2.2 Pontuação ponderada (peso × nota)

| Plataforma | C01 | C02 | C03 | C04 | C05 | C06 | C07 | C08 | C09 | C10 | C11 | C12 | **Total** |
|-----------|----:|----:|----:|----:|----:|----:|----:|----:|----:|----:|----:|----:|----------:|
| **Matomo On-Premise** | 75 | 75 | 60 | 50 | 40 | 40 | 40 | 30 | 40 | 15 | 20 | 20 | **505** |
| **Plausible CE** | 75 | 75 | 60 | 40 | 30 | 30 | 30 | 40 | 40 | 15 | 15 | 20 | **470** |
| **Matomo Cloud** | 60 | 45 | 45 | 40 | 50 | 40 | 40 | 40 | 40 | 25 | 20 | 20 | **465** |
| **Piwik PRO** | 75 | 60 | 30 | 20 | 50 | 40 | 40 | 50 | 50 | 20 | 10 | 20 | **465** |
| **Umami** | 75 | 75 | 75 | 40 | 20 | 30 | 20 | 30 | 30 | 20 | 15 | 15 | **445** |
| **PostHog auto-hospedado** | 60 | 75 | 30 | 30 | 50 | 40 | 40 | 30 | 40 | 5 | 20 | 20 | **440** |
| **PostHog Cloud EU** | 45 | 30 | 45 | 20 | 50 | 40 | 40 | 50 | 40 | 25 | 20 | 20 | **425** |
| **Google Analytics 4** | 30 | 15 | 60 | 10 | 40 | 40 | 50 | 50 | 40 | 25 | 25 | 25 | **410** |
| **Adobe Analytics** | 45 | 30 | 15 | 10 | 50 | 50 | 50 | 50 | 50 | 20 | 15 | 20 | **405** |
| **Microsoft Clarity** | 30 | 15 | 75 | 10 | 30 | 20 | 30 | 50 | 40 | 25 | 15 | 15 | **355** |
| **Simple Analytics** | 60 | 30 | 45 | 20 | 20 | 30 | 20 | 40 | 40 | 25 | 10 | 15 | **355** |
| **Cloudflare Web Analytics** | 45 | 15 | 75 | 10 | 10 | 30 | 20 | 50 | 40 | 25 | 10 | 15 | **345** |
| **Open Web Analytics** | 60 | 75 | 45 | 30 | 20 | 20 | 10 | 10 | 10 | 10 | 5 | 5 | **300** |

---

## 3. Resultado e ranking

### 3.1 Ranking final

| # | Plataforma | Pontuação | % | Faixa | Situação na triagem |
|--:|-----------|----------:|--:|-------|--------------------|
| 🥇 **1** | **Matomo On-Premise** | **505** | **84,2 %** | 🟢 **Recomendada** | Aprovada |
| 🥈 2 | Plausible CE | 470 | 78,3 % | 🟡 Viável | Aprovada |
| 🥉 3 | Matomo Cloud | 465 | 77,5 % | 🟡 Viável | Aprovada com ressalva |
| 🥉 3 | Piwik PRO | 465 | 77,5 % | 🟡 Viável | Aprovada |
| 5 | Umami | 445 | 74,2 % | 🟡 Viável | Aprovada |
| 6 | PostHog auto-hospedado | 440 | 73,3 % | 🟡 Viável | Aprovada |
| 7 | PostHog Cloud EU | 425 | 70,8 % | 🟡 Viável | Aprovada com ressalva |
| 8 | Google Analytics 4 | 410 | 68,3 % | 🟠 Condicionada | Aprovada com ressalva grave |
| 9 | Adobe Analytics | 405 | 67,5 % | 🟠 Condicionada | Aprovada com ressalva |
| 10 | Microsoft Clarity | 355 | 59,2 % | 🔴 Não recomendada | **Eliminada como primária** |
| 10 | Simple Analytics | 355 | 59,2 % | 🔴 Não recomendada | Aprovada com ressalva |
| 12 | Cloudflare Web Analytics | 345 | 57,5 % | 🔴 Não recomendada | **Eliminada como primária** |
| 13 | Open Web Analytics | 300 | 50,0 % | 🔴 Não recomendada | **Eliminada (E6)** |

### 3.2 Desempate — 3º lugar

Matomo Cloud e Piwik PRO empatam em 465 pontos. Aplicando a regra de desempate de [`03-criterios-de-avaliacao.md`, seção 6.4](03-criterios-de-avaliacao.md#64-regra-de-desempate):

| Critério de desempate | Matomo Cloud | Piwik PRO | Vencedor |
|----------------------|:------------:|:---------:|----------|
| 1º — Nota em C01 (LGPD) | 4 | **5** | **Piwik PRO** |

**Resultado:** Piwik PRO ocupa o 3º lugar; Matomo Cloud, o 4º.

### 3.3 Visualização do ranking

```mermaid
xychart-beta
    title "Pontuação ponderada (máximo 600)"
    x-axis ["Matomo OP", "Plausible", "Piwik PRO", "Matomo Cloud", "Umami", "PostHog SH", "PostHog Cloud", "GA4", "Adobe", "Clarity", "Simple", "Cloudflare", "OWA"]
    y-axis "Pontos" 0 --> 600
    bar [505, 470, 465, 465, 445, 440, 425, 410, 405, 355, 355, 345, 300]
```

### 3.4 Perfil das cinco primeiras colocadas

```mermaid
radar-beta
  axis lgpd["LGPD"], ctrl["Controle"], tco["TCO"], indep["Independência"], rec["Recursos"], api["APIs"], integ["Integrações"], esc["Escalabilidade"]
  curve matomo["Matomo On-Premise"]{5, 5, 4, 5, 4, 4, 4, 3}
  curve plausible["Plausible CE"]{5, 5, 4, 4, 3, 3, 3, 4}
  curve piwik["Piwik PRO"]{5, 4, 2, 2, 5, 4, 4, 5}
  curve ga4["Google Analytics 4"]{2, 1, 4, 1, 4, 4, 5, 5}
  max 5
  min 0
```

> **📌 Observação**
> O gráfico de radar evidencia a característica central do Matomo On-Premise: **a área coberta é a mais uniforme**. Piwik PRO e GA4 apresentam picos altos em alguns eixos e vales profundos em outros. Em decisão multicritério com pesos concentrados nos eixos de conformidade e governança, a uniformidade vence a especialização.

---

## 4. Justificativa nota a nota

Cada nota abaixo remete à definição operacional em [`03-criterios-de-avaliacao.md`, seção 5](03-criterios-de-avaliacao.md#5-definição-operacional-de-cada-critério).

---

### 4.1 Matomo On-Premise — 505 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **5** | Configurável para operar sem cookie de identificação e com anonimização de IP em até 4 bytes; nenhuma transferência internacional; retenção definida integralmente pelo Estado com exclusão física verificável; opt-out nativo por iframe e API; respeita DoNotTrack. **Precedente da CNIL** reconhece configuração específica do Matomo como isenta de consentimento — única plataforma da matriz com precedente favorável de autoridade de proteção de dados. Atende integralmente RL-01 a RL-14 |
| **C02 — Controle** | **5** | Dado bruto em MySQL/MariaDB sob custódia física do Estado; acesso SQL direto; exportação e eliminação totais e verificáveis por consulta; nenhum terceiro tem acesso |
| **C03 — TCO** | **4** | TCO estimado de ~R$ 660 k em 5 anos (faixa R$ 250 k – R$ 600 k seria nota 5; R$ 600 k – R$ 1,2 M é nota 4). Composição: licença zero, ~R$ 60 k em plugins premium, ~R$ 220 k de infraestrutura, ~R$ 300 k de equipe. **Não recebe 5** porque o custo de operação é real e recorrente — software livre não é gratuito em TCO |
| **C04 — Independência** | **5** | GPL v3; código auditável; sem dependência de serviço do fornecedor para operar; schema de banco inspecionável; múltiplas opções de hospedagem; fork tecnicamente viável; direito perpétuo sobre a versão obtida |
| **C05 — Recursos** | **4** | Cobertura ponderada dos RF entre 75 % e 89 %. Cobre page views, eventos, dimensões customizadas, metas, funis, segmentação, jornada, coortes, tempo real, e-commerce, heatmaps, session recording, form analytics e A/B testing. **Não recebe 5** porque funil, heatmap, session recording, form analytics e A/B testing são **plugins pagos** na modalidade On-Premise, e a análise de retenção é menos madura que a de GA4/PostHog |
| **C06 — APIs** | **4** | Reporting API HTTP completa com saída em JSON, XML, CSV, TSV, HTML e RSS; Tracking HTTP API; API de administração; SDK oficial em PHP; SDKs móveis oficiais; acesso SQL direto ao dado bruto. **Não recebe 5** por: ausência de GraphQL, ausência de webhooks nativos, versionamento implícito e SDKs Python/Node.js apenas comunitários |
| **C07 — Integrações** | **4** | Tag Manager nativo; integração com GTM; plugins de SAML e OIDC (pagos); plugins oficiais para WordPress e Drupal; **acesso SQL direto habilita Metabase, Superset, Power BI e Grafana sem intermediação de API** — vantagem estrutural sobre qualquer SaaS. **Não recebe 5** por ausência de conectores nativos certificados e por SSO exigir plugin pago |
| **C08 — Escalabilidade** | **3** | Atende o cenário de referência (12 M page views/mês) com tuning ativo. Escala horizontal da coleta é viável; escala do processamento é parcial. **Gargalo estrutural de arquivamento** e limitação do MySQL para escrita impedem nota superior. Mitigável por QueuedTracking + Redis, réplicas de leitura e particionamento — porém exige trabalho de engenharia, não vem pronto |
| **C09 — Segurança** | **4** | Gestão de vulnerabilidades ativa e responsiva; MFA por TOTP nativo; RBAC granular por site e por permissão; log de auditoria nativo; programa de divulgação de vulnerabilidades. **Não recebe 5** por ausência de certificação de terceiro (ISO 27001/SOC 2) — inaplicável ao modelo auto-hospedado, já que a responsabilidade recai sobre a infraestrutura do Estado |
| **C10 — Operação** | **3** | Auto-hospedado de complexidade moderada: 2 componentes obrigatórios (PHP + MySQL) mais Redis recomendado, cron de arquivamento e tuning periódico. Estimativa de ~0,3 FTE em regime permanente |
| **C11 — Comunidade** | **4** | Comunidade grande e ativa; mais de 15 anos de projeto; releases regulares; marketplace extenso de plugins; **adoção institucional documentada** (Comissão Europeia — Europa Analytics). **Não recebe 5** por concentração da governança na InnoCraft e escassez de profissionais no mercado brasileiro |
| **C12 — Documentação** | **4** | Documentação oficial extensa: guias de usuário, de desenvolvedor, referência de API, guias de implantação e de otimização de desempenho; FAQ abrangente. **Não recebe 5** por cobertura irregular de tópicos avançados de operação em escala e por tradução parcial para pt-BR |

---

### 4.2 Plausible Community Edition — 470 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **5** | Não coleta dado pessoal por design: sem cookies, sem armazenamento de IP, sem identificador persistente. A base legal é trivial — se não há dado pessoal, a LGPD não incide (art. 12). Dispensa CMP, dispensa consentimento, dispensa RIPD específico |
| **C02 — Controle** | **5** | Na modalidade Community Edition, dado bruto em ClickHouse sob custódia do Estado, com acesso SQL direto |
| **C03 — TCO** | **4** | ~R$ 450 k em 5 anos: licença zero, ~R$ 180 k de infraestrutura (dois bancos), ~R$ 200 k de equipe, ~R$ 50 k de implantação. **Não recebe 5** pelo custo de operar simultaneamente PostgreSQL e ClickHouse |
| **C04 — Independência** | **4** | AGPL v3 — copyleft de rede, proteção mais forte que GPL. **Não recebe 5** porque a governança é concentrada em um único mantenedor comercial e há funcionalidades reservadas aos planos comerciais da nuvem, criando divergência entre CE e produto pago |
| **C05 — Recursos** | **3** | Cobertura ponderada entre 55 % e 74 %. Cobre page views, eventos, metas, funis, campanhas e tempo real. **Não cobre**: heatmaps, session recording, coortes, retenção, atribuição multicanal, segmentos compostos, jornada de usuário, dimensões customizadas sem plano pago |
| **C06 — APIs** | **3** | Stats API REST funcional e bem documentada, com autenticação Bearer e versionamento explícito; Events API para ingestão. **Não recebe mais** por ausência de SDK oficial, ausência de webhooks e cobertura limitada de administração |
| **C07 — Integrações** | **3** | Integração viável via API; ClickHouse conecta nativamente a Grafana, Superset e Metabase; plugin para WordPress; integração com Slack. **Não recebe mais** por ausência de Tag Manager, ausência de SSO (SAML/OIDC) e ausência de conector de Power BI |
| **C08 — Escalabilidade** | **4** | ClickHouse escala para centenas de milhões de eventos com margem confortável sobre o cenário de pico; sem processo de arquivamento. **Não recebe 5** por ingestão síncrona sem fila, o que limita a absorção de picos instantâneos |
| **C09 — Segurança** | **4** | Código aberto auditável; superfície de ataque reduzida pelo escopo mínimo; gestão de vulnerabilidades ativa. **Penalizado** pela ausência de MFA nativo e por RBAC básico — compensável por proxy autenticador ou VPN, mas é lacuna real |
| **C10 — Operação** | **3** | Três componentes (aplicação Elixir + PostgreSQL + ClickHouse); container oficial disponível; sem cron crítico. Competência em ClickHouse é escassa no mercado |
| **C11 — Comunidade** | **3** | Comunidade moderada e crescente; releases regulares; repositório ativo. Base instalada relevante, porém concentrada em pequenas organizações; adoção institucional pública ainda incipiente |
| **C12 — Documentação** | **4** | Documentação clara, bem organizada e atualizada, incluindo guias de self-hosting e referência de API. **Não recebe 5** por cobertura limitada de operação em escala e ausência de tradução |

---

### 4.3 Piwik PRO — 465 pontos (3º)

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **5** | Conformidade é a proposta de valor central do produto: Consent Manager nativo integrado, controle granular de retenção, anonimização configurável, região de dados selecionável (incluindo On-Premises), DPA formal disponível, aderência declarada a RGPD e HIPAA |
| **C02 — Controle** | **4** | Private Cloud com isolamento dedicado ou On-Premises. **Não recebe 5** porque, mesmo em On-Premises, o acesso ao armazenamento é mediado pelo produto proprietário, sem schema documentado para consulta direta |
| **C03 — TCO** | **2** | Estimado em ~R$ 1,32 M em 5 anos (faixa R$ 1,2 M – R$ 3 M = nota 2), predominantemente licença. **Estimativa de mercado — preço não é público**, exige cotação formal |
| **C04 — Independência** | **2** | Proprietária, com acoplamento relevante ao ecossistema do fornecedor (Tag Manager, Consent Manager e CDP integrados). Exportação existe, mas o schema não é documentado. Sem capacidade de fork ou de operação sem o fornecedor |
| **C05 — Recursos** | **5** | Cobertura ponderada ≥ 90 %: web analytics completo, funis, coortes, retenção, atribuição, dimensões customizadas, Tag Manager, Consent Manager e CDP em produto único |
| **C06 — APIs** | **4** | API REST completa de leitura e administração, autenticação OAuth 2.0, versionamento explícito. **Não recebe 5** por ausência de acesso ao dado bruto sem contratação adicional e ausência de SDKs oficiais amplos |
| **C07 — Integrações** | **4** | SSO com SAML e OIDC nativo; LDAP/AD; Tag Manager e Consent Manager próprios; integração com GTM. **Não recebe 5** por ausência de conectores nativos certificados para as principais ferramentas de BI |
| **C08 — Escalabilidade** | **5** | Arquitetura colunar gerenciada, dimensionada pelo fornecedor; escalabilidade contratual sem esforço do Estado |
| **C09 — Segurança** | **5** | Certificações de terceiro (ISO 27001); SDLC seguro documentado; MFA e RBAC granulares; auditoria completa; SLA de segurança contratual |
| **C10 — Operação** | **4** | Private Cloud tem operação leve (administração apenas); On-Premises tem complexidade moderada com suporte do fornecedor |
| **C11 — Comunidade** | **2** | Comunidade praticamente inexistente — é produto comercial fechado. Ecossistema restrito à rede de parceiros do fornecedor; sem presença consolidada no Brasil |
| **C12 — Documentação** | **4** | Documentação oficial boa, com foco em conformidade e implantação. **Não recebe 5** por ausência de tradução e por não haver material comunitário |

---

### 4.4 Matomo Cloud — 465 pontos (4º)

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **4** | Mesmas capacidades técnicas de conformidade do On-Premise, com DPA formal disponível e região de dados selecionável. **Rebaixado para 4** pela existência de transferência internacional, que exige base legal do art. 33 e avaliação documentada |
| **C02 — Controle** | **3** | SaaS multi-tenant com exportação integral garantida via API. **Não recebe mais** pela ausência de acesso SQL direto e pela custódia física estar com o fornecedor |
| **C03 — TCO** | **3** | ~R$ 635 k em 5 anos, predominantemente assinatura em moeda estrangeira. Recebe nota inferior ao On-Premise apesar de valor absoluto similar porque **o custo é integralmente recorrente e exposto a câmbio e a variação de volume** — em um pico de 30×, a fatura sobe |
| **C04 — Independência** | **4** | O software subjacente é GPL v3, e a migração para On-Premise é viável e documentada — o que preserva substancialmente a independência. **Não recebe 5** pela dependência operacional do fornecedor |
| **C05 — Recursos** | **5** | Cobertura ≥ 90 % — todos os recursos premium (funis, heatmaps, session recording, form analytics, A/B testing, media analytics, roll-up, custom reports) estão incluídos nos planos, sem compra adicional |
| **C06 — APIs** | **4** | Idêntica ao On-Premise, menos o acesso SQL direto |
| **C07 — Integrações** | **4** | Idêntica ao On-Premise; recursos premium incluídos compensam a ausência de acesso ao banco |
| **C08 — Escalabilidade** | **4** | Gerenciada pelo fornecedor, dimensionada para o cenário de pico. **Não recebe 5** porque a escalabilidade é contratual — o pico gera custo, não falha |
| **C09 — Segurança** | **4** | Postura gerenciada pelo fornecedor, com práticas documentadas. **Não recebe 5** por certificações menos abrangentes que as de fornecedores enterprise |
| **C10 — Operação** | **5** | SaaS gerenciado — esforço operacional do Estado próximo de zero |
| **C11 — Comunidade** | **4** | A mesma do Matomo |
| **C12 — Documentação** | **4** | A mesma do Matomo, acrescida de documentação específica da nuvem |

---

### 4.5 Umami — 445 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **5** | Sem cookies; IP não armazenado em claro (hash com sal rotativo); sem identificador persistente; conformidade por design |
| **C02 — Controle** | **5** | Auto-hospedado com acesso SQL direto a PostgreSQL ou MySQL |
| **C03 — TCO** | **5** | ~R$ 320 k em 5 anos — o menor entre as opções soberanas viáveis. Stack de 2 componentes, footprint de infraestrutura reduzido, operação simples |
| **C04 — Independência** | **4** | MIT — máxima permissividade de uso, direito perpétuo sobre a versão obtida. **Não recebe 5** porque a licença permissiva permite fechamento de versões futuras, e a governança é concentrada em um mantenedor comercial |
| **C05 — Recursos** | **2** | Cobertura ponderada entre 35 % e 54 %. Cobre page views, eventos básicos, campanhas, geolocalização e tempo real. **Não cobre**: funis, coortes, retenção, heatmaps, session recording, jornada, atribuição, segmentação avançada, busca interna, e-commerce, Tag Manager |
| **C06 — APIs** | **3** | API REST funcional para leitura e ingestão, com autenticação Bearer; acesso SQL direto. **Não recebe mais** por documentação limitada, ausência de versionamento explícito e ausência de webhooks |
| **C07 — Integrações** | **2** | Integração exige desenvolvimento; sem Tag Manager, sem SSO, sem conectores de BI. O acesso SQL direto é a única via prática |
| **C08 — Escalabilidade** | **3** | Atende o cenário de referência com banco relacional; suporta ClickHouse na modalidade nuvem, mas essa configuração é menos documentada no auto-hospedado. Sem fila de ingestão |
| **C09 — Segurança** | **3** | Base de código pequena e auditável; gestão de vulnerabilidades presente porém com cadência irregular. **Penalizado** por ausência de MFA, RBAC básico e ausência de log de auditoria |
| **C10 — Operação** | **4** | Auto-hospedado de operação simples: dois componentes, container oficial, sem cron crítico, atualização trivial |
| **C11 — Comunidade** | **3** | Comunidade moderada; releases regulares; base instalada relevante mas concentrada em projetos pequenos |
| **C12 — Documentação** | **3** | Documentação suficiente para instalar e operar, porém incompleta em tópicos avançados (escala, tuning, API detalhada) |

---

### 4.6 PostHog auto-hospedado — 440 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **4** | Auto-hospedado elimina transferência internacional; anonimização e retenção configuráveis; controle total. **Não recebe 5** porque o produto é orientado a identificação de usuário e a session replay — a configuração conforme exige trabalho ativo, e o padrão de fábrica não é privacy-first |
| **C02 — Controle** | **5** | Dado bruto em ClickHouse sob custódia do Estado, com HogQL e SQL direto |
| **C03 — TCO** | **2** | ~R$ 1,68 M em 5 anos — infraestrutura de 6 componentes (~R$ 600 k) e operação (~R$ 900 k, aproximadamente 0,8 FTE de engenheiro de plataforma). Faixa R$ 1,2 M – R$ 3 M = nota 2 |
| **C04 — Independência** | **3** | Core em MIT, mas: (a) a edição EE é proprietária; (b) **o suporte ao auto-hospedado foi descontinuado pelo fornecedor**; (c) divergência crescente entre a versão OSS e a nuvem. O direito de uso permanece, mas a viabilidade prática de acompanhar a evolução é limitada |
| **C05 — Recursos** | **5** | Cobertura ≥ 90 %: product analytics, funis, coortes, retenção, jornada, session replay, feature flags, experimentos, surveys, HogQL e data warehouse |
| **C06 — APIs** | **4** | API REST completa de leitura, escrita e administração; webhooks nativos; SDKs oficiais em múltiplas linguagens; HogQL para consulta SQL. **Não recebe 5** por ausência de GraphQL e por rate limits relevantes na modalidade nuvem |
| **C07 — Integrações** | **4** | Ampla biblioteca de integrações e destinos; SSO disponível; ClickHouse conecta a Grafana, Superset e Metabase. **Não recebe 5** porque SAML é recurso da edição paga e não há conector nativo de Power BI |
| **C08 — Escalabilidade** | **3** | O ClickHouse escala, mas a configuração `docker-compose` **não é suportada para volumes elevados**, e não há Helm chart oficial mantido. A capacidade técnica existe; o caminho suportado, não |
| **C09 — Segurança** | **4** | Código aberto; gestão de vulnerabilidades ativa; SOC 2 na nuvem; RBAC e auditoria presentes. **Penalizado** pela superfície de ataque ampliada (6 componentes) na modalidade auto-hospedado |
| **C10 — Operação** | **1** | **Nota mínima.** Operar Django + PostgreSQL + ClickHouse + Kafka + Redis + object storage, com backup coordenado entre múltiplos armazenamentos e **sem suporte do fornecedor**, exige equipe dedicada especializada — o que é incompatível com a restrição **R5** ([01, seção 8](01-contexto.md#8-restrições)) |
| **C11 — Comunidade** | **4** | Comunidade grande e muito ativa; cadência de releases elevada; ecossistema em expansão. **Não recebe 5** porque a comunidade se concentra na modalidade nuvem, com pouco material sobre operação auto-hospedado em escala |
| **C12 — Documentação** | **4** | Documentação excelente para uso do produto e para a nuvem. **Rebaixada para 4** porque a documentação de self-hosting é explicitamente marcada como não suportada e é substancialmente menos completa |

---

### 4.7 PostHog Cloud EU — 425 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **3** | Região UE disponível e DPA formal, mas a transferência internacional persiste e a orientação do produto à identificação de usuário exige configuração ativa e RIPD |
| **C02 — Controle** | **2** | SaaS; exportação disponível, mas o dado bruto de session replay não é integralmente portável |
| **C03 — TCO** | **3** | ~R$ 900 k – R$ 1,1 M em 5 anos no modelo por evento. **Custo altamente sensível a pico de tráfego** — 30× de pico gera 30× de custo naquele mês |
| **C04 — Independência** | **2** | Dependência operacional integral do fornecedor; migração para auto-hospedado teoricamente possível, mas sem caminho suportado |
| **C05 — Recursos** | **5** | Idêntica ao auto-hospedado, acrescida de funcionalidades exclusivas da nuvem |
| **C06 — APIs** | **4** | Idêntica ao auto-hospedado, com rate limits |
| **C07 — Integrações** | **4** | Idêntica ao auto-hospedado |
| **C08 — Escalabilidade** | **5** | Gerenciada, sem limite prático |
| **C09 — Segurança** | **4** | SOC 2 Type II; práticas maduras. **Não recebe 5** por ausência de ISO 27001 consolidada |
| **C10 — Operação** | **5** | SaaS gerenciado |
| **C11 — Comunidade** | **4** | A mesma do PostHog |
| **C12 — Documentação** | **4** | Excelente para a modalidade nuvem |

---

### 4.8 Google Analytics 4 — 410 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **2** | Coleta dado pessoal por padrão; **transferência internacional para os EUA**, jurisdição sem decisão de adequação da ANPD; exige consentimento explícito e CMP; retenção máxima de 14 meses para dados de evento, definida pelo fornecedor; opt-out depende de extensão instalada pelo titular; **decisões desfavoráveis de quatro autoridades europeias de proteção de dados sob norma análoga**. Não recebe 1 porque existe caminho de conformidade (cláusulas contratuais, consent mode, RIPD), ainda que oneroso e de resultado incerto |
| **C02 — Controle** | **1** | Dado sob custódia exclusiva do Google; sem acesso ao armazenamento; exportação apenas via BigQuery, que precisa estar configurado **antes** da coleta — dado histórico anterior à configuração é irrecuperável; eliminação não verificável pelo Estado |
| **C03 — TCO** | **4** | ~R$ 380 k em 5 anos: licença zero, custo concentrado em conformidade (CMP, RIPD, assessoria jurídica), implantação e equipe. **Não recebe 5** porque o custo de saída (~R$ 200 k) e o risco de migração forçada são materialmente superiores aos das alternativas — precedente do desligamento do Universal Analytics |
| **C04 — Independência** | **1** | Proprietária, com acoplamento profundo ao ecossistema Google; formato de dados não portável entre plataformas; **precedente concretizado de descontinuidade unilateral de versão com perda de histórico**; custo de saída elevado |
| **C05 — Recursos** | **4** | Cobertura entre 75 % e 89 %: eventos, dimensões customizadas, funis, coortes, retenção, atribuição, explorações, machine learning, tempo real. **Não recebe 5** por: ausência de heatmap e session recording, **aplicação de amostragem** acima de limiar e limites de cardinalidade |
| **C06 — APIs** | **4** | Data API v1 e Admin API completas; OAuth 2.0; versionamento explícito; SDKs oficiais em múltiplas linguagens; Measurement Protocol para ingestão; exportação nativa para BigQuery. **Não recebe 5** por quotas rígidas por token e por dia, e por ausência de acesso ao dado bruto fora do BigQuery |
| **C07 — Integrações** | **5** | O ecossistema de integração mais amplo do mercado: Looker Studio nativo, BigQuery nativo, Google Ads, Search Console, GTM, conector oficial de Power BI, SSO via Google Workspace, centenas de integrações de terceiros |
| **C08 — Escalabilidade** | **5** | Escala global comprovada, sem esforço do Estado |
| **C09 — Segurança** | **4** | ISO 27001, SOC 2, infraestrutura Google; MFA; RBAC. **Não recebe 5** porque o log de auditoria é limitado e não exportável para SIEM |
| **C10 — Operação** | **5** | SaaS gerenciado |
| **C11 — Comunidade** | **5** | A maior base instalada do mercado; oferta ampla de profissionais e agências no Brasil; volume massivo de material |
| **C12 — Documentação** | **5** | Documentação extensa, atualizada, com referência completa de API, exemplos, guias de migração, certificações e **tradução para pt-BR** |

> **📌 Observação — Por que o GA4 pontua 68,3 % apesar de excelência técnica**
> O GA4 obtém notas 5 em quatro critérios e 4 em outros quatro. Sua posição no ranking decorre exclusivamente de notas 1 e 2 nos critérios de **maior peso**: C01 (LGPD, peso 15, nota 2 = 30/75) e C02 (Controle, peso 15, nota 1 = 15/75). Somente nesses dois critérios o GA4 perde **105 pontos** em relação ao Matomo On-Premise — mais que a diferença total de 95 pontos entre as duas plataformas.
>
> Isso não é artefato da ponderação: é exatamente o que a ponderação pretende capturar. Para um órgão público, soberania e conformidade não são compensáveis por conveniência de integração.

---

### 4.9 Adobe Analytics — 405 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **3** | DPA robusto, controles de privacidade maduros, região de dados negociável, retenção contratual. **Rebaixado para 3** por transferência internacional e por exigir consentimento e configuração não trivial |
| **C02 — Controle** | **2** | SaaS; exportação de dado bruto disponível via Data Feeds, porém como **módulo pago adicional**; sem acesso ao armazenamento |
| **C03 — TCO** | **1** | Estimado entre R$ 4,5 M e R$ 7 M em 5 anos (> R$ 3 M = nota 1). Inclui licença, implantação, consultoria e treinamento. **Estimativa de mercado — preço não é público** |
| **C04 — Independência** | **1** | Lock-in máximo do estudo: implementação profundamente acoplada ao ecossistema Adobe (Launch, Experience Platform, Customer Journey Analytics); custo de saída estimado em R$ 600 k+ |
| **C05 — Recursos** | **5** | A maior profundidade analítica do mercado: Analysis Workspace, atribuição algorítmica, segmentação ilimitada, Data Warehouse, Customer Journey Analytics |
| **C06 — APIs** | **5** | Analytics API 2.0 completa; OAuth Server-to-Server; versionamento explícito; SDKs oficiais; Data Feeds para dado bruto; Bulk Data Insertion API |
| **C07 — Integrações** | **5** | Suíte Adobe Experience Cloud integrada; Adobe Launch (tag management); SSO enterprise; conectores de BI; ecossistema de parceiros consolidado |
| **C08 — Escalabilidade** | **5** | Escala enterprise comprovada, sem amostragem |
| **C09 — Segurança** | **5** | ISO 27001, SOC 2 Type II, FedRAMP em determinadas ofertas; SDLC seguro; MFA e RBAC granulares; auditoria completa |
| **C10 — Operação** | **4** | SaaS gerenciado, porém a administração e a governança de implementação exigem especialista dedicado |
| **C11 — Comunidade** | **3** | Comunidade profissional relevante e certificação formal, porém restrita ao segmento enterprise; ecossistema fechado |
| **C12 — Documentação** | **4** | Documentação extensa, com referência de API e material de certificação. **Não recebe 5** por complexidade de navegação e fragmentação entre produtos da suíte |

---

### 4.10 Microsoft Clarity — 355 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **2** | Coleta interação detalhada, incluindo movimento de mouse e conteúdo de página; transferência internacional para os EUA; retenção não configurável; ausência de instrumento contratual de operador adequado para órgão público brasileiro; **risco de captura acidental de dado sensível** em portais transacionais |
| **C02 — Controle** | **1** | Custódia exclusiva da Microsoft; exportação apenas parcial e agregada; eliminação não verificável |
| **C03 — TCO** | **5** | ~R$ 48 k em 5 anos — serviço gratuito; custo restrito a implantação e conformidade |
| **C04 — Independência** | **1** | Proprietária; sem exportação integral; sem alternativa de hospedagem; a única parte aberta (`clarity-js`) é a biblioteca de instrumentação, não o produto |
| **C05 — Recursos** | **3** | Excelente em análise comportamental (heatmaps completos, session recording, insights por IA), porém **não cobre** aquisição, canais, campanhas, metas nem funis de conversão tradicionais. Cobertura ponderada entre 55 % e 74 % apenas no recorte comportamental |
| **C06 — APIs** | **2** | API de exportação com **janela temporal curta, dados agregados e cota diária de baixa ordem de grandeza**. Não é utilizável para série histórica nem para alimentação regular de BI |
| **C07 — Integrações** | **3** | Integração nativa com GA4, GTM, WordPress, Shopify e conta Microsoft. **Não recebe mais** por ausência de integração com BI |
| **C08 — Escalabilidade** | **5** | Gerenciada, sem limite de tráfego declarado |
| **C09 — Segurança** | **4** | Infraestrutura Azure; ISO 27001 e SOC 2. **Não recebe 5** por ausência de log de auditoria acessível ao cliente e por controle de acesso limitado |
| **C10 — Operação** | **5** | SaaS gerenciado |
| **C11 — Comunidade** | **3** | Base de usuários ampla; suporte oficial da Microsoft; material moderado |
| **C12 — Documentação** | **3** | Documentação suficiente para uso, porém limitada em API e em detalhamento de tratamento de dados |

---

### 4.11 Simple Analytics — 355 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **4** | Sem cookies, sem IP armazenado, sem identificador persistente; hospedagem nos Países Baixos; DPA disponível. **Não recebe 5** porque a transferência internacional persiste (UE) e a retenção depende do plano contratado |
| **C02 — Controle** | **2** | SaaS; exportação disponível, porém de dado já processado, sem acesso ao bruto |
| **C03 — TCO** | **3** | ~R$ 315 k em 5 anos, mas **o custo escala com o volume** — no cenário de pico, a faixa de plano sobe. Contratação de fornecedor estrangeiro em moeda estrangeira |
| **C04 — Independência** | **2** | Proprietária, sem auto-hospedado; a única mitigação é a exportação |
| **C05 — Recursos** | **2** | Cobertura entre 35 % e 54 %: page views, referenciadores, campanhas, eventos simples. Sem funis, coortes, segmentação avançada, jornada, Tag Manager |
| **C06 — APIs** | **3** | API REST funcional com exportação em CSV e JSON; autenticação por chave; versionamento. Cobertura de administração limitada |
| **C07 — Integrações** | **2** | Poucas integrações nativas; sem SSO; sem Tag Manager; sem conector de BI |
| **C08 — Escalabilidade** | **4** | Gerenciada, adequada ao cenário de pico; limitada pelo plano contratado |
| **C09 — Segurança** | **4** | Hospedagem na UE; práticas declaradas maduras; escopo mínimo reduz a superfície. Sem certificação abrangente publicada |
| **C10 — Operação** | **5** | SaaS gerenciado |
| **C11 — Comunidade** | **2** | Comunidade pequena; empresa de porte reduzido; pouco material |
| **C12 — Documentação** | **3** | Documentação suficiente e clara, porém enxuta |

---

### 4.12 Cloudflare Web Analytics — 345 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **3** | Sem cookies e sem identificador persistente — favorável. **Rebaixado** por transferência internacional para os EUA e por retenção fixa definida pelo fornecedor, sem controle do Estado |
| **C02 — Controle** | **1** | Custódia exclusiva da Cloudflare; sem exportação de dado bruto; retenção não configurável |
| **C03 — TCO** | **5** | ~R$ 24 k em 5 anos — o menor do estudo; gratuito para domínios já servidos pela CDN |
| **C04 — Independência** | **1** | Proprietária; acoplada à CDN; sem portabilidade relevante |
| **C05 — Recursos** | **1** | Cobertura < 35 %: page views, referenciadores, países, navegadores e Core Web Vitals. **Sem eventos customizados, sem metas, sem funis, sem segmentação** — falha em três requisitos *Must have* |
| **C06 — APIs** | **3** | API GraphQL de analytics disponível e bem documentada — tecnicamente boa, mas com pouco dado a extrair |
| **C07 — Integrações** | **2** | Integração restrita ao ecossistema Cloudflare; sem conectores de BI |
| **C08 — Escalabilidade** | **5** | Escala de borda global, sem esforço |
| **C09 — Segurança** | **4** | Infraestrutura Cloudflare; ISO 27001 e SOC 2; superfície mínima. Sem controle de acesso granular para a funcionalidade |
| **C10 — Operação** | **5** | SaaS gerenciado; ativação por interruptor |
| **C11 — Comunidade** | **2** | Sem comunidade específica; suporte via canais gerais da Cloudflare |
| **C12 — Documentação** | **3** | Documentação suficiente, porém enxuta, refletindo o escopo reduzido |

---

### 4.13 Open Web Analytics — 300 pontos

| Critério | Nota | Justificativa |
|----------|:----:|---------------|
| **C01 — LGPD** | **4** | Auto-hospedado elimina transferência internacional e permite controle total de retenção. **Não recebe 5** porque a anonimização não é padrão e o produto rastreia por cookie por default |
| **C02 — Controle** | **5** | Auto-hospedado com acesso SQL direto ao MySQL |
| **C03 — TCO** | **3** | ~R$ 650 k em 5 anos — licença zero, mas custo de operação elevado em razão da manutenção e do esforço de mitigação de segurança |
| **C04 — Independência** | **3** | GPL v2 e código auditável — favorável. **Rebaixado** porque a independência prática é limitada: manter o projeto exigiria que o Estado assumisse o desenvolvimento |
| **C05 — Recursos** | **2** | Cobertura entre 35 % e 54 %: page views, eventos básicos, heatmaps e clickstream. Sem funis, coortes, segmentação avançada, Tag Manager |
| **C06 — APIs** | **2** | API restrita, com documentação fraca e sem versionamento |
| **C07 — Integrações** | **1** | Praticamente inexistentes; sem SSO, sem Tag Manager, sem conectores. Apenas o acesso SQL direto |
| **C08 — Escalabilidade** | **1** | Sem estratégia de escala documentada; arquitetura degrada em volumes modestos |
| **C09 — Segurança** | **1** | **Nota mínima.** Histórico de vulnerabilidade crítica de execução remota de código (CVE-2022-24637); cadência de correções irregular; sem MFA, sem log de auditoria, sem programa de divulgação de vulnerabilidades |
| **C10 — Operação** | **2** | Auto-hospedado com stack familiar (PHP + MySQL), porém a ausência de suporte e a necessidade de vigilância contínua de segurança elevam substancialmente o esforço |
| **C11 — Comunidade** | **1** | Comunidade praticamente inativa; mantenedores em número muito reduzido |
| **C12 — Documentação** | **1** | Documentação esparsa e desatualizada; dependência de leitura do código-fonte |

> **🚨 Alerta**
> Ainda que a pontuação de 300 pontos (50 %) situe o OWA na faixa "não recomendada", a decisão de exclusão **não decorre da pontuação**: decorre da **triagem eliminatória (E6)**. Mesmo que a plataforma pontuasse 500, a falha em critério eliminatório de segurança a excluiria. É exatamente para isso que a arquitetura de decisão em duas etapas foi construída.

---

## 5. Análise de sensibilidade

### 5.1 Cenários de ponderação testados

| Cenário | Vetor de pesos `[C01…C12]` | Soma |
|---------|---------------------------|-----:|
| **Base** | `15, 15, 15, 10, 10, 10, 10, 10, 10, 5, 5, 5` | 120 |
| **Conformidade máxima** | `25, 25, 10, 10, 5, 5, 5, 5, 15, 5, 5, 5` | 120 |
| **Econômico** | `10, 10, 30, 5, 10, 10, 10, 10, 10, 10, 2,5, 2,5` | 120 |
| **Capacidade analítica** | `10, 10, 10, 5, 25, 15, 15, 10, 10, 5, 2,5, 2,5` | 120 |
| **Operação enxuta** | `10, 10, 15, 5, 10, 5, 5, 15, 5, 25, 5, 10` | 120 |

Todos os cenários preservam a soma de pesos em 120, mantendo o máximo de 600 pontos e permitindo comparação direta entre cenários.

### 5.2 Resultados por cenário

| Plataforma | Base | Conformidade máxima | Econômico | Capacidade analítica | Operação enxuta |
|-----------|-----:|--------------------:|----------:|---------------------:|----------------:|
| **Matomo On-Premise** | **505** 🥇 | **530** 🥇 | **485** 🥇 | 490 🥈 | 465 |
| Plausible CE | 470 🥈 | 505 🥈 | 457,5 🥉 | 437,5 | 450 |
| Piwik PRO | 465 🥉 | 480 | 445 | **500** 🥇 | 470 |
| Matomo Cloud | 465 | 455 | 460 🥈 | 490 🥈 | **490** 🥇 |
| Umami | 445 | 485 🥉 | 455 | 390 | 445 |
| PostHog auto-hospedado | 440 | 460 | 395 | 465 | 375 |
| PostHog Cloud EU | 425 | 390 | 440 | 470 | 475 🥈 |
| Google Analytics 4 | 410 | 350 | 450 | 450 | 475 🥈 |
| Adobe Analytics | 405 | 375 | 392,5 | 477,5 🥉 | 425 |
| Microsoft Clarity | 355 | 315 | 420 | 365 | 430 |
| Simple Analytics | 355 | 365 | 372,5 | 342,5 | 405 |
| Cloudflare WA | 345 | 325 | 407,5 | 322,5 | 415 |
| Open Web Analytics | 300 | 350 | 290 | 265 | 270 |

> **📌 Observação — Memória de cálculo**
> A tabela é reproduzível a partir das notas da seção 2.1 e dos vetores de peso da seção 5.1. Exemplo de verificação — Matomo On-Premise no cenário "Conformidade máxima":
> `(25×5) + (25×5) + (10×4) + (10×5) + (5×4) + (5×4) + (5×4) + (5×3) + (15×4) + (5×3) + (5×4) + (5×4) = 530`.
> A planilha completa está em [`../anexos/matriz.md`](../anexos/matriz.md).

### 5.3 Verificação do critério de robustez

O critério de aceitação estabelecido em [`03-criterios-de-avaliacao.md`, seção 7.2](03-criterios-de-avaliacao.md#72-critério-de-robustez) exige liderança em pelo menos **4 dos 5 cenários**.

| Cenário | Líder | Pontuação do líder | Matomo On-Premise | Mantém a liderança? |
|---------|-------|-------------------:|------------------:|:-------------------:|
| Base | Matomo On-Premise | 505 | 505 (1º) | ✅ Sim |
| Conformidade máxima | Matomo On-Premise | 530 | 530 (1º) | ✅ Sim |
| Econômico | Matomo On-Premise | 485 | 485 (1º) | ✅ Sim |
| Capacidade analítica | Piwik PRO | 500 | 490 (2º, empatado com Matomo Cloud) | ❌ Não — diferença de **10 pontos (1,7 %)** |
| Operação enxuta | Matomo Cloud | 490 | 465 (5º) | ❌ Não — diferença de **25 pontos (4,2 %)** |

**Resultado: liderança em 3 de 5 cenários.**

> **⚠️ Bloco de Risco — Robustez parcial**
> O critério formal de robustez (4 de 5) **não foi plenamente atingido**. A liderança do Matomo On-Premise se inverte em dois cenários:
>
> 1. **Cenário "capacidade analítica"**: o Piwik PRO assume a liderança por 10 pontos (500 × 490). Margem de 1,7 % — dentro da incerteza inerente a uma escala de 5 pontos, na qual a alteração de uma única nota em critério de peso 10 desloca 10 pontos.
> 2. **Cenário "operação enxuta"**: o **Matomo Cloud** assume a liderança (490 × 465). Notavelmente, a plataforma vencedora nesse cenário é **a mesma plataforma em outra modalidade de entrega** — o que reforça, e não contradiz, a escolha do produto.
>
> **Fato adicional relevante:** nos três cenários em que lidera, o Matomo On-Premise não o faz por margem estreita — a diferença para o segundo colocado é de 35 pontos (base), 25 pontos (conformidade máxima) e 25 pontos (econômico). A liderança é sólida exatamente nos cenários que refletem os direcionadores declarados pelo Estado.
>
> **Interpretação:** o resultado não é frágil quanto ao **produto**, mas é sensível quanto à **modalidade de entrega**. Isso é tecnicamente coerente: quanto mais se prioriza baixo esforço operacional, mais a modalidade gerenciada supera a auto-hospedada.
>
> **Consequência para a recomendação:** a recomendação é emitida como **robusta quanto ao produto (Matomo) e condicionada quanto à modalidade (On-Premise)**. A condição é a validação da premissa **P2** — capacidade técnica interna sustentável. O roadmap ([10](10-roadmap.md)) prevê um *gate* formal de verificação dessa premissa na Onda 1, com o Matomo Cloud e o Piwik PRO como alternativas de contingência formalmente registradas no ADR ([08](08-adr.md)).

### 5.4 Ponto de virada

Cálculo de quanto os pesos precisariam mudar para alterar o líder no cenário base:

Método: partindo do cenário base, varia-se isoladamente o peso de um critério (redistribuindo a diferença proporcionalmente entre os demais) até que o líder mude.

| Cenário de virada | Alteração necessária no peso | Plausibilidade institucional |
|-------------------|-----------------------------|:----------------------------|
| **Plausible CE** assumir a liderança | Reduzir C05 (Recursos analíticos) de 10 para ≈ 2, **ou** elevar C08 (Escalabilidade) de 10 para ≈ 28 | 🔴 Muito baixa — o requisito RN-02 (análise de funil de serviços digitais) é *Must have* e depende diretamente de C05 |
| **Piwik PRO** assumir a liderança | Reduzir simultaneamente C03 (TCO) de 15 para ≈ 6 **e** C04 (Independência) de 10 para ≈ 4 | 🟡 Baixa — exigiria abandonar economicidade e independência como direcionadores declarados |
| **Umami** assumir a liderança | Elevar C03 (TCO) de 15 para ≈ 27, reduzindo C05 e C07 proporcionalmente | 🟠 Média — plausível sob restrição orçamentária severa |
| **Matomo Cloud** assumir a liderança | Elevar C10 (Operação) de 5 para ≈ 17 | 🟠 Média — plausível se a premissa P2 for invalidada |
| **GA4** assumir a liderança | Reduzir C01 + C02 de 30 pontos combinados para ≤ 10 | 🔴 Praticamente nula — contraria a restrição legal R1 e o veto do DPO |
| **Adobe Analytics** assumir a liderança | Reduzir C03 (TCO) para ≈ 2 **e** C01 + C02 para ≤ 14 combinados | 🔴 Nula — inviável simultaneamente por economicidade e por conformidade |

> **📌 Observação — Interpretação dos pontos de virada**
> Dois cenários de virada são institucionalmente plausíveis, e ambos apontam para conclusões compatíveis com a recomendação:
>
> 1. **Umami sob restrição orçamentária severa.** Esse cenário é neutralizado pela triagem eliminatória, e não pela ponderação: o Umami não atende ao requisito RF-27 (funil de conversão), que é *Must have*. Uma plataforma eliminada não pode liderar, independentemente da pontuação.
> 2. **Matomo Cloud caso a premissa P2 seja invalidada.** Esse cenário não altera a escolha do produto — apenas a modalidade de entrega. Está formalmente previsto como alternativa de contingência no ADR ([08](08-adr.md)).
>
> Nenhum ponto de virada plausível conduz a uma plataforma estruturalmente distinta da recomendada.

---

## 6. Análise de dominância

Uma alternativa **domina** outra se for igual ou melhor em todos os critérios e estritamente melhor em ao menos um. Alternativas dominadas podem ser eliminadas sem depender de pesos — é o teste mais forte de uma análise multicritério, porque independe da ponderação escolhida.

### 6.1 Relações de dominância identificadas

| Dominante | Dominada | Critérios em que é superior | Critérios em que é inferior |
|-----------|----------|----------------------------|----------------------------|
| **Matomo On-Premise** | **Open Web Analytics** | C01, C05, C06, C07, C08, C09, C10, C11, C12 | Nenhum |
| **Plausible CE** | **Simple Analytics** | C01, C02, C03, C04, C05, C08, C11, C12 | C10 (Operação) |
| **Matomo Cloud** | **PostHog Cloud EU** | C01, C02, C03, C04 | Nenhum |
| **Umami** | **Cloudflare Web Analytics** | C01, C02, C04, C05, C06, C11 | C08, C10 |

> **✅ Bloco de Decisão — Dominância estrita**
> O **Matomo On-Premise domina estritamente o Open Web Analytics**: é superior em 9 critérios e inferior em nenhum. Não existe qualquer atribuição de pesos, por mais extrema, sob a qual o OWA supere o Matomo On-Premise. A eliminação do OWA é, portanto, **independente da ponderação** — é uma conclusão matematicamente robusta, e não uma consequência das escolhas metodológicas deste estudo.
>
> O **Matomo Cloud domina o PostHog Cloud EU** pela mesma lógica: superior em 4 critérios, inferior em nenhum.

### 6.2 Fronteira de Pareto

As alternativas **não dominadas** — isto é, aquelas em que qualquer melhoria em um critério implica piora em outro — constituem a fronteira de Pareto. Só elas são candidatas legítimas.

| Plataforma | Na fronteira? | Justificativa |
|-----------|:-------------:|---------------|
| **Matomo On-Premise** | ✅ | Melhor em C01, C02, C04; não dominada por ninguém |
| **Plausible CE** | ✅ | Empata no melhor em C01, C02; superior ao Matomo em C08 |
| **Umami** | ✅ | Único com nota 5 simultânea em C01, C02 e C03 |
| **Piwik PRO** | ✅ | Melhor em C08 e C09 entre as de nota 5 em C01 |
| **Matomo Cloud** | ✅ | Melhor combinação de C05 com C10 |
| **PostHog auto-hospedado** | ✅ | Melhor em C05 entre as de nota 5 em C02 |
| **Adobe Analytics** | ✅ | Melhor em C06; empata no melhor em C05, C07, C08, C09 |
| **Google Analytics 4** | ✅ | Melhor em C07, C11, C12 |
| **Microsoft Clarity** | ✅ | Empata no melhor em C03 |
| **Cloudflare WA** | ✅ | Empata no melhor em C03 com C08=5 |
| **Simple Analytics** | ❌ | Dominada por Plausible CE (exceto em C10) |
| **PostHog Cloud EU** | ❌ | **Dominada** por Matomo Cloud |
| **Open Web Analytics** | ❌ | **Dominada** por Matomo On-Premise |

---

## 7. Conclusão da matriz

### 7.1 Resultado quantitativo

> **✅ Bloco de Decisão — Resultado da matriz**
>
> **O Matomo On-Premise obtém a maior pontuação ponderada: 505 de 600 pontos (84,2 %)**, único resultado na faixa 🟢 "Recomendada" (≥ 80 %).
>
> **Vantagem sobre o 2º colocado:** 35 pontos (7,4 %).
> **Vantagem sobre o GA4 (status quo de mercado):** 95 pontos (23,2 %).
>
> **Robustez:** liderança em 3 de 5 cenários de ponderação, com margens de 25 a 35 pontos sobre o segundo colocado nesses cenários. Nos dois cenários em que não lidera, é superada por margem de 10 pontos (Piwik PRO, cenário analítico) e por outra modalidade do mesmo produto (Matomo Cloud, cenário operacional).
>
> **Dominância:** situa-se na fronteira de Pareto e **domina estritamente** o Open Web Analytics.

### 7.2 Decomposição da vantagem

Origem dos 95 pontos de vantagem do Matomo On-Premise sobre o GA4:

| Critério | Peso | Matomo OP | GA4 | Diferença de pontos |
|----------|-----:|:---------:|:---:|--------------------:|
| C02 — Controle dos dados | 15 | 5 | 1 | **+60** |
| C01 — LGPD | 15 | 5 | 2 | **+45** |
| C04 — Independência tecnológica | 10 | 5 | 1 | **+40** |
| C03 — TCO | 15 | 4 | 4 | 0 |
| C05 — Recursos analíticos | 10 | 4 | 4 | 0 |
| C06 — APIs | 10 | 4 | 4 | 0 |
| C09 — Segurança | 10 | 4 | 4 | 0 |
| C11 — Comunidade | 5 | 4 | 5 | −5 |
| C12 — Documentação | 5 | 4 | 5 | −5 |
| C07 — Integrações | 10 | 4 | 5 | −10 |
| C10 — Operação | 5 | 3 | 5 | −10 |
| C08 — Escalabilidade | 10 | 3 | 5 | **−20** |
| | | | **Saldo** | **+95** |

| Grupo de critérios | Saldo do Matomo On-Premise |
|-------------------|---------------------------:|
| Conformidade e governança (C01, C02, C04) | **+145** |
| Custo, funcionalidade e segurança (C03, C05, C06, C09) | 0 |
| Conveniência e ecossistema (C07, C08, C10, C11, C12) | **−50** |
| **Saldo líquido** | **+95** |

**Leitura:** a vantagem do Matomo On-Premise é integralmente construída sobre conformidade, soberania e independência. Em custo e funcionalidade há empate técnico. Em conveniência e ecossistema, o GA4 é superior — e essa superioridade está corretamente refletida na pontuação, apenas não é suficiente para compensar a perda nos critérios de maior peso.

### 7.3 Ressalvas explícitas

1. **A recomendação é condicionada quanto à modalidade.** A liderança do Matomo On-Premise depende da validade da premissa **P2** (capacidade técnica interna). Se essa premissa for invalidada, a modalidade recomendada passa a ser o **Matomo Cloud** (líder no cenário "operação enxuta") ou o **Piwik PRO**.

2. **A diferença para o Piwik PRO no cenário analítico não sustenta afirmação de superioridade.** 10 pontos em 600 (1,7 %) equivalem à alteração de uma única nota em um critério de peso 10 — dentro da margem de incerteza da escala. Naquele cenário específico, as duas plataformas devem ser tratadas como equivalentes.

3. **O Plausible CE é a melhor alternativa complementar, não substituta.** Sua pontuação de 470 é obtida com cobertura funcional de nota 3 — adequada a portais de conteúdo, insuficiente para análise de serviços transacionais.

4. **Nenhuma nota deste estudo foi medida em ambiente do Estado.** As notas de escalabilidade e desempenho decorrem de documentação e de arquitetura conhecida. A prova de conceito prevista na Onda 1 do roadmap deve confirmá-las.

---

## Navegação

| ⬅️ Anterior | ➡️ Próximo |
|------------|-----------|
| [06 — Trade-offs](06-tradeoffs.md) | [08 — ADR](08-adr.md) |
