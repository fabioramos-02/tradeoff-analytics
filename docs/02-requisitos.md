# 02 — Requisitos

> **Fase TOGAF ADM:** B — Business Architecture / Requirements Management
> **Anterior:** [01 — Contexto](01-contexto.md) · **Próximo:** [03 — Critérios](03-criterios.md)

## Sumário

- [1. Notação](#1-notação)
- [2. Requisitos legais (RL)](#2-requisitos-legais-rl)
- [3. Requisitos funcionais (RF)](#3-requisitos-funcionais-rf)
- [4. Requisitos não funcionais (RNF)](#4-requisitos-não-funcionais-rnf)
- [5. Requisitos de integração (RI)](#5-requisitos-de-integração-ri)
- [6. Requisitos de governança (RG)](#6-requisitos-de-governança-rg)
- [7. Requisitos de negócio (RN)](#7-requisitos-de-negócio-rn)
- [8. Matriz de rastreabilidade](#8-matriz-de-rastreabilidade)
- [9. Cenários ATAM](#9-cenários-atam)

---

## 1. Notação

**MoSCoW:** M = *Must* (eliminatório); S = *Should* (peso alto); C = *Could* (desempate); W = *Won't (now)*.

**Verificação:** DOC (doc oficial), INSP (código/licença/config), TEST (PoC funcional), BENCH (medição sob carga), JUR (parecer).

---

## 2. Requisitos legais (RL)

| ID | Requisito | Prio | Verif | Fundamento |
|----|-----------|:----:|:-----:|-----------|
| RL-01 | Estado atua como **controlador**; papel de operador definido | **M** | JUR/DOC | LGPD art. 5º |
| RL-02 | Operar sem transferência internacional OU com base do art. 33 documentada | **M** | JUR/DOC | LGPD arts. 33–36 |
| RL-03 | Anonimização de IP antes da persistência | **M** | TEST/DOC | LGPD arts. 6º, 12 |
| RL-04 | Operar sem cookies persistentes ou com cookies estritamente necessários | **S** | TEST/DOC | LGPD art. 7º; CNIL |
| RL-05 | Política de retenção com exclusão física ao término | **M** | TEST/DOC | LGPD arts. 15, 16 |
| RL-06 | Atender requisições de titular (arts. 18) em prazo legal | **M** | TEST/DOC | LGPD art. 18 |
| RL-07 | Opt-out acessível ao titular | **M** | TEST/DOC | LGPD art. 8º, §5º |
| RL-08 | Respeitar Do Not Track / Global Privacy Control (se configurado) | **C** | TEST/DOC | Boa prática |
| RL-09 | Registro de operações de tratamento | **M** | DOC | LGPD art. 37 |
| RL-10 | Logs de auditoria de acesso administrativo | **S** | TEST/DOC | LGPD art. 46 |
| RL-11 | Integração com Consent Management Platform | **S** | TEST/DOC | LGPD art. 8º |
| RL-12 | TLS 1.2+ em trânsito; criptografia possível em repouso | **M** | TEST/INSP | LGPD art. 46 |
| RL-13 | Sem fingerprinting sem base legal explícita | **S** | INSP/DOC | LGPD art. 6º |
| RL-14 | Estatísticas publicáveis em formato aberto quando sem dado pessoal | **C** | TEST/DOC | Lei 12.527; 14.129 |

> **🚨 Alerta — Eliminatórios**
> RL-01, RL-02, RL-03, RL-05, RL-06, RL-07, RL-09 e RL-12 são **Must have**. Falha em qualquer um elimina a plataforma, independentemente da pontuação. Triagem em [`05-matriz.md`](05-matriz.md).

---

## 3. Requisitos funcionais (RF)

### 3.1 Coleta

| ID | Requisito | Prio |
|----|-----------|:----:|
| RF-01 | Page views (URL, título, referrer, timestamp) | **M** |
| RF-02 | Eventos customizados (categoria, ação, nome, valor) | **M** |
| RF-03 | Downloads e cliques externos automáticos | **S** |
| RF-04 | Busca interna do site | **S** |
| RF-05 | Dimensões customizadas (visita e ação) | **S** |
| RF-06 | Tracking server-side (API HTTP direta) | **S** |
| RF-08 | Detecção de bots/crawlers | **S** |
| RF-09 | SPAs (rastreamento de rota sem reload) | **M** |
| RF-10 | Cross-domain tracking entre portais | **C** |
| RF-12 | Exclusão de IPs internos do Estado | **S** |

### 3.2 Análise e relatórios

| ID | Requisito | Prio |
|----|-----------|:----:|
| RF-20 | Dashboards configuráveis | **M** |
| RF-21 | Audiência (visitantes, page views, rejeição, duração) | **M** |
| RF-22 | Aquisição (canais, referrers, UTM) | **M** |
| RF-23 | Comportamento (páginas, entrada, saída, fluxo) | **M** |
| RF-24 | Tecnologia (device, browser, SO) | **S** |
| RF-25 | Geografia (país, região, cidade) | **S** |
| RF-26 | Metas com atribuição e valor | **M** |
| RF-27 | Funis multi-etapa com ponto de abandono | **M** |
| RF-28 | Segmentação aplicável a todos os relatórios | **M** |
| RF-29 | Comparação de períodos e segmentos | **S** |
| RF-30 | Tempo real (latência < 5 min) | **S** |
| RF-31 | Coortes e retenção | **C** |
| RF-32 | Heatmaps | **C** |
| RF-33 | Session replay com mascaramento | **C** |
| RF-34 | Análise de formulários | **C** |
| RF-35 | A/B testing | **W** |
| RF-36 | Jornada do usuário | **S** |

### 3.3 Administração

| ID | Requisito | Prio |
|----|-----------|:----:|
| RF-40 | Multi-site com segregação por site/órgão | **M** |
| RF-41 | RBAC granular | **M** |
| RF-42 | Roll-up de múltiplos sites | **S** |
| RF-43 | MFA para admin | **M** |
| RF-44 | UI em pt-BR | **S** |
| RF-45 | Tag Manager | **S** |

---

## 4. Requisitos não funcionais (RNF)

### 4.1 Desempenho

| ID | Requisito | Meta | Prio |
|----|-----------|------|:----:|
| RNF-01 | Script tracking (gzip) | ≤ 25 KB | **S** |
| RNF-02 | Impacto no LCP do portal | ≤ 50 ms (75 ms no Xvia, conf. [ADR-002](06-adr-002.md)) | **M** |
| RNF-03 | Latência do endpoint tracking (p95) | ≤ 200 ms | **M** |
| RNF-04 | Vazão sustentada | ≥ 500 req/s | **M** |
| RNF-05 | Vazão em pico absorvido (com fila) | ≥ 2.000 req/s | **S** |
| RNF-06 | Carga relatório padrão 30d (p95) | ≤ 3 s | **S** |
| RNF-08 | Script assíncrono e não bloqueante | — | **M** |

### 4.2 Disponibilidade

| ID | Requisito | Meta | Prio |
|----|-----------|------|:----:|
| RNF-10 | Disponibilidade do endpoint de coleta | ≥ 99,5 %/mês | **M** |
| RNF-11 | Disponibilidade da UI | ≥ 99,0 %/mês | **S** |
| RNF-12 | RPO | ≤ 24 h | **M** |
| RNF-13 | RTO | ≤ 8 h | **S** |
| RNF-14 | Falha da plataforma **não pode degradar** os portais | — | **M** |
| RNF-16 | Perda máxima em pico | ≤ 1 % | **S** |

### 4.3 Escalabilidade

| ID | Requisito | Prio |
|----|-----------|:----:|
| RNF-20 | Escalabilidade horizontal da coleta | **M** |
| RNF-22 | Desacoplamento via fila/buffer | **S** |
| RNF-23 | Crescimento 5× sem redesenho | **S** |
| RNF-24 | ≥ 150 propriedades em instância única | **M** |

### 4.4 Segurança

| ID | Requisito | Prio |
|----|-----------|:----:|
| RNF-30 | TLS 1.2+ obrigatório | **M** |
| RNF-31 | MFA para admin | **M** |
| RNF-32 | SAML 2.0 ou OIDC | **S** |
| RNF-33 | RBAC | **M** |
| RNF-34 | Log auditoria exportável para SIEM | **S** |
| RNF-35 | Processo público de disclosure | **M** |
| RNF-36 | Patch crítico ≤ 15 dias | **M** |
| RNF-37 | CSP restritiva compatível | **S** |
| RNF-38 | Criptografia em repouso | **S** |
| RNF-39 | Sem CVE crítica sem correção | **M** |

### 4.5 Operação

| ID | Requisito | Prio |
|----|-----------|:----:|
| RNF-40 | Container OCI oficial | **S** |
| RNF-41 | Helm chart / manifests K8s | **C** |
| RNF-42 | Update documentado e reversível | **M** |
| RNF-43 | Métricas para Prometheus | **C** |
| RNF-44 | Backup e restore documentados | **M** |
| RNF-45 | IaC (config declarável) | **S** |
| RNF-46 | Ciclo de release previsível (LTS) | **S** |

### 4.6 Usabilidade

| ID | Requisito | Prio |
|----|-----------|:----:|
| RNF-50 | UI em pt-BR | **S** |
| RNF-51 | WCAG 2.1 AA | **C** |
| RNF-52 | Curva compatível com analista não técnico | **S** |

---

## 5. Requisitos de integração (RI)

| ID | Requisito | Prio |
|----|-----------|:----:|
| RI-01 | API REST de leitura com token | **M** |
| RI-02 | Export em CSV/JSON/XML | **M** |
| RI-03 | Export em lote de dado bruto | **S** |
| RI-04 | API de importação histórica | **S** |
| RI-05 | Integração com Power BI | **S** |
| RI-06 | Integração com Grafana | **C** |
| RI-07 | Integração com Metabase / Superset | **C** |
| RI-08 | Integração com Looker Studio | **C** |
| RI-10 | SDK Python e Node.js | **S** |
| RI-11 | Webhooks | **C** |
| RI-12 | Google Tag Manager | **C** |
| RI-13 | Tag Manager próprio versionado | **S** |
| RI-14 | Consent Management (IAB TCF ou eq.) | **S** |
| RI-15 | OAuth 2.0 / OIDC | **S** |
| RI-16 | SAML 2.0 | **C** |
| RI-17 | Acesso ao banco (réplica leitura) para ETL | **S** |
| RI-18 | Rate limits documentados e compatíveis com BI | **S** |
| RI-19 | Versionamento explícito de API | **S** |

---

## 6. Requisitos de governança (RG)

| ID | Requisito | Prio |
|----|-----------|:----:|
| RG-01 | Código-fonte auditável pelo Estado | **S** |
| RG-02 | Licença permite uso/modificação/operação sem custo por volume | **S** |
| RG-03 | Portabilidade total sem custo/formato fechado | **M** |
| RG-04 | Sem cláusula que restrinja migração/export | **M** |
| RG-05 | Schema documentado ou inspecionável | **S** |
| RG-06 | Roadmap público | **C** |
| RG-07 | Governança transparente do projeto | **C** |
| RG-08 | Comunidade ativa ou fornecedor viável | **S** |
| RG-09 | Suporte comercial contratável (opcional) | **C** |
| RG-10 | Sem dependência de nuvem específica não substituível | **S** |

---

## 7. Requisitos de negócio (RN)

| ID | Requisito | Prio | Métrica |
|----|-----------|:----:|---------|
| RN-01 | Mensurar adesão a serviços digitais | **M** | 100 % dos prioritários com meta |
| RN-02 | Identificar gargalos em jornadas | **M** | Funil configurado nos top 20 serviços |
| RN-03 | Padronizar analytics entre órgãos | **S** | ≥ 80 % dos portais em instância padrão |
| RN-04 | Alimentar BI corporativo | **S** | Pipeline operacional |
| RN-05 | Reduzir custo/M eventos vs. baseline | **C** | Medido e monitorado |
| RN-06 | Publicar estatísticas no portal de transparência | **C** | Dataset aberto publicado |
| RN-07 | Provisionamento de nova propriedade | **S** | ≤ 1 dia útil |

---

## 8. Matriz de rastreabilidade

| Direcionador ([01](01-contexto.md#9-direcionadores-arquiteturais)) | Requisitos | Critério ([03](03-criterios.md)) | Peso |
|---|---|---|---:|
| D1 — Soberania | RL-01, RL-02, RG-03, RG-04, RG-10, RI-17 | Controle dos dados | 15 |
| D2 — LGPD | RL-01 a RL-14, RNF-30, RNF-38 | LGPD | 15 |
| D3 — Independência | RG-01, RG-02, RG-05, RG-07, RG-10 | Independência | 10 |
| D4 — Operação | RNF-40 a RNF-46, RNF-52 | Operação | 5 |
| D5 — Analítica | RF-01 a RF-45 | Recursos analíticos | 10 |
| D6 — BI | RI-01 a RI-19 | APIs + Integrações | 20 |
| D7 — Economicidade | RN-05, RG-02, RG-09 | TCO (informativo no ADR-002) | 0 |
| D8 — Elasticidade | RNF-01 a RNF-24 | Escalabilidade | 10 |
| Transversal — Segurança | RNF-30 a RNF-39, RL-10, RL-12 | Segurança | 10 |
| Transversal — Sustentabilidade do produto | RG-06, RG-08, RNF-46 | Comunidade + Documentação | 10 |

**Soma dos pesos: 105** (máx. teórico: 105 × 5 = **525 pontos** no ADR-002; para leitura histórica de 600 pontos, ver ADR-001 em `anexos/historico/`).

---

## 9. Cenários ATAM

Formato canônico: `<fonte> <estímulo> <artefato> <ambiente> <resposta> <medida>`.

### QA-01 — Desempenho sob pico

Cidadão acessa portal após resultado de concurso. Tráfego 30× por 45 min. Endpoint de coleta + persistência. Operação normal, sem aviso. **Resposta:** todos os eventos aceitos/enfileirados; portais não degradados. **Medida:** perda ≤ 1 %, p95 ≤ 200 ms, LCP inalterado.
**Requisitos:** RNF-03, RNF-05, RNF-14, RNF-16, RNF-22.

> **⚠️ Sensibilidade:** só arquiteturas com **desacoplamento de ingestão** atendem. Matomo sem `QueuedTracking` falha (lacuna L1 do parque).

### QA-02 — Eliminação por titular

Titular exerce art. 18 LGPD. Solicita eliminação. Base de dado bruto. Operação normal. **Resposta:** localizado, eliminado, log de auditoria. **Medida:** ≤ 15 dias, comprovação auditável.
**Requisitos:** RL-06, RL-09, RL-10.

> **📌 Paradoxo útil:** anonimização completa é resposta ótima — se não há dado pessoal, art. 18 não se aplica (LGPD art. 12).

### QA-03 — Migração de plataforma

SETDIG decide migrar. Histórico completo + config. Janela planejada. **Resposta:** dado agregado exportado em formato aberto. **Medida:** 100 % agregado, custo de saída ≤ 15 % TCO anual, janela ≤ 90 dias.
**Requisitos:** RG-03, RG-04, RI-02, RI-03, RI-04.

### QA-04 — Integração com BI

Equipe BI consolida 80 portais em painel diário. API/réplica de banco. Janela noturna. **Resposta:** extração completa sem impactar coleta. **Medida:** ≤ 2 h, zero degradação, sem estouro de rate limit.
**Requisitos:** RI-01, RI-05, RI-17, RI-18.

### QA-05 — Vulnerabilidade crítica

CVE crítica RCE publicado. Instância em produção. **Resposta:** patch do fornecedor aplicado pela STI. **Medida:** correção fornecedor ≤ 15 dias; aplicação ≤ 72 h.
**Requisitos:** RNF-35, RNF-36, RNF-39.

### QA-06 — Descontinuidade do fornecedor

Fornecedor encerra/vende/muda licença. Aviso 6–12 meses (otimista) ou imediato (pessimista). **Resposta:** Estado mantém operação com versão existente e migra. **Medida:** continuidade ≥ 12 meses, dados preservados.
**Requisitos:** RG-01, RG-02, RG-03, RG-08, RG-10.

> **✅ Bloco de Decisão — QA-06**
> Só **auto-hospedado com licença livre** atende plenamente. Estado opera indefinidamente sem o fornecedor. Fundamento do peso 10 em "Independência tecnológica".

---

## Navegação

| ⬅️ Anterior | ➡️ Próximo |
|------------|-----------|
| [01 — Contexto](01-contexto.md) | [03 — Critérios](03-criterios.md) |
