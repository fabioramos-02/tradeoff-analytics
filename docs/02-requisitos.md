# 02 — Requisitos

> **Fase TOGAF ADM:** B — Business Architecture / Requirements Management
> **Anterior:** [01 — Contexto](01-contexto.md) · **Próximo:** [03 — Critérios de avaliação](03-criterios-de-avaliacao.md)

---

## Sumário

- [1. Convenções de notação](#1-convenções-de-notação)
- [2. Requisitos legais e de conformidade (RL)](#2-requisitos-legais-e-de-conformidade-rl)
- [3. Requisitos funcionais (RF)](#3-requisitos-funcionais-rf)
- [4. Requisitos não funcionais (RNF)](#4-requisitos-não-funcionais-rnf)
- [5. Requisitos de integração (RI)](#5-requisitos-de-integração-ri)
- [6. Requisitos de governança (RG)](#6-requisitos-de-governança-rg)
- [7. Requisitos de negócio (RN)](#7-requisitos-de-negócio-rn)
- [8. Matriz de rastreabilidade](#8-matriz-de-rastreabilidade)
- [9. Cenários de atributo de qualidade (ATAM)](#9-cenários-de-atributo-de-qualidade-atam)

---

## 1. Convenções de notação

### 1.1 Priorização — MoSCoW

| Marcador | Significado | Efeito na avaliação |
|----------|-------------|--------------------|
| **M** — *Must have* | Obrigatório | **Eliminatório.** Plataforma que não atende é descartada, independentemente da pontuação |
| **S** — *Should have* | Importante | Peso alto na matriz de decisão |
| **C** — *Could have* | Desejável | Peso baixo; funciona como critério de desempate |
| **W** — *Won't have (now)* | Fora desta iteração | Registrado para revisões futuras |

### 1.2 Verificabilidade

Todo requisito possui um **método de verificação** declarado:

| Método | Descrição |
|--------|-----------|
| **DOC** | Verificação em documentação oficial do fornecedor |
| **INSP** | Inspeção de código-fonte, licença ou configuração |
| **TEST** | Teste funcional em ambiente controlado (PoC) |
| **BENCH** | Medição sob carga (benchmark) |
| **JUR** | Parecer jurídico ou do Encarregado de Dados |

---

## 2. Requisitos legais e de conformidade (RL)

| ID | Requisito | Prioridade | Verificação | Fundamento |
|----|-----------|:----------:|:-----------:|-----------|
| **RL-01** | A plataforma deve permitir que o Estado atue como **controlador** dos dados, com definição inequívoca do papel de eventual operador | **M** | JUR/DOC | LGPD art. 5º, VI e VII |
| **RL-02** | A plataforma deve permitir operação **sem transferência internacional de dados pessoais**, ou, se houver, dispor de base legal do art. 33 documentada e cláusulas contratuais adequadas | **M** | JUR/DOC | LGPD arts. 33–36 |
| **RL-03** | A plataforma deve suportar **anonimização de endereço IP** antes da persistência | **M** | TEST/DOC | LGPD art. 12 (dado anonimizado fora do escopo da lei); princípio da necessidade (art. 6º, III) |
| **RL-04** | A plataforma deve permitir operação **sem cookies de identificação persistente** ou com cookies estritamente necessários | **S** | TEST/DOC | LGPD art. 7º; precedente CNIL |
| **RL-05** | A plataforma deve permitir **definição e aplicação automática de política de retenção**, com exclusão física do dado bruto ao término do prazo | **M** | TEST/DOC | LGPD art. 15 e 16 (término do tratamento) |
| **RL-06** | A plataforma deve permitir atender a **requisições de titular** (acesso, correção, eliminação, portabilidade) em prazo legal | **M** | TEST/DOC | LGPD art. 18 |
| **RL-07** | A plataforma deve fornecer mecanismo de **opt-out** acessível ao titular | **M** | TEST/DOC | LGPD art. 18, §2º; art. 8º, §5º |
| **RL-08** | A plataforma deve respeitar sinais de **Do Not Track** e/ou Global Privacy Control quando configurado | **C** | TEST/DOC | Boa prática; não vinculante no Brasil |
| **RL-09** | A plataforma deve manter **registro de operações de tratamento** ou permitir sua construção a partir de sua documentação | **M** | DOC | LGPD art. 37 |
| **RL-10** | A plataforma deve produzir **logs de auditoria** de acesso administrativo e alterações de configuração | **S** | TEST/DOC | LGPD art. 46; boas práticas de segurança |
| **RL-11** | A plataforma deve permitir **integração com Consent Management Platform** quando o consentimento for a base legal aplicável | **S** | TEST/DOC | LGPD art. 8º |
| **RL-12** | Os dados devem ser **criptografados em trânsito** (TLS 1.2+) e o repouso deve poder ser criptografado | **M** | TEST/INSP | LGPD art. 46; boas práticas |
| **RL-13** | A plataforma não deve realizar **fingerprinting** de dispositivo sem base legal explícita | **S** | INSP/DOC | LGPD art. 6º, princípios da finalidade e necessidade |
| **RL-14** | Dados de uso de portais públicos devem poder ser **publicados em formato aberto** quando não contiverem dado pessoal | **C** | TEST/DOC | Lei 12.527/2011; Lei 14.129/2021 |

> **🚨 Alerta — Requisitos eliminatórios**
> RL-01, RL-02, RL-03, RL-05, RL-06, RL-07, RL-09 e RL-12 são **Must have**. Uma plataforma que falhe em qualquer um deles está eliminada, independentemente de sua pontuação na matriz. A aplicação desse filtro está registrada em [`07-matriz-decisao.md`](07-matriz-decisao.md), seção "Triagem eliminatória".

---

## 3. Requisitos funcionais (RF)

### 3.1 Coleta

| ID | Requisito | Prioridade | Verificação |
|----|-----------|:----------:|:-----------:|
| RF-01 | Rastrear page views com URL, título, referenciador e timestamp | **M** | TEST |
| RF-02 | Rastrear **eventos customizados** (categoria, ação, nome, valor) | **M** | TEST |
| RF-03 | Rastrear **downloads de arquivos** e cliques em links externos automaticamente | **S** | TEST |
| RF-04 | Rastrear **buscas internas** do site (termo, resultados, categoria) | **S** | TEST |
| RF-05 | Suportar **dimensões customizadas** em escopo de visita e de ação | **S** | TEST |
| RF-06 | Suportar **tracking server-side** (API HTTP direta, sem JavaScript) | **S** | TEST/DOC |
| RF-07 | Suportar **importação de logs de servidor web** como fonte de dados | **C** | DOC |
| RF-08 | Detectar e classificar **bots e crawlers**, excluindo-os das métricas | **S** | TEST/DOC |
| RF-09 | Suportar **Single Page Applications** (rastreamento de mudança de rota sem reload) | **M** | TEST |
| RF-10 | Suportar **cross-domain tracking** entre portais do Estado | **C** | DOC |
| RF-11 | Rastrear **conteúdo de mídia** (vídeo/áudio: play, pause, tempo assistido) | **C** | DOC |
| RF-12 | Permitir **exclusão de IPs internos** (faixas do Estado) das métricas | **S** | TEST |

### 3.2 Análise e relatórios

| ID | Requisito | Prioridade | Verificação |
|----|-----------|:----------:|:-----------:|
| RF-20 | Dashboards configuráveis por usuário | **M** | TEST |
| RF-21 | Relatórios de audiência: visitantes, visitas, page views, taxa de rejeição, duração | **M** | TEST |
| RF-22 | Relatórios de aquisição: canais, referenciadores, campanhas (UTM) | **M** | TEST |
| RF-23 | Relatórios de comportamento: páginas mais vistas, entrada, saída, fluxo de navegação | **M** | TEST |
| RF-24 | Relatórios de tecnologia: dispositivo, navegador, SO, resolução | **S** | TEST |
| RF-25 | Relatórios geográficos: país, região, cidade | **S** | TEST |
| RF-26 | **Metas (goals)** com atribuição e valor | **M** | TEST |
| RF-27 | **Funis de conversão** multi-etapa com identificação do ponto de abandono | **M** | TEST |
| RF-28 | **Segmentação** de dados por qualquer dimensão, aplicável a todos os relatórios | **M** | TEST |
| RF-29 | **Comparação de períodos** e de segmentos | **S** | TEST |
| RF-30 | **Relatórios em tempo real** (latência < 5 minutos) | **S** | TEST |
| RF-31 | **Coortes** e análise de **retenção** | **C** | DOC |
| RF-32 | **Mapas de calor** (clique, movimento, rolagem) | **C** | DOC |
| RF-33 | **Gravação de sessão** com mascaramento de campos sensíveis | **C** | DOC |
| RF-34 | **Análise de formulários** (abandono por campo, tempo de preenchimento) | **C** | DOC |
| RF-35 | **Testes A/B** e teste multivariado | **W** | DOC |
| RF-36 | **Jornada do usuário** / fluxo de usuários entre páginas | **S** | TEST |
| RF-37 | Relatórios agendados por e-mail | **C** | TEST |
| RF-38 | Anotações de eventos institucionais na linha do tempo dos gráficos | **C** | DOC |

### 3.3 Administração

| ID | Requisito | Prioridade | Verificação |
|----|-----------|:----------:|:-----------:|
| RF-40 | Gestão multi-site em instância única, com **segregação de acesso por site/órgão** | **M** | TEST |
| RF-41 | Perfis de permissão granulares (visualizar, escrever, administrar, superusuário) | **M** | TEST |
| RF-42 | Agregação de métricas de múltiplos sites em visão consolidada (*roll-up*) | **S** | DOC |
| RF-43 | Gestão de usuários com autenticação forte (MFA) | **M** | TEST/DOC |
| RF-44 | Interface administrativa em **português do Brasil** | **S** | TEST |
| RF-45 | Gestão de tags integrada (**Tag Manager**) | **S** | DOC |

---

## 4. Requisitos não funcionais (RNF)

### 4.1 Desempenho

| ID | Requisito | Meta | Prioridade | Verificação |
|----|-----------|------|:----------:|:-----------:|
| RNF-01 | Tamanho do script de tracking (gzip) | ≤ 25 KB | **S** | BENCH |
| RNF-02 | Impacto no Largest Contentful Paint do portal | ≤ 50 ms | **M** | BENCH |
| RNF-03 | Latência do endpoint de tracking (p95) | ≤ 200 ms | **M** | BENCH |
| RNF-04 | Vazão de ingestão sustentada | ≥ 500 req/s | **M** | BENCH |
| RNF-05 | Vazão de ingestão em pico absorvido (com fila) | ≥ 2.000 req/s | **S** | BENCH |
| RNF-06 | Tempo de carga de relatório padrão (30 dias, p95) | ≤ 3 s | **S** | BENCH |
| RNF-07 | Tempo de carga de relatório com segmento customizado (p95) | ≤ 10 s | **C** | BENCH |
| RNF-08 | O script de tracking deve carregar de forma assíncrona e não bloqueante | **M** | INSP |

### 4.2 Disponibilidade e resiliência

| ID | Requisito | Meta | Prioridade | Verificação |
|----|-----------|------|:----------:|:-----------:|
| RNF-10 | Disponibilidade do endpoint de coleta | ≥ 99,5 % mensal | **M** | BENCH |
| RNF-11 | Disponibilidade da interface de relatórios | ≥ 99,0 % mensal | **S** | BENCH |
| RNF-12 | **RPO** (Recovery Point Objective) | ≤ 24 h | **M** | TEST |
| RNF-13 | **RTO** (Recovery Time Objective) | ≤ 8 h | **S** | TEST |
| RNF-14 | Indisponibilidade da plataforma de analytics **não pode degradar** os portais rastreados | **M** | TEST |
| RNF-15 | Suporte a arquitetura ativa/passiva ou ativa/ativa | **S** | DOC |
| RNF-16 | Perda máxima aceitável de eventos em janela de pico | ≤ 1 % | **S** | BENCH |

### 4.3 Escalabilidade

| ID | Requisito | Prioridade | Verificação |
|----|-----------|:----------:|:-----------:|
| RNF-20 | Escalabilidade **horizontal** da camada de coleta | **M** | DOC/BENCH |
| RNF-21 | Escalabilidade **horizontal** da camada de processamento/arquivamento | **S** | DOC |
| RNF-22 | Desacoplamento de ingestão via fila ou buffer | **S** | DOC |
| RNF-23 | Suportar crescimento de 5× no volume sem redesenho arquitetural | **S** | BENCH |
| RNF-24 | Suportar ≥ 150 propriedades em instância única | **M** | DOC |

### 4.4 Segurança

| ID | Requisito | Prioridade | Verificação |
|----|-----------|:----------:|:-----------:|
| RNF-30 | TLS 1.2+ obrigatório em todos os endpoints | **M** | TEST |
| RNF-31 | Autenticação multifator para usuários administrativos | **M** | DOC/TEST |
| RNF-32 | Integração com provedor de identidade corporativo via **SAML 2.0 ou OIDC** | **S** | DOC |
| RNF-33 | Controle de acesso baseado em papéis (RBAC) | **M** | TEST |
| RNF-34 | Log de auditoria imutável ou exportável para SIEM | **S** | DOC |
| RNF-35 | Processo público e responsivo de divulgação de vulnerabilidades | **M** | DOC |
| RNF-36 | Cadência de correções de segurança compatível com o risco (patch crítico ≤ 15 dias) | **M** | DOC |
| RNF-37 | Compatibilidade com Content Security Policy restritiva | **S** | TEST |
| RNF-38 | Criptografia de dados em repouso (nível de banco ou de disco) | **S** | DOC |
| RNF-39 | Ausência de CVE crítica sem correção disponível | **M** | INSP |

### 4.5 Operação e manutenibilidade

| ID | Requisito | Prioridade | Verificação |
|----|-----------|:----------:|:-----------:|
| RNF-40 | Distribuição via **container OCI** oficial | **S** | DOC |
| RNF-41 | Implantação em **Kubernetes** documentada (Helm chart ou manifests) | **C** | DOC |
| RNF-42 | Procedimento de atualização documentado e reversível | **M** | DOC |
| RNF-43 | Exposição de métricas operacionais para monitoramento (Prometheus ou equivalente) | **C** | DOC |
| RNF-44 | Procedimento de backup e restauração documentado | **M** | DOC |
| RNF-45 | Configuração declarável por arquivo/variável de ambiente (Infrastructure as Code) | **S** | DOC |
| RNF-46 | Ciclo de releases previsível com política de versões suportadas (LTS ou equivalente) | **S** | DOC |

### 4.6 Usabilidade e acessibilidade

| ID | Requisito | Prioridade | Verificação |
|----|-----------|:----------:|:-----------:|
| RNF-50 | Interface disponível em pt-BR | **S** | TEST |
| RNF-51 | Interface administrativa aderente ao WCAG 2.1 nível AA | **C** | TEST |
| RNF-52 | Curva de aprendizado compatível com usuário não técnico (analista de comunicação) | **S** | TEST |
| RNF-53 | Documentação de usuário disponível | **S** | DOC |

---

## 5. Requisitos de integração (RI)

| ID | Requisito | Prioridade | Verificação |
|----|-----------|:----------:|:-----------:|
| RI-01 | **API REST** de leitura de relatórios com autenticação por token | **M** | TEST |
| RI-02 | Exportação de dados em **CSV, JSON e XML** | **M** | TEST |
| RI-03 | Exportação **em lote** de dado bruto (não apenas agregado) | **S** | DOC |
| RI-04 | API de **importação** de dados históricos | **S** | DOC |
| RI-05 | Conector ou método documentado de integração com **Power BI** | **S** | DOC/TEST |
| RI-06 | Integração com **Grafana** (datasource nativo ou via API) | **C** | DOC |
| RI-07 | Integração com **Metabase** e **Apache Superset** | **C** | TEST |
| RI-08 | Integração com **Google Looker Studio** | **C** | DOC |
| RI-09 | Integração com **Qlik Sense** | **C** | DOC |
| RI-10 | SDK ou biblioteca oficial/comunitária em **Python** e **Node.js** | **S** | DOC |
| RI-11 | Suporte a **webhooks** para notificação de eventos | **C** | DOC |
| RI-12 | Integração com **Google Tag Manager** | **C** | DOC |
| RI-13 | **Tag Manager próprio** com versionamento e ambientes | **S** | DOC |
| RI-14 | Integração com **Consent Management Platform** (IAB TCF ou equivalente) | **S** | DOC |
| RI-15 | Autenticação federada via **OAuth 2.0 / OpenID Connect** | **S** | DOC |
| RI-16 | Autenticação federada via **SAML 2.0** | **C** | DOC |
| RI-17 | Acesso direto ao banco de dados para ETL (réplica de leitura) | **S** | INSP |
| RI-18 | Rate limits da API documentados e compatíveis com carga de BI | **S** | DOC |
| RI-19 | Versionamento explícito da API com política de depreciação | **S** | DOC |

---

## 6. Requisitos de governança (RG)

| ID | Requisito | Prioridade | Verificação |
|----|-----------|:----------:|:-----------:|
| RG-01 | Código-fonte **auditável** pelo Estado | **S** | INSP |
| RG-02 | Licença que permita uso, modificação e operação sem custo de licença por volume | **S** | INSP |
| RG-03 | **Portabilidade total** dos dados sem custo adicional e sem formato proprietário fechado | **M** | TEST |
| RG-04 | Ausência de cláusula contratual que restrinja migração ou exportação | **M** | JUR |
| RG-05 | Schema de dados documentado ou inspecionável | **S** | INSP |
| RG-06 | Roadmap público do produto | **C** | DOC |
| RG-07 | Governança do projeto transparente (para software livre: processo de contribuição, mantenedores identificados) | **C** | INSP |
| RG-08 | Existência de comunidade ativa ou de fornecedor com viabilidade financeira demonstrável | **S** | INSP/DOC |
| RG-09 | Possibilidade de suporte comercial contratável (opcional, não obrigatório) | **C** | DOC |
| RG-10 | Ausência de dependência de serviço em nuvem específico não substituível | **S** | INSP |

---

## 7. Requisitos de negócio (RN)

| ID | Requisito | Prioridade | Métrica de sucesso |
|----|-----------|:----------:|-------------------|
| RN-01 | Habilitar mensuração de adesão a serviços digitais | **M** | 100 % dos serviços digitais prioritários com meta configurada |
| RN-02 | Habilitar identificação de gargalos em jornadas de serviço | **M** | Funil configurado para os 20 serviços de maior volume |
| RN-03 | Padronizar analytics entre órgãos estaduais | **S** | ≥ 80 % dos portais em instância padronizada |
| RN-04 | Alimentar painéis corporativos de transformação digital | **S** | Pipeline de dados operacional para o BI do Estado |
| RN-05 | Reduzir custo por milhão de eventos em relação à linha de base | **C** | Custo/M eventos medido e monitorado |
| RN-06 | Permitir publicação de estatísticas de uso em portal de transparência | **C** | Conjunto de dados abertos publicado |
| RN-07 | Tempo de provisionamento de nova propriedade para um órgão | **S** | ≤ 1 dia útil |

---

## 8. Matriz de rastreabilidade

Rastreia direcionadores arquiteturais → requisitos → critérios de avaliação.

| Direcionador ([01](01-contexto.md#10-direcionadores-arquiteturais)) | Requisitos vinculados | Critério da matriz ([03](03-criterios-de-avaliacao.md)) | Peso |
|---|---|---|---:|
| **D1** — Soberania de dados | RL-01, RL-02, RG-03, RG-04, RG-10, RI-17 | Controle dos dados | 15 |
| **D2** — Conformidade LGPD | RL-01 a RL-14, RNF-30, RNF-38 | LGPD | 15 |
| **D3** — Independência tecnológica | RG-01, RG-02, RG-05, RG-07, RG-10 | Independência tecnológica | 10 |
| **D4** — Sustentabilidade operacional | RNF-40 a RNF-46, RNF-52 | Operação | 5 |
| **D5** — Capacidade analítica | RF-01 a RF-45 | Recursos analíticos | 10 |
| **D6** — Integração com BI | RI-01 a RI-19 | APIs (10) + Integrações (10) | 20 |
| **D7** — Economicidade | RN-05, RG-02, RG-09 | TCO | 15 |
| **D8** — Elasticidade | RNF-01 a RNF-24 | Escalabilidade | 10 |
| **Transversal** — Segurança | RNF-30 a RNF-39, RL-10, RL-12 | Segurança | 10 |
| **Transversal** — Sustentabilidade do produto | RG-06, RG-08, RNF-46 | Comunidade (5) + Documentação (5) | 10 |

**Total dos pesos: 120** (máximo teórico da matriz: 120 × 5 = **600 pontos**).

---

## 9. Cenários de atributo de qualidade (ATAM)

Cenários no formato canônico do ATAM: `<fonte> <estímulo> <artefato> <ambiente> <resposta> <medida da resposta>`.

### QA-01 — Desempenho sob pico

| Elemento | Valor |
|----------|-------|
| **Atributo** | Desempenho / Escalabilidade |
| **Fonte** | Cidadãos acessando portal após divulgação de resultado de concurso |
| **Estímulo** | Aumento súbito de 30× no tráfego, sustentado por 45 minutos |
| **Artefato** | Endpoint de coleta e camada de persistência |
| **Ambiente** | Operação normal, sem aviso prévio |
| **Resposta** | Todos os eventos são aceitos e enfileirados; nenhum portal é degradado |
| **Medida** | Perda de eventos ≤ 1 %; latência do endpoint p95 ≤ 200 ms; impacto zero no LCP do portal |
| **Requisitos** | RNF-03, RNF-05, RNF-14, RNF-16, RNF-22 |

> **⚠️ Ponto de sensibilidade identificado**
> Este cenário é atendido apenas por arquiteturas com **desacoplamento de ingestão**. Instalações Matomo sem o plugin `QueuedTracking` gravam de forma síncrona no MySQL e **falham neste cenário**. Trata-se do principal requisito de adequação da arquitetura vigente ([01, seção 5.3, lacuna L1](01-contexto.md#53-lacunas-técnicas-da-arquitetura-vigente)).

### QA-02 — Solicitação de eliminação por titular

| Elemento | Valor |
|----------|-------|
| **Atributo** | Conformidade / Manutenibilidade |
| **Fonte** | Titular de dados exercendo direito do art. 18 da LGPD |
| **Estímulo** | Solicitação de eliminação dos dados associados ao seu identificador |
| **Artefato** | Base de dado bruto |
| **Ambiente** | Operação normal |
| **Resposta** | Dados localizados e eliminados; confirmação registrada em log de auditoria |
| **Medida** | Atendimento em ≤ 15 dias; comprovação auditável |
| **Requisitos** | RL-06, RL-09, RL-10 |

> **📌 Observação**
> Esse cenário revela um **trade-off entre privacidade e capacidade de resposta**: quanto mais anonimizado o dado na coleta, menor o risco regulatório — porém torna-se impossível localizar o dado de um titular específico para eliminá-lo. Paradoxalmente, **a anonimização completa é a resposta ótima**: se não há dado pessoal, o art. 18 não se aplica (LGPD art. 12). Discutido em [`06-tradeoffs.md`](06-tradeoffs.md).

### QA-03 — Migração de plataforma

| Elemento | Valor |
|----------|-------|
| **Atributo** | Portabilidade / Modificabilidade |
| **Fonte** | SETDIG, em decorrência de revisão arquitetural futura |
| **Estímulo** | Decisão de migrar para outra plataforma |
| **Artefato** | Histórico completo de dados + configuração de sites e metas |
| **Ambiente** | Planejado, com janela definida |
| **Resposta** | Histórico exportado integralmente em formato aberto e reimportado ou arquivado |
| **Medida** | 100 % do dado agregado exportado; custo de saída ≤ 15 % do TCO anual; janela ≤ 90 dias |
| **Requisitos** | RG-03, RG-04, RI-02, RI-03, RI-04 |

### QA-04 — Integração com BI corporativo

| Elemento | Valor |
|----------|-------|
| **Atributo** | Interoperabilidade |
| **Fonte** | Equipe de BI do Estado |
| **Estímulo** | Necessidade de consolidar métricas de 80 portais em painel único, atualizado diariamente |
| **Artefato** | API de relatórios / réplica de banco |
| **Ambiente** | Janela noturna de ETL |
| **Resposta** | Extração completa concluída sem impactar coleta nem relatórios |
| **Medida** | Janela de extração ≤ 2 h; zero degradação da coleta; sem estouro de rate limit |
| **Requisitos** | RI-01, RI-05, RI-17, RI-18 |

### QA-05 — Vulnerabilidade crítica divulgada

| Elemento | Valor |
|----------|-------|
| **Atributo** | Segurança |
| **Fonte** | Pesquisador de segurança / CVE publicado |
| **Estímulo** | Divulgação de vulnerabilidade crítica com RCE na plataforma |
| **Artefato** | Instância de produção |
| **Ambiente** | Operação normal |
| **Resposta** | Correção disponibilizada pelo fornecedor e aplicada pela STI |
| **Medida** | Correção do fornecedor ≤ 15 dias; aplicação pela STI ≤ 72 h após disponibilização |
| **Requisitos** | RNF-35, RNF-36, RNF-39 |

> **🚨 Alerta**
> Este cenário é o principal fator de eliminação do **Open Web Analytics**, cujo histórico de manutenção e o CVE de execução remota de código documentado indicam incapacidade estrutural de atender à medida de resposta. Detalhado em [`../plataformas/open-web-analytics.md`](../plataformas/open-web-analytics.md).

### QA-06 — Descontinuidade do fornecedor

| Elemento | Valor |
|----------|-------|
| **Atributo** | Disponibilidade / Governança |
| **Fonte** | Fornecedor da plataforma |
| **Estímulo** | Encerramento do produto, aquisição por terceiro ou mudança de licenciamento restritiva |
| **Artefato** | Plataforma inteira |
| **Ambiente** | Aviso de 6 a 12 meses (cenário otimista) ou imediato (cenário pessimista) |
| **Resposta** | Estado mantém operação com a versão existente e executa plano de migração |
| **Medida** | Continuidade operacional ≥ 12 meses sem o fornecedor; dados preservados integralmente |
| **Requisitos** | RG-01, RG-02, RG-03, RG-08, RG-10 |

> **✅ Bloco de Decisão — Resposta arquitetural ao QA-06**
> Apenas plataformas **auto-hospedado com licença livre** atendem plenamente a este cenário: o Estado continua operando o software indefinidamente mesmo sem o fornecedor. Plataformas SaaS proprietárias apresentam resposta estruturalmente inferior — a continuidade depende integralmente do fornecedor. Este é o fundamento técnico do peso 10 atribuído ao critério "Independência tecnológica".

---

## Navegação

| ⬅️ Anterior | ➡️ Próximo |
|------------|-----------|
| [01 — Contexto](01-contexto.md) | [03 — Critérios de avaliação](03-criterios-de-avaliacao.md) |
