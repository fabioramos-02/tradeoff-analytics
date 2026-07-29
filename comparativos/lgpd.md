# Comparativo — LGPD e proteção de dados pessoais

> **Corte transversal** · Base legal: Lei nº 13.709/2018 (LGPD), Lei nº 14.129/2021 (Governo Digital), Decreto nº 10.332/2020 (EGD)
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/07-matriz-decisao.md)

---

## Sumário

- [1. Fundamentos legais aplicáveis](#1-fundamentos-legais-aplicáveis)
- [2. Matriz sinóptica](#2-matriz-sinóptica)
- [3. Base legal para tratamento](#3-base-legal-para-tratamento)
- [4. Transferência internacional](#4-transferência-internacional)
- [5. Papéis LGPD (controlador × operador)](#5-papéis-lgpd-controlador--operador)
- [6. Direitos do titular](#6-direitos-do-titular)
- [7. DPIA (Relatório de Impacto)](#7-dpia-relatório-de-impacto)
- [8. Retenção e anonimização](#8-retenção-e-anonimização)
- [9. Precedentes regulatórios europeus (referência)](#9-precedentes-regulatórios-europeus-referência)
- [10. Recomendação](#10-recomendação)

---

## 1. Fundamentos legais aplicáveis

| Norma | Aplicação neste estudo |
|-------|-----------------------|
| **LGPD** (Lei 13.709/2018) | Regime central de tratamento de dado pessoal no Brasil |
| **Lei de Governo Digital** (14.129/2021) | Princípios de interoperabilidade, transparência ativa e soberania de dado |
| **EGD** — Estratégia de Governo Digital (Decreto 10.332/2020) | Diretrizes federais adaptáveis ao Estado |
| **Marco Civil da Internet** (12.965/2014) | Guarda de registros de acesso a aplicações (art. 15) |
| **Resolução CD/ANPD nº 2/2022** | Enquadramento de agentes de pequeno porte |
| **Guia ANPD sobre Cookies e Proteção de Dados** (out/2023) | Referência direta para banners e trackers |
| **Nota Técnica ANPD sobre transferência internacional** | Aplicável ao GA4, Clarity, Cloudflare e demais SaaS extraterritoriais |

---

## 2. Matriz sinóptica

| Plataforma | Modalidade | Residência do dado | Transferência internacional | Base legal viável | Consentimento obrigatório | DPA disponível | Adequação estimada |
|-----------|-----------|-------------------|:---------------------------:|:-----------------:|:-------------------------:|:--------------:|:------------------:|
| **Matomo** | On-Premise (custódia estatal) | Brasil | Nenhuma | Legítimo interesse; execução de política pública | ❌ (config. cookieless) | N/A | 🟢 Excelente |
| Matomo | Cloud UE | UE (Alemanha) | Sim, Brasil → UE | Legítimo interesse | 🟠 Recomendável | 🟢 Sim | 🟢 Boa |
| Plausible | Community Edition | Brasil | Nenhuma | Legítimo interesse | ❌ | N/A | 🟢 Excelente |
| Plausible | Cloud | UE | Sim, Brasil → UE | Legítimo interesse | 🟠 Recomendável | 🟢 Sim | 🟢 Boa |
| Umami | Auto-hospedado | Brasil | Nenhuma | Legítimo interesse | ❌ | N/A | 🟢 Excelente |
| Umami | Cloud | UE | Sim, Brasil → UE | Legítimo interesse | 🟠 Recomendável | 🟢 Sim | 🟢 Boa |
| PostHog | Auto-hospedado | Brasil | Nenhuma | Legítimo interesse | 🟠 Depende de recursos ativos (session replay) | N/A | 🟢 Boa |
| PostHog | Cloud EU | UE | Sim, Brasil → UE | Legítimo interesse (com ressalvas) | 🟠 Recomendável | 🟢 Sim | 🟠 Média |
| **GA4** | SaaS gratuito | EUA / múltiplas | Sim, Brasil → EUA | Consentimento | 🔴 Sim (obrigatório) | 🟢 Sim | 🔴 Difícil |
| Adobe Analytics | SaaS enterprise | Múltiplas (contratual) | Sim | Consentimento ou legítimo interesse (com ressalvas) | 🟠 Recomendável | 🟢 Sim | 🟠 Média |
| Simple Analytics | SaaS | UE (Amsterdam) | Sim, Brasil → UE | Legítimo interesse | ❌ (cookieless) | 🟢 Sim | 🟢 Boa |
| **Microsoft Clarity** | SaaS gratuito | Múltiplas (global) | Sim, Brasil → EUA/global | Consentimento | 🔴 Sim (obrigatório) | 🟠 Padrão | 🔴 Difícil |
| **Cloudflare Web Analytics** | SaaS gratuito | Múltiplas (global, PoPs) | Sim, Brasil → EUA/global | Legítimo interesse | ❌ (cookieless) | 🟠 Padrão | 🟠 Média |
| Open Web Analytics | Auto-hospedado | Brasil | Nenhuma | Legítimo interesse | ❌ | N/A | 🟢 Boa |

---

## 3. Base legal para tratamento

### 3.1 Bases aplicáveis (LGPD, art. 7º)

Para tratamento de dado pessoal em analytics de portal público, as bases legais praticáveis são:

| Base | Aplicabilidade em analytics | Observações |
|------|----------------------------|-------------|
| **Consentimento** (I) | Sempre aplicável — mas oneroso operacionalmente | Requer banner acessível, revogável, granular |
| **Cumprimento de obrigação legal** (II) | Trilha de auditoria e transparência (LAI, EGD) | Base sólida para métricas agregadas de acesso |
| **Execução de política pública** (III) | Base específica para administração pública | Adequada para monitoramento de serviço digital |
| **Legítimo interesse** (IX) | Aplicável para analytics não invasivo | Exige LIA — *Legitimate Interest Assessment* |
| **Estudos por órgão de pesquisa** (IV) | Não aplicável ao caso |
| **Execução de contrato** (V) | Não aplicável ao caso |

### 3.2 Regra prática por modalidade de plataforma

| Configuração da plataforma | Base recomendada |
|---------------------------|-----------------|
| Cookieless + IP anonimizado + sem fingerprint (Matomo/Plausible/Umami/Cloudflare/Simple corretamente configurados) | Legítimo interesse OU execução de política pública — **dispensa consentimento** |
| Cookies persistentes OU fingerprinting OU cross-site (GA4 padrão, Adobe, Clarity com session recording) | Consentimento obrigatório com banner |
| Session recording (Clarity, PostHog, Matomo com plugin) | Consentimento explícito e opt-in, mesmo que a plataforma esteja em custódia estatal |

> **📌 Observação — precedente CNIL (França) para o Matomo**
> A autoridade francesa de proteção de dados (CNIL) publicou orientação declarando que **Matomo pode operar sem banner de consentimento** quando configurado sob determinados parâmetros (cookieless, IP anonimizado, sem cross-domain, sem plugin invasivo). Essa configuração é chamada de "*Matomo exemption*". Não existe precedente equivalente formal no Brasil, mas o guia da ANPD sobre cookies (out/2023) admite raciocínio análogo.

---

## 4. Transferência internacional

### 4.1 Regime da LGPD (arts. 33 a 36)

Transferência internacional de dado pessoal exige uma das hipóteses do art. 33 — a mais praticável para analytics é a **cláusula contratual padrão / DPA** (art. 33, II c).

### 4.2 Mapa de risco

| Plataforma | Destino | Risco de transferência | Mitigação |
|-----------|--------|----------------------|-----------|
| Matomo On-Premise | — | ❌ Nulo | Não aplicável |
| Matomo Cloud | Alemanha (UE) | 🟢 Baixo | DPA + SCC-equivalente |
| Plausible Community | — | ❌ Nulo | Não aplicável |
| Plausible Cloud | Alemanha (UE) | 🟢 Baixo | DPA + SCC-equivalente |
| Umami Cloud | EUA (padrão) | 🟠 Médio | DPA — mas jurisdição EUA sob CLOUD Act |
| PostHog Cloud EU | Alemanha (UE) | 🟢 Baixo | DPA |
| PostHog Cloud US | EUA | 🔴 Alto | DPA + análise de CLOUD Act |
| GA4 | EUA (Google) | 🔴 Alto | DPA + Data Processing Amendment + configuração *server-side* + IP anonimizado |
| Adobe Analytics | Múltiplas (contratual) | 🟠 Médio | Cláusula de residência específica em contrato enterprise |
| Simple Analytics | Amsterdam (UE) | 🟢 Baixo | DPA |
| Microsoft Clarity | Global | 🔴 Alto | Termos padrão insuficientes para governo com dado sensível |
| Cloudflare Web Analytics | Global (edge) | 🔴 Alto | Termos padrão — sem residência garantida no gratuito |

### 4.3 CLOUD Act (EUA) e a jurisdição do operador

Empresas constituídas nos EUA (Google, Microsoft, Cloudflare, PostHog US) estão sujeitas ao *Clarifying Lawful Overseas Use of Data Act* (2018). Isso significa que autoridades federais americanas podem, sob determinadas condições, requisitar dados armazenados por essas empresas **mesmo fora dos EUA**. Para dado pessoal de cidadão brasileiro tratado em portal público, isso constitui **risco de soberania**.

> **🚨 Alerta — jurisdição do operador é um risco arquitetural**
> A residência técnica do dado (por exemplo, "servidor em Frankfurt") **não** anula o risco jurisdicional se o operador for empresa americana. Somente auto-hospedagem em infraestrutura estatal elimina esse vetor.

---

## 5. Papéis LGPD (controlador × operador)

Em qualquer modalidade SaaS, o Estado é **controlador** (define finalidade e meios) e o fornecedor é **operador**. Isso implica:

- Contrato formal com cláusula de operador (LGPD art. 39).
- DPA (Data Processing Addendum) assinado.
- Subcontratações (sub-operadores) declaradas.
- Comunicação de incidente pelo operador ao controlador em prazo contratual.

**Modalidade auto-hospedada:** o Estado é controlador e o próprio Estado é o operador (autotratamento). Isso simplifica drasticamente a governança contratual.

---

## 6. Direitos do titular

Aplicação prática dos direitos do titular (LGPD art. 18) em analytics:

| Direito | Como atender em Matomo | Como atender em GA4 |
|---------|-------------------------|---------------------|
| Confirmação e acesso | SQL direto no MariaDB / API `UsersManager.getUsers` | Solicitação via portal Google — resposta em prazo |
| Correção | Não aplicável em geral (dado agregado) | Não aplicável em geral |
| Anonimização, bloqueio ou eliminação (dados desnecessários ou excessivos) | Configuração `anonymize_ip=full`; `RawData purge` | Configuração de retenção reduzida no console |
| Portabilidade | Export API + acesso direto ao banco | Data Export para BigQuery (limitado) |
| Eliminação de dados tratados com consentimento | `RawData purge` | *User deletion API* |
| Informação sobre operadores e sub-operadores | Não há (Estado é operador) | Documentação Google |
| Revogação do consentimento | Não aplicável (base ≠ consentimento) | Banner com opt-out |

**Vantagem estrutural do auto-hospedado:** os direitos são atendidos com **acesso direto ao dado**, sem depender de janela SLA do fornecedor.

---

## 7. DPIA (Relatório de Impacto)

### 7.1 Quando é obrigatório

A ANPD pode exigir DPIA (RIPD) sempre que o tratamento gerar risco elevado — em analytics de portal público, os gatilhos comuns são:

- Uso de session recording (captura de tela do usuário).
- Cross-site tracking / *fingerprinting*.
- Uso de IA sobre dado pessoal (Copilot do Clarity, PostHog LLM Observability).
- Tratamento em grande escala (portal com alto tráfego).
- Transferência internacional para país sem adequação.

### 7.2 Esforço por plataforma

| Plataforma | Esforço DPIA | Elementos-chave |
|-----------|:------------:|-----------------|
| Matomo On-Premise cookieless | 🟢 Baixo | Configuração de anonimização + política de retenção |
| Plausible CE | 🟢 Baixo | Similar |
| Umami / Simple Analytics | 🟢 Baixo | Similar |
| PostHog OSS sem session replay | 🟠 Médio | Depende dos recursos ativados |
| PostHog OSS com session replay | 🔴 Alto | Consentimento explícito + análise de eventos capturados |
| Matomo Cloud UE | 🟠 Médio | Transferência internacional + análise de sub-operadores |
| GA4 | 🔴 Alto | Precedente adverso europeu; análise de fingerprinting; CLOUD Act |
| Microsoft Clarity | 🔴 Alto | Session recording por default; transferência internacional; retenção fixa |
| Adobe Analytics | 🔴 Alto | Escopo funcional amplo + configurações padrão invasivas |
| Cloudflare Web Analytics | 🟠 Médio | Cookieless mitiga, mas jurisdição EUA obriga análise |

---

## 8. Retenção e anonimização

| Plataforma | Retenção configurável | Anonimização de IP nativa | Purga programada |
|-----------|:----------------------:|:-------------------------:|:----------------:|
| Matomo | 🟢 Total | 🟢 Nativa (`anonymize_ip`) | 🟢 (`core:archive` + `RawData purge`) |
| Plausible | 🟢 Total | 🟢 Nativa (hash + descarte) | 🟢 |
| Umami | 🟢 Total | 🟢 Nativa | 🟢 |
| PostHog | 🟢 Total | 🟠 Configurável | 🟢 |
| GA4 | 🟠 Máx. 14 meses (gratuito) | 🟠 Configurável (`_anonymizeIp`) | 🟠 Automática |
| Adobe Analytics | 🟢 Configurável (contratual) | 🟢 | 🟢 |
| Simple Analytics | 🟢 Total | 🟢 Nativa (não persistência) | 🟢 |
| Microsoft Clarity | 🔴 30 dias fixos | 🟢 IP nunca persistido | Automática (não configurável) |
| Cloudflare | 🔴 6 meses fixos | 🟢 IP hasheado | Automática |

**Regra ANPD:** dado pessoal deve ser eliminado após o fim do tratamento, salvo hipóteses legais de guarda (LGPD art. 16). Retenções **não configuráveis** dificultam o cumprimento formal desse requisito.

---

## 9. Precedentes regulatórios europeus (referência)

Embora não vinculantes no Brasil, precedentes europeus sobre GDPR são citados pela ANPD como referência interpretativa:

| Precedente | Ano | Plataforma | Decisão |
|-----------|:---:|-----------|---------|
| DPA Áustria (`noyb` × Google) | 2022 | GA (Universal) | Uso em desconformidade com GDPR sem *server-side proxy* |
| CNIL (França) | 2022 | GA (Universal) | Uso em desconformidade — recomendação de alternativas |
| Garante (Itália) | 2022 | GA (Universal) | Uso em desconformidade |
| CNIL (França) | 2020 | Matomo | Isenção de consentimento para configuração privacy-preserving |
| DPA Dinamarca | 2022 | Google Workspace / Chromebook | Uso escolar em desconformidade — regressão para alternativas |

Fonte primária: [`docs/12-referencias.md`](../docs/12-referencias.md).

> **📌 Observação — GA4 mitigado com server-side tagging**
> Após 2023, a implementação de *server-side tagging* no Google Tag Manager tornou o GA4 defensável em muitos casos europeus, ao mediar o tráfego do navegador para o Google via infraestrutura própria do controlador. Essa mitigação **é possível**, mas eleva significativamente o custo de operação e não elimina o vínculo com a jurisdição do operador.

---

## 10. Recomendação

> **✅ Bloco de Decisão — perfil LGPD-preferencial**
>
> Priorizar plataformas com:
>
> 1. **Auto-hospedagem em infraestrutura estatal** — elimina transferência internacional.
> 2. **Configuração cookieless por padrão** — permite operar sob **legítimo interesse** dispensando consentimento explícito.
> 3. **Retenção configurável** — cumprimento formal do princípio de necessidade (LGPD art. 6º).
> 4. **DPA e RBAC granular** quando SaaS — mas apenas como opção secundária.
>
> Plataformas que **cumprem** simultaneamente os quatro critérios: **Matomo On-Premise**, **Plausible Community Edition**, **Umami auto-hospedado**, **PostHog auto-hospedado** (com session replay desativado ou consentido).
>
> Plataformas que **falham** em ao menos dois critérios (não recomendadas como primárias): **GA4**, **Microsoft Clarity**, **Cloudflare Web Analytics**, **PostHog Cloud US**, **Adobe Analytics** (padrão).

### 10.1 Checklist mínimo LGPD para qualquer plataforma escolhida

- [ ] Anonimização de IP configurada (`anonymize_ip=full` no Matomo; equivalente nas demais).
- [ ] Retenção definida por política formal do órgão (padrão sugerido: 24 meses, agregado; 6 meses, granular).
- [ ] Base legal declarada em Registro de Operações de Tratamento (ROPA).
- [ ] Banner de consentimento apenas onde exigido pela base legal escolhida.
- [ ] Procedimento operacional para atendimento de direitos do titular.
- [ ] DPO da SETDIG ciente e formalmente responsável pela plataforma no ROPA.
- [ ] DPIA elaborado e revisto anualmente.
- [ ] Cláusula contratual completa quando houver operador externo.
- [ ] Registro de sub-operadores (para SaaS).
- [ ] Procedimento formal para comunicação de incidente à ANPD.

Fundamentação normativa consolidada: [`docs/12-referencias.md`](../docs/12-referencias.md).
Análise de trade-offs: [`docs/06-tradeoffs.md`](../docs/06-tradeoffs.md).
