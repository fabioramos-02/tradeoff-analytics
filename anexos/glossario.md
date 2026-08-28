# Anexo — Glossário

> **Anexo estrutural** · Definição de termos técnicos, siglas e jargão do estudo
> [← Voltar ao índice](../README.md)

---

## Sumário

- [1. Como usar](#1-como-usar)
- [2. Termos de arquitetura corporativa](#2-termos-de-arquitetura-corporativa)
- [3. Termos de Web Analytics](#3-termos-de-web-analytics)
- [4. Termos de infraestrutura e engenharia](#4-termos-de-infraestrutura-e-engenharia)
- [5. Termos jurídicos e regulatórios](#5-termos-jurídicos-e-regulatórios)
- [6. Termos de licenciamento e negócios](#6-termos-de-licenciamento-e-negócios)
- [7. Siglas](#7-siglas)

---

## 1. Como usar

Consulte por ordem alfabética dentro de cada seção. Referências cruzadas em `[[colchetes]]` indicam o termo correspondente em outra seção deste glossário. Fontes normativas relevantes: [`docs/12-referencias.md`](../docs/99-referencias.md).

---

## 2. Termos de arquitetura corporativa

### ADR — Architecture Decision Record

Registro formal, versionado e datado de uma decisão arquitetural relevante, com contexto, alternativas consideradas, decisão tomada e consequências. Formato Nygard (2011) ou MADR 4.0. Neste estudo: [`docs/08-adr.md`](../anexos/historico/docs/adr-001.md).

### Architectural driver

Fator (funcional, não-funcional, restritivo ou de negócio) que **conduz** a definição de uma arquitetura. Categorias comuns: qualidade requerida, restrições técnicas, restrições organizacionais, princípios corporativos.

### ATAM — Architecture Trade-off Analysis Method

Método desenvolvido pelo Software Engineering Institute (SEI/CMU) para avaliar arquiteturas em relação a atributos de qualidade concorrentes. Produz uma **árvore de utilidade**, identifica **pontos de sensibilidade** e **trade-off points**.

### Fitness function

Métrica quantitativa que avalia se uma arquitetura atende a um atributo de qualidade específico. Ex.: "arquivamento < 15 min" é uma fitness function para o atributo *timeliness*.

### MADR — Markdown ADR

Template padronizado para ADR em Markdown, mantido pela comunidade em `adr.github.io`. Versão 4.0 é a de referência neste estudo.

### MAUT — Multi-Attribute Utility Theory

Fundamentação matemática de decisão multicritério. Consiste em atribuir notas a cada critério, ponderar por peso e agregar linearmente. Neste estudo: [`docs/07-matriz-decisao.md`](../docs/05-matriz.md).

### Quality Attribute Utility Tree

Árvore hierárquica de atributos de qualidade (segurança, desempenho, disponibilidade, manutenibilidade, etc.) refinados até se transformarem em **cenários mensuráveis**.

### Sensitivity point

Ponto do sistema em que uma pequena mudança em uma decisão arquitetural produz **grande impacto** em algum atributo de qualidade.

### TOGAF — The Open Group Architecture Framework

Framework de arquitetura corporativa, cuja versão 10 (2022) é a referência. ADM (Architecture Development Method) é o processo iterativo em fases (A a H) descrito no framework.

### Trade-off point

Ponto do sistema em que uma decisão arquitetural afeta **múltiplos** atributos de qualidade simultaneamente, alguns positivamente e outros negativamente.

---

## 3. Termos de Web Analytics

### A/B testing

Experimentação com dois ou mais grupos aleatorizados. Compara variantes de uma página/fluxo para medir efeito de uma mudança sobre uma métrica-alvo. Requer amostragem estatisticamente significativa.

### Beacon

Requisição HTTP disparada pelo navegador (ou app) para o servidor de analytics, transportando um evento (page view, evento customizado, session ping). Nome vem do formato clássico de "pixel de rastreamento" (1×1 GIF).

### Cohort

Grupo de usuários que compartilham uma característica ou evento comum em um mesmo intervalo de tempo. Ex.: "cidadãos que se cadastraram em julho/2026".

### Conversion funnel (funil de conversão)

Sequência ordenada de etapas até uma conversão-alvo. Métrica principal: taxa de completude entre etapas.

### Cookieless analytics

Modelo de analytics que **não persiste cookies** no navegador. Identifica visitantes por hash volátil (IP + user-agent + salt diário), descartado após o cálculo. Base para modelos privacy-first.

### Custom dimension

Metadado extra anexado a um evento ou visitante, definido pelo próprio operador. Ex.: "tipo de serviço", "unidade organizacional".

### DAU / MAU

Daily Active Users / Monthly Active Users — contadores de usuários únicos ativos em uma janela.

### Event (evento)

Ação discreta do usuário registrada pela plataforma (clique, envio de formulário, tempo em vídeo). Modelo dominante em analytics moderna.

### First-party / Third-party (cookies e dado)

*First-party*: dado ou cookie do próprio domínio visitado. *Third-party*: dado ou cookie de um domínio diferente (típico de trackers cross-site). Regulação recente restringe *third-party*.

### Fingerprinting

Técnica de identificação passiva do dispositivo/navegador por combinação de atributos (fonte, resolução, plugins). Considerada tratamento de dado pessoal pela ANPD/CNIL/DPAs europeias.

### Goal (meta)

Métrica-alvo formalizada na plataforma (ex.: "envio de formulário", "download de documento"). Base para funis e taxa de conversão.

### Heatmap

Visualização agregada da atividade do usuário na página (cliques, scroll, mouse). Cores mostram intensidade.

### LCP / CLS / INP — Core Web Vitals

Métricas do Google Web.dev que avaliam experiência de usuário:
- **LCP** (Largest Contentful Paint) — tempo até o maior elemento visível.
- **CLS** (Cumulative Layout Shift) — instabilidade visual.
- **INP** (Interaction to Next Paint) — latência de interação.

### Page view

Um carregamento de página, disparando o evento padrão da plataforma.

### Real-time analytics

Ingestão e visualização de eventos com latência de segundos ou poucos minutos.

### Retention (retenção)

Métrica de fidelidade: porcentagem de usuários de um cohort que retorna em janelas futuras.

### Session recording

Reconstrução visual da sessão do usuário via captura de eventos DOM. Considera-se tratamento de dado potencialmente sensível — sujeita a consentimento explícito.

### Server-side tagging

Mediar a chamada do beacon do navegador para o serviço de analytics **via infraestrutura do próprio controlador** (um GTM server-side, por exemplo), em vez do envio direto para o operador estrangeiro. Reduz *fingerprinting* e ajusta o dado antes do envio.

### Tag (tracking tag)

Trecho de código JavaScript embarcado no site que dispara beacons a cada evento.

### Tag Manager

Ferramenta para gerenciar múltiplas tags sem alterar o código do site (Google Tag Manager, Matomo Tag Manager). Cliente ou server-side.

### User journey

Sequência de páginas ou eventos percorridos pelo usuário até uma conversão ou abandono.

---

## 4. Termos de infraestrutura e engenharia

### Archiving (Matomo)

Rotina periódica de agregação de eventos brutos em relatórios pré-calculados. Executada pelo comando `console core:archive`. Determinante para o desempenho do Matomo em alto volume.

### CDN — Content Delivery Network

Rede de servidores de borda geograficamente distribuída, servindo conteúdo estático próximo ao usuário. Cloudflare, Akamai, CloudFront.

### ClickHouse

Banco de dados colunar OLAP open-source, otimizado para consultas analíticas sobre grandes volumes. Utilizado por Plausible, PostHog e Matomo Cloud.

### DPA — Data Processing Addendum

Adendo contratual entre controlador e operador estabelecendo termos do tratamento de dado pessoal.

### Helm chart

Pacote de implantação para Kubernetes que agrupa manifests, valores e templates.

### HA — High Availability

Arquitetura de deployment resistente a falha de um componente individual, tipicamente via múltiplas réplicas atrás de load balancer.

### Ingest / ingestion

Recebimento e persistência inicial de eventos pela plataforma.

### Kafka

Sistema de streaming distribuído open-source (Apache). Utilizado como buffer de ingestão em plataformas de alta escala (PostHog).

### K8s — Kubernetes

Orquestrador de containers de referência. Runtime primário do parque SETDIG.

### PoP — Point of Presence

Localização geográfica onde um provedor de rede (CDN, ISP, cloud) mantém infraestrutura.

### RPO / RTO

*Recovery Point Objective* — quanto dado pode ser perdido (janela). *Recovery Time Objective* — quanto tempo pode ficar indisponível.

### SLA — Service Level Agreement

Nível de serviço contratado, tipicamente disponibilidade (ex.: 99,9 %/ano) e penalidades por descumprimento.

### SLO / SLI

*Service Level Objective* (objetivo interno) e *Service Level Indicator* (métrica que mede). Não são contratuais como o SLA — são internos.

---

## 5. Termos jurídicos e regulatórios

### ANPD — Autoridade Nacional de Proteção de Dados

Órgão regulador do tratamento de dado pessoal no Brasil, criado pela LGPD. Emite normativos, guias e aplica sanções.

### Base legal

Fundamento previsto na LGPD (art. 7º ou art. 11 para dado sensível) que autoriza o tratamento. Base incorreta invalida o tratamento.

### CLOUD Act

*Clarifying Lawful Overseas Use of Data Act* — lei federal americana (2018) que autoriza requisição extraterritorial de dado por empresa constituída nos EUA. Aplica-se a Google, Microsoft, Cloudflare, PostHog Cloud US, entre outras.

### Consentimento (LGPD)

Base legal específica: manifestação livre, informada e inequívoca do titular. Deve ser específica (por finalidade) e revogável a qualquer momento.

### Controlador

Agente que decide **a finalidade** e **os meios** do tratamento de dado pessoal. No caso de portal público estadual: o próprio Estado (via órgão titular do portal).

### DPIA / RIPD — Data Protection Impact Assessment

Relatório de Impacto à Proteção de Dados. Documento formal exigido pela LGPD quando o tratamento apresenta risco elevado.

### DPO — Data Protection Officer

Encarregado de proteção de dado pessoal. Papel obrigatório para o controlador (LGPD art. 41).

### Fingerprinting

Tratado por autoridades europeias como equivalente a cookie *third-party* para fins de consentimento — mesmo sem persistência de cookie, é dado pessoal se identificar o titular.

### GDPR — General Data Protection Regulation

Regulamento europeu (2018) equivalente à LGPD em muitos pontos. Precedentes GDPR são citados pela ANPD como referência interpretativa.

### Legítimo interesse

Base legal (LGPD art. 7º, IX) que autoriza tratamento sem consentimento quando o interesse do controlador (ou terceiro) prevalece sobre os direitos do titular. Exige avaliação (LIA — *Legitimate Interest Assessment*).

### LGPD — Lei Geral de Proteção de Dados

Lei nº 13.709/2018. Regime central de proteção de dado pessoal no Brasil.

### Operador

Agente que trata dado pessoal em nome do controlador. No caso SaaS: o fornecedor.

### ROPA — Registro de Operações de Tratamento

Documento formal exigido pela LGPD (art. 37) descrevendo cada tratamento realizado pelo controlador.

### Titular

Pessoa natural a quem se referem os dados. Neste estudo: o cidadão do Estado.

### Transferência internacional

Envio de dado pessoal para outro país. Regulada pelos arts. 33 a 36 da LGPD.

---

## 6. Termos de licenciamento e negócios

### AGPL — Affero General Public License

Licença copyleft (GPL) estendida ao caso de software oferecido como serviço via rede. Software AGPL usado como SaaS obriga o operador a publicar o código-fonte de suas modificações. Adotada por Plausible.

### Copyleft

Cláusula em licença de software que **obriga** obras derivadas a manter a mesma licença. Contrária a licenças permissivas (MIT, Apache, BSD).

### GPL — GNU General Public License

Licença copyleft clássica. Versão 3 (2007) é a de referência. Adotada por Matomo.

### MIT (License)

Licença permissiva minimalista. Permite uso, modificação, redistribuição e comercialização, inclusive em software proprietário. Adotada por Umami, PostHog (core), clarity-js.

### Open core

Modelo de negócio: núcleo do produto sob licença open-source, complementos ("enterprise edition") sob licença proprietária. Adotado por PostHog, GitLab, Elastic (pré-2021).

### SaaS — Software as a Service

Software oferecido como serviço, tipicamente por assinatura, sem instalação pelo cliente.

### TCO — Total Cost of Ownership

Custo total de posse. Soma de licenciamento, infraestrutura, equipe, adequação regulatória, riscos e operação ao longo do horizonte considerado.

### Vendor lock-in

Situação em que o cliente **não consegue** migrar de fornecedor sem custo desproporcional (por dependência técnica, contratual ou de formato de dado).

---

## 7. Siglas

| Sigla | Significado |
|-------|-------------|
| ADM | Architecture Development Method (TOGAF) |
| ADR | Architecture Decision Record |
| AGPL | Affero General Public License |
| ANPD | Autoridade Nacional de Proteção de Dados |
| API | Application Programming Interface |
| ATAM | Architecture Trade-off Analysis Method |
| BI | Business Intelligence |
| CE | Community Edition |
| CDN | Content Delivery Network |
| CH | ClickHouse |
| CLS | Cumulative Layout Shift |
| CNIL | Commission Nationale de l'Informatique et des Libertés (França) |
| CPU | Central Processing Unit |
| CSV | Comma-Separated Values |
| DPA | Data Processing Addendum |
| DPIA | Data Protection Impact Assessment |
| DPO | Data Protection Officer |
| DR | Disaster Recovery |
| DW | Data Warehouse |
| EE | Enterprise Edition |
| EGD | Estratégia de Governo Digital |
| ETL | Extract, Transform, Load |
| FTE | Full-Time Equivalent |
| GA | Google Analytics |
| GA4 | Google Analytics 4 |
| GCP | Google Cloud Platform |
| GDPR | General Data Protection Regulation |
| GPL | GNU General Public License |
| GTM | Google Tag Manager |
| HA | High Availability |
| HPA | Horizontal Pod Autoscaler |
| IdP | Identity Provider |
| INP | Interaction to Next Paint |
| ISO | International Organization for Standardization |
| JSON | JavaScript Object Notation |
| JWT | JSON Web Token |
| K8s | Kubernetes |
| LAI | Lei de Acesso à Informação (12.527/2011) |
| LB | Load Balancer |
| LCP | Largest Contentful Paint |
| LGPD | Lei Geral de Proteção de Dados |
| LIA | Legitimate Interest Assessment |
| MADR | Markdown Architecture Decision Record |
| MAUT | Multi-Attribute Utility Theory |
| OIDC | OpenID Connect |
| OP | On-Premise |
| OSS | Open-Source Software |
| OWA | Open Web Analytics |
| OWASP | Open Web Application Security Project |
| PDCA | Plan-Do-Check-Act |
| POC | Proof of Concept |
| PoP | Point of Presence |
| PV | Page View |
| RBAC | Role-Based Access Control |
| REST | Representational State Transfer |
| RIPD | Relatório de Impacto à Proteção de Dados |
| ROPA | Registro de Operações de Tratamento |
| RPO | Recovery Point Objective |
| RTO | Recovery Time Objective |
| SaaS | Software as a Service |
| SAML | Security Assertion Markup Language |
| SCC | Standard Contractual Clauses |
| SDK | Software Development Kit |
| SETDIG | Secretaria-Executiva de Transformação Digital |
| SEGOV | Secretaria de Estado de Governo e Gestão Estratégica |
| SEI/CMU | Software Engineering Institute / Carnegie Mellon University |
| SGD | Superintendência de Governo Digital |
| SIEM | Security Information and Event Management |
| SLA | Service Level Agreement |
| SLI | Service Level Indicator |
| SLO | Service Level Objective |
| SQL | Structured Query Language |
| SSO | Single Sign-On |
| STI | Superintendência de Tecnologia da Informação |
| TCO | Total Cost of Ownership |
| TLS | Transport Layer Security |
| TOGAF | The Open Group Architecture Framework |
| UE | União Europeia |
| WAF | Web Application Firewall |
| YAGNI | You Aren't Gonna Need It |

---

Referências completas para termos aqui listados: [`docs/12-referencias.md`](../docs/99-referencias.md).
