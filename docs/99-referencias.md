# 12 — Referências

> **Anterior:** [11 — Riscos](11-riscos.md) · **Voltar ao** [README](../README.md)

---

## Sumário

- [1. Critério de seleção de fontes](#1-critério-de-seleção-de-fontes)
- [2. Normativos brasileiros](#2-normativos-brasileiros)
- [3. Normativos e decisões internacionais](#3-normativos-e-decisões-internacionais)
- [4. Arcabouços e metodologias de arquitetura](#4-arcabouços-e-metodologias-de-arquitetura)
- [5. Normas técnicas](#5-normas-técnicas)
- [6. RFCs e especificações](#6-rfcs-e-especificações)
- [7. Documentação oficial — Matomo](#7-documentação-oficial--matomo)
- [8. Documentação oficial — Google Analytics 4](#8-documentação-oficial--google-analytics-4)
- [9. Documentação oficial — Plausible](#9-documentação-oficial--plausible)
- [10. Documentação oficial — Umami](#10-documentação-oficial--umami)
- [11. Documentação oficial — Open Web Analytics](#11-documentação-oficial--open-web-analytics)
- [12. Documentação oficial — Adobe Analytics](#12-documentação-oficial--adobe-analytics)
- [13. Documentação oficial — Simple Analytics](#13-documentação-oficial--simple-analytics)
- [14. Documentação oficial — Microsoft Clarity](#14-documentação-oficial--microsoft-clarity)
- [15. Documentação oficial — Cloudflare Web Analytics](#15-documentação-oficial--cloudflare-web-analytics)
- [16. Documentação oficial — PostHog](#16-documentação-oficial--posthog)
- [17. Documentação oficial — Piwik PRO](#17-documentação-oficial--piwik-pro)
- [18. Licenças](#18-licenças)
- [19. Segurança e vulnerabilidades](#19-segurança-e-vulnerabilidades)
- [20. Tecnologias de infraestrutura](#20-tecnologias-de-infraestrutura)
- [21. Ferramentas de BI e integração](#21-ferramentas-de-bi-e-integração)
- [22. Adoção no setor público](#22-adoção-no-setor-público)
- [23. Fontes secundárias](#23-fontes-secundárias)
- [24. Registro de verificação](#24-registro-de-verificação)

---

## 1. Critério de seleção de fontes

### 1.1 Hierarquia de fontes adotada

| Nível | Tipo de fonte | Aceitação | Exemplos |
|:-----:|--------------|:---------:|----------|
| **1** | Instrumento normativo (lei, decreto, instrução normativa) | ✅ Primária | LGPD, Lei do Governo Digital |
| **1** | Decisão de autoridade regulatória ou judicial | ✅ Primária | Decisões de DPAs, TJUE |
| **1** | Norma técnica (ISO, ABNT, W3C, IETF) | ✅ Primária | ISO/IEC 27001, RFC 6749 |
| **2** | Documentação oficial do fornecedor | ✅ Primária | docs.matomo.org, developers.google.com |
| **2** | Repositório de código-fonte e arquivo de licença | ✅ Primária | GitHub do projeto, `LICENSE` |
| **2** | Especificação de API publicada pelo fornecedor | ✅ Primária | Referência de API |
| **3** | Publicação acadêmica ou institucional revisada | ✅ Primária | SEI/CMU, The Open Group |
| **4** | Base de vulnerabilidades reconhecida | ✅ Primária | NVD/NIST, MITRE CVE |
| **5** | Análise de consultoria de mercado | ⚠️ Secundária | Relatórios de analistas |
| **6** | Blog técnico, artigo de opinião, conteúdo de marketing | ⚠️ Secundária, rotulada | Blogs de fornecedores |
| **7** | Fórum, rede social, conteúdo não atribuído | ❌ Não aceita | — |

### 1.2 Regra aplicada

> **📌 Observação**
> Conforme declarado em [`../README.md`, seção 4.1](../README.md#41-regra-de-evidência), toda afirmação técnica deste estudo é rastreável a fonte de nível 1 a 4. Fontes de nível 5 e 6 aparecem apenas quando explicitamente rotuladas como secundárias, e nunca sustentam sozinhas uma conclusão do estudo.

### 1.3 Aviso sobre volatilidade

> **⚠️ Bloco de Risco**
> URLs de documentação de fornecedores são voláteis: fornecedores reorganizam sites, renomeiam produtos e removem páginas. As URLs abaixo refletem a estrutura vigente na data-base de **julho/2026**.
>
> **Recomendação de preservação:** antes da homologação, capturar as páginas críticas (licenciamento, preços, política de privacidade, termos de uso) em formato PDF com data e hora, anexando-as ao processo administrativo. Isso preserva a evidência independentemente de alterações posteriores no site do fornecedor.

---

## 2. Normativos brasileiros

### 2.1 Federais

| Norma | Ementa | Aplicação neste estudo |
|-------|--------|------------------------|
| **Constituição Federal de 1988**, art. 37 e art. 70 | Princípios da Administração Pública; controle de economicidade | Fundamento dos critérios de eficiência e economicidade |
| **Lei nº 13.709/2018** — LGPD | Proteção de dados pessoais | Base de todos os requisitos legais (RL-01 a RL-14) |
| **Lei nº 14.129/2021** — Lei do Governo Digital | Princípios de governo digital, interoperabilidade, dados abertos, software público | Fundamento de APIs abertas, transparência e reuso |
| **Lei nº 12.527/2011** — LAI | Acesso à informação | Publicação de estatísticas de uso como dado aberto |
| **Lei nº 12.965/2014** — Marco Civil da Internet | Direitos e deveres na Internet; guarda de registros de acesso | Contexto de tratamento de registro de navegação |
| **Lei nº 14.133/2021** — Lei de Licitações | Contratações públicas; estudo técnico preliminar (art. 18) | Fundamento formal do estudo como instrução de eventual contratação |
| **Decreto nº 10.046/2019** | Governança de compartilhamento de dados na Administração federal | Referência de boas práticas |
| **Decreto nº 10.332/2020** | Estratégia de Governo Digital | Contexto estratégico |
| **IN SGD/ME nº 94/2022** | Processo de contratação de soluções de TIC | Modelo de referência de ETP e Termo de Referência |

**Fonte oficial:** Portal da Legislação — Planalto — `https://www.planalto.gov.br/`

### 2.2 Autoridade Nacional de Proteção de Dados

| Documento | Aplicação |
|-----------|-----------|
| Guia Orientativo — Tratamento de Dados Pessoais pelo Poder Público | Fundamento das bases legais aplicáveis ao setor público |
| Guia Orientativo — Segurança da Informação para Agentes de Tratamento de Pequeno Porte | Referência de controles mínimos |
| Regulamento de aplicação de sanções administrativas | Dimensionamento do risco regulatório |
| Guia Orientativo sobre Transferência Internacional de Dados | Fundamento da análise dos arts. 33 a 36 |

**Fonte oficial:** `https://www.gov.br/anpd/`

### 2.3 Estaduais — Mato Grosso do Sul

| Norma | Ementa | Aplicação |
|-------|--------|-----------|
| **Lei nº 6.035/2022** | Estrutura da Administração estadual; institui a Secretaria-Executiva de Transformação Digital — SETDIG | Competência institucional para estabelecer padrões |
| **Decreto nº 16.166/2023** | Regulamenta a estrutura da SETDIG | Atribuições da SGD e da STI |

**Fonte oficial:** Diário Oficial do Estado de MS — `https://www.spdo.ms.gov.br/`
**Portal institucional:** `https://www.setdig.ms.gov.br/`

> **📌 Observação — Verificação pendente**
> A existência de decreto estadual específico de MS regulamentando a aplicação da LGPD no âmbito da Administração estadual (comitê gestor, designação de encarregado, política de segurança da informação) deve ser confirmada junto à Procuradoria-Geral do Estado antes da homologação, conforme atividade 0.4 do roadmap.

---

## 3. Normativos e decisões internacionais

| Referência | Conteúdo | Aplicação |
|-----------|----------|-----------|
| **Regulamento (UE) 2016/679 — RGPD** | Regulamento Geral de Proteção de Dados | Base comparativa da LGPD; fundamento das decisões de DPAs |
| **TJUE — Caso C-311/18 (*Schrems II*)**, 16/07/2020 | Invalida o Privacy Shield; exige avaliação caso a caso das transferências para os EUA | Fundamento da análise de transferência internacional |
| **Decisões de autoridades de proteção de dados sobre Google Analytics (2022)** — Áustria (DSB), França (CNIL), Itália (Garante), Dinamarca (Datatilsynet) | Uso do Google Analytics em sítios europeus considerado incompatível com o RGPD | Evidência primária do risco regulatório do GA4 |
| **CNIL — Medição de audiência e isenção de consentimento** | Condições para que ferramentas de medição de audiência sejam isentas de consentimento; configuração do Matomo reconhecida | **Precedente central da justificativa J4** do ADR |
| **Decisão de Execução (UE) 2023/1795 — EU-US Data Privacy Framework** | Decisão de adequação para transferências UE→EUA | Mitigação parcial do risco pós-*Schrems II*; **não aplicável ao Brasil** |
| **EDPB — Diretrizes sobre bases legais e sobre transferências internacionais** | Interpretação do RGPD | Referência interpretativa por analogia |
| **ePrivacy Directive 2002/58/CE** (e alterações) | Cookies e comunicações eletrônicas | Contexto do requisito de consentimento para cookies |

**Fontes oficiais:**
- EUR-Lex: `https://eur-lex.europa.eu/`
- CNIL: `https://www.cnil.fr/`
- EDPB: `https://www.edpb.europa.eu/`
- Curia (TJUE): `https://curia.europa.eu/`

---

## 4. Arcabouços e metodologias de arquitetura

| Referência | Organização | Aplicação |
|-----------|------------|-----------|
| **TOGAF Standard, 10th Edition** — Architecture Development Method | The Open Group | Fases A e B do ADM; contexto e requisitos |
| **ArchiMate 3.2 Specification** | The Open Group | Notação de referência para os diagramas de arquitetura |
| **ATAM: Method for Architecture Evaluation** (CMU/SEI-2000-TR-004) | SEI / Carnegie Mellon University | Árvore de atributos de qualidade; pontos de sensibilidade e trade-off points |
| **Quality Attribute Workshops (QAW)** (CMU/SEI-2003-TR-016) | SEI / CMU | Formato dos cenários de atributo de qualidade |
| **Documenting Software Architectures: Views and Beyond** | SEI / CMU | Estrutura de documentação arquitetural |
| **Architecture Decision Records** — Michael Nygard, "Documenting Architecture Decisions" (2011) | — | Formato do ADR |
| **MADR — Markdown Any Decision Records, v4.0** | Comunidade | Estrutura complementar do ADR |
| **Multi-Attribute Utility Theory** — Keeney & Raiffa, *Decisions with Multiple Objectives* (1976) | — | Fundamento matemático da matriz de decisão ponderada |
| **Gartner Decision Framework for Technology Selection** | Gartner | Estrutura de critérios ponderados e avaliação de fornecedor |
| **C4 Model** — Simon Brown | — | Níveis de abstração dos diagramas |

**Fontes oficiais:**
- The Open Group: `https://www.opengroup.org/togaf`
- SEI/CMU: `https://resources.sei.cmu.edu/`
- ADR: `https://adr.github.io/`
- MADR: `https://adr.github.io/madr/`

---

## 5. Normas técnicas

| Norma | Título | Aplicação |
|-------|--------|-----------|
| **ISO/IEC 27001:2022** | Sistemas de gestão de segurança da informação — Requisitos | Referência de certificação de fornecedor (critério C09) |
| **ISO/IEC 27002:2022** | Controles de segurança da informação | Referência de controles compensatórios |
| **ISO/IEC 27017:2015** | Controles para serviços em nuvem | Avaliação de fornecedores SaaS |
| **ISO/IEC 27018:2019** | Proteção de dados pessoais em nuvem pública | Avaliação de fornecedores SaaS |
| **ISO/IEC 27701:2019** | Extensão para gestão de privacidade | Referência de conformidade |
| **ISO/IEC 25010:2023** | Modelo de qualidade de produto de software | Taxonomia dos atributos de qualidade |
| **ISO/IEC 42010:2022** | Descrição de arquitetura de sistemas e software | Estrutura da documentação arquitetural |
| **ABNT NBR ISO/IEC 27001** | Versão brasileira | Referência nacional |
| **SOC 2 Type II** (AICPA) | Relatório de controles de organização de serviço | Referência de certificação de fornecedor |
| **WCAG 2.1** (W3C) | Diretrizes de acessibilidade para conteúdo web | Requisito RNF-51 |
| **eMAG 3.1** | Modelo de Acessibilidade em Governo Eletrônico | Referência nacional de acessibilidade |

**Fontes oficiais:**
- ISO: `https://www.iso.org/`
- ABNT: `https://www.abnt.org.br/`
- W3C WCAG: `https://www.w3.org/TR/WCAG21/`
- eMAG: `https://emag.governoeletronico.gov.br/`

---

## 6. RFCs e especificações

| Especificação | Título | Aplicação |
|--------------|--------|-----------|
| **RFC 6749** | The OAuth 2.0 Authorization Framework | Autenticação de APIs (requisito RI-15) |
| **RFC 6750** | OAuth 2.0 Bearer Token Usage | Autenticação por token |
| **RFC 7519** | JSON Web Token (JWT) | Autenticação de APIs |
| **RFC 8446** | TLS 1.3 | Requisito RL-12 e RNF-30 |
| **RFC 6265** | HTTP State Management Mechanism (Cookies) | Fundamento técnico do requisito RL-04 |
| **RFC 6265bis** | Cookies: HTTP State Management Mechanism (atualização) | `SameSite`, cookies particionados |
| **RFC 9110** | HTTP Semantics | Base do protocolo de coleta |
| **RFC 7231, seção 5.5.2** | Referer header | Contexto da coleta de referenciador |
| **RFC 2119 / RFC 8174** | Palavras-chave de nível de requisito | Convenção de MUST/SHOULD nos requisitos |
| **OpenID Connect Core 1.0** | Camada de identidade sobre OAuth 2.0 | Requisito RI-15 |
| **SAML 2.0** (OASIS) | Security Assertion Markup Language | Requisito RI-16 |
| **W3C Tracking Preference Expression (DNT)** | Do Not Track | Requisito RL-08 |
| **Global Privacy Control (GPC)** | Sinal de preferência de privacidade | Requisito RL-08 |
| **W3C Beacon API** | `navigator.sendBeacon()` | Mecanismo de envio não bloqueante do tracker |
| **W3C Content Security Policy Level 3** | CSP | Requisito RNF-37 |
| **IAB TCF v2.2** | Transparency and Consent Framework | Requisito RI-14 |

**Fontes oficiais:**
- IETF: `https://www.rfc-editor.org/`
- W3C: `https://www.w3.org/TR/`
- OpenID Foundation: `https://openid.net/developers/specs/`
- OASIS: `https://www.oasis-open.org/`

---

## 7. Documentação oficial — Matomo

| Recurso | URL |
|---------|-----|
| Site oficial | `https://matomo.org/` |
| Documentação de usuário | `https://matomo.org/docs/` |
| Documentação de desenvolvedor | `https://developer.matomo.org/` |
| **Reporting API** | `https://developer.matomo.org/api-reference/reporting-api` |
| **Tracking HTTP API** | `https://developer.matomo.org/api-reference/tracking-api` |
| Tracking JavaScript API | `https://developer.matomo.org/guides/tracking-javascript-guide` |
| Referência de métricas e dimensões | `https://developer.matomo.org/api-reference/reporting-api-metadata` |
| Repositório principal | `https://github.com/matomo-org/matomo` |
| Imagem Docker oficial | `https://hub.docker.com/_/matomo` |
| Repositório Docker | `https://github.com/matomo-org/docker` |
| Marketplace de plugins | `https://plugins.matomo.org/` |
| Plugin QueuedTracking | `https://github.com/matomo-org/plugin-QueuedTracking` |
| Guia de otimização para alto tráfego | `https://matomo.org/docs/optimize-how-to/` |
| Configuração de arquivamento por cron | `https://matomo.org/docs/setup-auto-archiving/` |
| Requisitos de sistema | `https://matomo.org/docs/requirements/` |
| Guia de instalação | `https://matomo.org/docs/installation/` |
| Guia de atualização | `https://matomo.org/docs/update/` |
| **Privacidade e conformidade RGPD** | `https://matomo.org/gdpr-analytics/` |
| Guia de conformidade RGPD | `https://matomo.org/docs/gdpr/` |
| Anonimização de dados | `https://matomo.org/docs/privacy/` |
| Retenção e exclusão de dados | `https://matomo.org/docs/privacy-how-to/` |
| Segurança | `https://matomo.org/security/` |
| Política de divulgação de vulnerabilidades | `https://matomo.org/security-policy/` |
| Changelog | `https://matomo.org/changelog/` |
| Roadmap e planejamento | `https://github.com/matomo-org/matomo/milestones` |
| Fórum da comunidade | `https://forum.matomo.org/` |
| Matomo Tag Manager | `https://matomo.org/tag-manager/` |
| Matomo Cloud — preços | `https://matomo.org/pricing/` |
| SDKs oficiais (PHP, iOS, Android) | `https://developer.matomo.org/api-reference/tracking-api#client-libraries` |
| Plugin de conector para Looker Studio | `https://plugins.matomo.org/LookerStudio` |
| Esquema de banco de dados | `https://developer.matomo.org/guides/persistence-and-the-mysql-backend` |
| Importação de logs de servidor | `https://matomo.org/log-analytics/` |

---

## 8. Documentação oficial — Google Analytics 4

| Recurso | URL |
|---------|-----|
| Central de Ajuda do GA4 | `https://support.google.com/analytics/` |
| Documentação de desenvolvedor | `https://developers.google.com/analytics` |
| **Data API v1** | `https://developers.google.com/analytics/devguides/reporting/data/v1` |
| **Admin API v1** | `https://developers.google.com/analytics/devguides/config/admin/v1` |
| Measurement Protocol (GA4) | `https://developers.google.com/analytics/devguides/collection/protocol/ga4` |
| gtag.js | `https://developers.google.com/tag-platform/gtagjs` |
| Cotas e limites da API | `https://developers.google.com/analytics/devguides/reporting/data/v1/quotas` |
| Amostragem de dados | `https://support.google.com/analytics/answer/13331684` |
| Limites de cardinalidade | `https://support.google.com/analytics/answer/12226705` |
| Retenção de dados | `https://support.google.com/analytics/answer/7667196` |
| Exportação para BigQuery | `https://support.google.com/analytics/answer/9358801` |
| Consent Mode | `https://developers.google.com/tag-platform/security/guides/consent` |
| Termos de Serviço do Google Analytics | `https://marketingplatform.google.com/about/analytics/terms/` |
| Emenda de tratamento de dados | `https://privacy.google.com/businesses/processorterms/` |
| Cláusulas contratuais padrão | `https://business.safety.google/gdpr/` |
| Certificações e conformidade | `https://cloud.google.com/security/compliance` |
| Google Analytics 360 | `https://marketingplatform.google.com/about/analytics-360/` |
| Google Tag Manager | `https://developers.google.com/tag-platform/tag-manager` |
| Encerramento do Universal Analytics | `https://support.google.com/analytics/answer/11583528` |

---

## 9. Documentação oficial — Plausible

| Recurso | URL |
|---------|-----|
| Site oficial | `https://plausible.io/` |
| Documentação | `https://plausible.io/docs` |
| **Stats API** | `https://plausible.io/docs/stats-api` |
| **Events API** | `https://plausible.io/docs/events-api` |
| Sites API | `https://plausible.io/docs/sites-api` |
| Repositório (Community Edition) | `https://github.com/plausible/analytics` |
| Guia de self-hosting | `https://github.com/plausible/community-edition` |
| Documentação de self-hosting | `https://plausible.io/docs/self-hosting` |
| Configuração de self-hosting | `https://plausible.io/docs/self-hosting-configuration` |
| Política de dados | `https://plausible.io/data-policy` |
| Conformidade com RGPD, CCPA e PECR | `https://plausible.io/privacy-focused-web-analytics` |
| Preços | `https://plausible.io/#pricing` |
| Script de tracking | `https://plausible.io/docs/script-extensions` |
| Comparação com Google Analytics | `https://plausible.io/vs-google-analytics` |
| Changelog | `https://plausible.io/changelog` |

---

## 10. Documentação oficial — Umami

| Recurso | URL |
|---------|-----|
| Site oficial | `https://umami.is/` |
| Documentação | `https://umami.is/docs` |
| **API** | `https://umami.is/docs/api` |
| API de envio de eventos | `https://umami.is/docs/sending-stats` |
| Repositório | `https://github.com/umami-software/umami` |
| Guia de instalação | `https://umami.is/docs/install` |
| Instalação via Docker | `https://umami.is/docs/running-on-docker` |
| Variáveis de ambiente | `https://umami.is/docs/environment-variables` |
| Tracker | `https://umami.is/docs/tracker-configuration` |
| Preços da nuvem | `https://umami.is/pricing` |
| Política de privacidade | `https://umami.is/privacy` |

---

## 11. Documentação oficial — Open Web Analytics

| Recurso | URL |
|---------|-----|
| Site oficial | `http://www.openwebanalytics.com/` |
| Repositório | `https://github.com/Open-Web-Analytics/Open-Web-Analytics` |
| Wiki de documentação | `https://github.com/Open-Web-Analytics/Open-Web-Analytics/wiki` |
| Guia de instalação | `https://github.com/Open-Web-Analytics/Open-Web-Analytics/wiki/Installation` |
| Documentação de API | `https://github.com/Open-Web-Analytics/Open-Web-Analytics/wiki/API` |
| Releases | `https://github.com/Open-Web-Analytics/Open-Web-Analytics/releases` |
| **CVE-2022-24637** | `https://nvd.nist.gov/vuln/detail/CVE-2022-24637` |

---

## 12. Documentação oficial — Adobe Analytics

| Recurso | URL |
|---------|-----|
| Documentação do produto | `https://experienceleague.adobe.com/docs/analytics.html` |
| **Analytics API 2.0** | `https://developer.adobe.com/analytics-apis/docs/2.0/` |
| Autenticação (OAuth Server-to-Server) | `https://developer.adobe.com/developer-console/docs/guides/authentication/` |
| Data Feeds | `https://experienceleague.adobe.com/docs/analytics/export/analytics-data-feed/data-feed-overview.html` |
| Data Warehouse | `https://experienceleague.adobe.com/docs/analytics/export/data-warehouse/data-warehouse.html` |
| Bulk Data Insertion API | `https://developer.adobe.com/analytics-apis/docs/2.0/guides/endpoints/bulk-data-insertion/` |
| Adobe Experience Platform Web SDK | `https://experienceleague.adobe.com/docs/experience-platform/edge/home.html` |
| Adobe Experience Platform Data Collection (Launch) | `https://experienceleague.adobe.com/docs/experience-platform/tags/home.html` |
| Analysis Workspace | `https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html` |
| Privacidade e RGPD | `https://experienceleague.adobe.com/docs/analytics/admin/data-governance/an-gdpr-overview.html` |
| Conformidade e certificações | `https://www.adobe.com/trust/compliance/compliance-list.html` |
| Adobe Experience Cloud — segurança | `https://www.adobe.com/trust/security.html` |

---

## 13. Documentação oficial — Simple Analytics

| Recurso | URL |
|---------|-----|
| Site oficial | `https://www.simpleanalytics.com/` |
| Documentação | `https://docs.simpleanalytics.com/` |
| **API** | `https://docs.simpleanalytics.com/api` |
| Exportação de dados | `https://docs.simpleanalytics.com/export-data` |
| Script | `https://docs.simpleanalytics.com/script` |
| Eventos | `https://docs.simpleanalytics.com/events` |
| Privacidade — o que é coletado | `https://docs.simpleanalytics.com/what-we-collect` |
| RGPD | `https://www.simpleanalytics.com/gdpr` |
| Preços | `https://www.simpleanalytics.com/pricing` |
| DPA | `https://www.simpleanalytics.com/dpa` |

---

## 14. Documentação oficial — Microsoft Clarity

| Recurso | URL |
|---------|-----|
| Site oficial | `https://clarity.microsoft.com/` |
| Documentação | `https://learn.microsoft.com/en-us/clarity/` |
| **Data Export API** | `https://learn.microsoft.com/en-us/clarity/setup-and-installation/clarity-data-export-api` |
| Mascaramento de conteúdo | `https://learn.microsoft.com/en-us/clarity/setup-and-installation/clarity-masking` |
| Integração com Google Analytics | `https://learn.microsoft.com/en-us/clarity/third-party-integrations/google-analytics-integration` |
| Repositório `clarity-js` (MIT) | `https://github.com/microsoft/clarity` |
| Termos de uso | `https://clarity.microsoft.com/terms` |
| Privacidade | `https://learn.microsoft.com/en-us/clarity/faq#privacy` |
| Declaração de Privacidade da Microsoft | `https://privacy.microsoft.com/privacystatement` |
| Microsoft Trust Center | `https://www.microsoft.com/trust-center` |

---

## 15. Documentação oficial — Cloudflare Web Analytics

| Recurso | URL |
|---------|-----|
| Documentação | `https://developers.cloudflare.com/web-analytics/` |
| Visão geral | `https://www.cloudflare.com/web-analytics/` |
| **GraphQL Analytics API** | `https://developers.cloudflare.com/analytics/graphql-api/` |
| Conjuntos de dados da API | `https://developers.cloudflare.com/analytics/graphql-api/features/data-sets/` |
| Beacon de RUM | `https://developers.cloudflare.com/web-analytics/get-started/` |
| Privacidade | `https://www.cloudflare.com/trust-hub/privacy-and-data-protection/` |
| Certificações | `https://www.cloudflare.com/trust-hub/compliance-resources/` |

---

## 16. Documentação oficial — PostHog

| Recurso | URL |
|---------|-----|
| Site oficial | `https://posthog.com/` |
| Documentação | `https://posthog.com/docs` |
| **API** | `https://posthog.com/docs/api` |
| HogQL | `https://posthog.com/docs/hogql` |
| SDKs | `https://posthog.com/docs/libraries` |
| Repositório | `https://github.com/PostHog/posthog` |
| **Self-hosting (Open Source)** | `https://posthog.com/docs/self-host` |
| Aviso sobre descontinuação do self-hosting gerenciado | `https://posthog.com/blog/sunsetting-helm-support-posthog` |
| Preços | `https://posthog.com/pricing` |
| Região de dados (EU/US) | `https://posthog.com/docs/data/data-residency` |
| Privacidade e conformidade | `https://posthog.com/docs/privacy` |
| RGPD | `https://posthog.com/docs/privacy/gdpr-compliance` |
| Segurança | `https://posthog.com/handbook/company/security` |
| Licença | `https://github.com/PostHog/posthog/blob/master/LICENSE` |
| Roadmap | `https://posthog.com/roadmap` |

---

## 17. Documentação oficial — Piwik PRO

| Recurso | URL |
|---------|-----|
| Site oficial | `https://piwik.pro/` |
| Central de ajuda | `https://help.piwik.pro/` |
| **API** | `https://developers.piwik.pro/en/latest/` |
| Analytics API | `https://developers.piwik.pro/en/latest/custom_reports/http_api/http_api.html` |
| Tracker API | `https://developers.piwik.pro/en/latest/data_collection/api/api.html` |
| Autenticação | `https://developers.piwik.pro/en/latest/platform/api_authentication.html` |
| Opções de hospedagem | `https://piwik.pro/hosting/` |
| Consent Manager | `https://piwik.pro/consent-manager/` |
| Tag Manager | `https://piwik.pro/tag-manager/` |
| Conformidade e privacidade | `https://piwik.pro/privacy-compliance/` |
| Segurança | `https://piwik.pro/security/` |
| Comparação com Matomo | `https://piwik.pro/blog/piwik-pro-vs-matomo/` ⚠️ *fonte do fornecedor — parcial* |

---

## 18. Licenças

| Licença | Texto oficial | Plataformas |
|---------|--------------|-------------|
| **GPL v3** | `https://www.gnu.org/licenses/gpl-3.0.html` | Matomo |
| **GPL v2** | `https://www.gnu.org/licenses/old-licenses/gpl-2.0.html` | Open Web Analytics |
| **AGPL v3** | `https://www.gnu.org/licenses/agpl-3.0.html` | Plausible Community Edition |
| **MIT** | `https://opensource.org/license/mit` | Umami, PostHog (core), `clarity-js` |
| **Apache 2.0** | `https://www.apache.org/licenses/LICENSE-2.0` | Referência comparativa |
| **EUPL 1.2** | `https://joinup.ec.europa.eu/collection/eupl` | Referência comparativa (GoatCounter) |

**Referências sobre licenciamento:**
- Open Source Initiative — licenças aprovadas: `https://opensource.org/licenses`
- Free Software Foundation — compatibilidade de licenças: `https://www.gnu.org/licenses/license-list.html`
- SPDX License List: `https://spdx.org/licenses/`

---

## 19. Segurança e vulnerabilidades

| Recurso | URL | Aplicação |
|---------|-----|-----------|
| **NVD — National Vulnerability Database** | `https://nvd.nist.gov/` | Consulta de CVEs das plataformas |
| **MITRE CVE** | `https://cve.mitre.org/` | Registro de vulnerabilidades |
| **CVE-2022-24637** (Open Web Analytics) | `https://nvd.nist.gov/vuln/detail/CVE-2022-24637` | Fundamento da eliminação do OWA |
| **CISA KEV Catalog** | `https://www.cisa.gov/known-exploited-vulnerabilities-catalog` | Vulnerabilidades exploradas ativamente |
| **OWASP Top 10** | `https://owasp.org/www-project-top-ten/` | Referência de ameaças de aplicação web |
| **OWASP ASVS** | `https://owasp.org/www-project-application-security-verification-standard/` | Padrão de verificação de segurança |
| **CIS Benchmarks** | `https://www.cisecurity.org/cis-benchmarks` | Hardening de SO, MySQL, Nginx |
| **NIST SP 800-53** | `https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final` | Catálogo de controles de segurança |
| **NIST SP 800-34** | `https://csrc.nist.gov/publications/detail/sp/800-34/rev-1/final` | Planejamento de contingência (DR) |

---

## 20. Tecnologias de infraestrutura

| Tecnologia | Documentação oficial | Uso na arquitetura-alvo |
|-----------|---------------------|------------------------|
| **MySQL 8.0** | `https://dev.mysql.com/doc/refman/8.0/en/` | Banco primário do Matomo |
| **MariaDB** | `https://mariadb.com/kb/en/documentation/` | Alternativa ao MySQL |
| Replicação MySQL | `https://dev.mysql.com/doc/refman/8.0/en/replication.html` | Réplica de leitura (AA-4) |
| Particionamento MySQL | `https://dev.mysql.com/doc/refman/8.0/en/partitioning.html` | Gestão de tabelas de log |
| **Redis** | `https://redis.io/docs/latest/` | Fila de ingestão (AA-1) |
| Persistência Redis (AOF/RDB) | `https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/` | Durabilidade da fila |
| **PHP 8.x** | `https://www.php.net/docs.php` | Runtime do Matomo |
| PHP-FPM | `https://www.php.net/manual/en/install.fpm.php` | Processamento de requisições |
| **Nginx** | `https://nginx.org/en/docs/` | Servidor web e proxy reverso |
| **Docker** | `https://docs.docker.com/` | Containerização |
| **Kubernetes** | `https://kubernetes.io/docs/` | Orquestração (opcional) |
| **Terraform** | `https://developer.hashicorp.com/terraform/docs` | Infrastructure as Code (AA-11) |
| **Ansible** | `https://docs.ansible.com/` | Configuração declarativa |
| **Prometheus** | `https://prometheus.io/docs/` | Métricas e monitoramento |
| **Grafana** | `https://grafana.com/docs/grafana/latest/` | Visualização de métricas |
| **ClickHouse** | `https://clickhouse.com/docs` | Banco colunar (Plausible, PostHog) |
| **PostgreSQL** | `https://www.postgresql.org/docs/` | Metadados (Plausible, PostHog, Umami) |
| **Apache Kafka** | `https://kafka.apache.org/documentation/` | Streaming (PostHog) |
| **ModSecurity / OWASP CRS** | `https://coreruleset.org/docs/` | Regras de WAF |

---

## 21. Ferramentas de BI e integração

| Ferramenta | Documentação | Aplicação |
|-----------|-------------|-----------|
| **Apache Superset** | `https://superset.apache.org/docs/intro` | BI corporativo (conector MySQL/ClickHouse) |
| Conectores de banco do Superset | `https://superset.apache.org/docs/configuration/databases` | Conexão com a réplica |
| **Metabase** | `https://www.metabase.com/docs/latest/` | BI de autoatendimento |
| Conector MySQL do Metabase | `https://www.metabase.com/docs/latest/databases/connections/mysql` | Conexão com a réplica |
| **Grafana** | `https://grafana.com/docs/grafana/latest/datasources/mysql/` | Painéis operacionais |
| Datasource ClickHouse do Grafana | `https://grafana.com/grafana/plugins/grafana-clickhouse-datasource/` | Integração com Plausible/PostHog |
| **Power BI** — conector MySQL | `https://learn.microsoft.com/power-query/connectors/mysqldatabase` | BI corporativo |
| Power BI — conector Web/REST | `https://learn.microsoft.com/power-query/connectors/web/web` | Consumo da Reporting API |
| Power BI — Gateway de dados local | `https://learn.microsoft.com/power-bi/connect-data/service-gateway-onprem` | Acesso a fonte interna |
| **Google Looker Studio** | `https://support.google.com/looker-studio/` | Painéis públicos |
| Looker Studio — conectores da comunidade | `https://developers.google.com/looker-studio/connector` | Conector para Matomo |
| **Qlik Sense** — conectores | `https://help.qlik.com/en-US/connectors/` | BI corporativo |
| Qlik REST Connector | `https://help.qlik.com/en-US/connectors/Subsystems/REST_connector_help/Content/Connectors_REST/Introduction/REST-connector.htm` | Consumo da Reporting API |
| **dbt** | `https://docs.getdbt.com/` | Transformação no data warehouse |
| **Apache Airflow** | `https://airflow.apache.org/docs/` | Orquestração de ETL |

---

## 22. Adoção no setor público

| Referência | URL | Aplicação |
|-----------|-----|-----------|
| **Europa Analytics** — serviço de analytics da Comissão Europeia | `https://commission.europa.eu/legal-notice/europa-analytics_en` | **Evidência primária** do precedente institucional (justificativa J5) |
| CNIL — medição de audiência e isenção de consentimento | `https://www.cnil.fr/fr/cookies-et-autres-traceurs/regles/cookies-solutions-pour-les-outils-de-mesure-daudience` | **Evidência primária** do precedente regulatório (justificativa J4) |
| CNIL — configuração recomendada para ferramentas de medição | `https://www.cnil.fr/fr/cookies-et-autres-traceurs/regles/cookies-solutions-pour-les-outils-de-mesure-daudience` | Parâmetros de configuração conforme |
| Catálogo de software livre da administração pública francesa (SILL) | `https://code.gouv.fr/sill/` | Adoção institucional do Matomo |
| Portal Software Público Brasileiro | `https://www.gov.br/governodigital/pt-br/software-publico/` | Referência de compartilhamento (Lei 14.129/2021, art. 16) |
| Padrão Digital de Governo — gov.br | `https://www.gov.br/ds/` | Referência de padrões digitais brasileiros |
| Guia de Governo Digital | `https://www.gov.br/governodigital/pt-br` | Contexto estratégico nacional |

---

## 23. Fontes secundárias

Fontes de nível 5 e 6, **explicitamente rotuladas**. Nenhuma sustenta sozinha uma conclusão do estudo.

| Fonte | Natureza | Uso neste estudo | Ressalva |
|-------|----------|------------------|----------|
| Comparativos publicados por fornecedores (ex.: "Matomo vs Google Analytics", "Piwik PRO vs Matomo") | Marketing | Identificação de dimensões de comparação a verificar em fonte primária | ⚠️ Parcial por natureza; nenhuma afirmação foi extraída sem verificação independente |
| Relatórios de analistas de mercado sobre analytics | Consultoria | Contexto de segmentação de mercado | ⚠️ Metodologia não pública; não utilizados para atribuição de notas |
| Levantamentos de participação de mercado de tecnologias web | Estatística de terceiro | Contexto de base instalada | ⚠️ Metodologia de amostragem limitada; não utilizados para conclusão |
| Estimativas de preço de plataformas enterprise (Adobe, GA360, Piwik PRO) | Mercado | Faixas de TCO | ⚠️ **Preços não são públicos.** Exigem cotação formal antes de qualquer uso em contratação |
| Discussões técnicas em repositórios (*issues*) | Comunidade | Identificação de limitações conhecidas a verificar na documentação | ⚠️ Não citadas como evidência isolada |

> **📌 Observação**
> A regra aplicada é: uma fonte secundária pode **indicar** onde procurar, mas nunca **substitui** a verificação em fonte primária. Toda limitação técnica atribuída a uma plataforma neste estudo foi confirmada na documentação oficial, no código-fonte ou em base de vulnerabilidades.

---

## 24. Registro de verificação

### 24.1 Controle de verificação de fontes

| Categoria | Quantidade de referências | Nível predominante | Data-base |
|-----------|--------------------------:|:------------------:|-----------|
| Normativos brasileiros | 11 | 1 | jul/2026 |
| Normativos internacionais | 7 | 1 | jul/2026 |
| Arcabouços de arquitetura | 10 | 3 | jul/2026 |
| Normas técnicas | 11 | 1 | jul/2026 |
| RFCs e especificações | 15 | 1 | jul/2026 |
| Documentação de plataformas | 11 conjuntos | 2 | jul/2026 |
| Licenças | 6 | 1 | jul/2026 |
| Segurança | 9 | 1/4 | jul/2026 |
| Infraestrutura | 18 | 2 | jul/2026 |
| BI e integração | 14 | 2 | jul/2026 |
| Setor público | 7 | 1/2 | jul/2026 |
| Secundárias | 5 categorias | 5/6 | jul/2026 |

### 24.2 Pendências de verificação antes da homologação

| # | Item a verificar | Responsável | Onda |
|---|-----------------|------------|:----:|
| V1 | Existência de decreto estadual de MS regulamentando a LGPD na Administração estadual | PGE + Arquitetura | 0 |
| V2 | Manifestações da ANPD posteriores à data-base sobre analytics no setor público | Encarregado de Dados | 0 |
| V3 | Preços vigentes dos plugins premium do Matomo | SETDIG | 3 |
| V4 | Cotação formal de Piwik PRO e Matomo Cloud (para as contingências) | SETDIG | Se acionada |
| V5 | CVEs publicados para Matomo, PHP e MySQL após a data-base | Segurança da Informação | 0 e contínuo |
| V6 | Captura em PDF das páginas críticas de licenciamento e privacidade | Arquitetura | 0 |
| V7 | Confirmação da vigência das URLs desta seção | Arquitetura | 0 |

> **⚠️ Bloco de Risco — Verificação é condição de homologação**
> As pendências **V1, V2, V5, V6 e V7** devem ser concluídas na Onda 0, antes da homologação do ADR-001. Especificamente:
>
> - **V2** e **V5** podem alterar conclusões do estudo, caso surja manifestação regulatória ou vulnerabilidade relevante após a data-base;
> - **V6** preserva a evidência de forma independente de alterações posteriores nos sites dos fornecedores, requisito para instrução de processo administrativo.

---

## Navegação

| ⬅️ Anterior | 🏠 Índice |
|------------|-----------|
| [11 — Riscos](11-riscos.md) | [README](../README.md) |
