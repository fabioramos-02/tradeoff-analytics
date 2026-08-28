# CLAUDE.md — Instruções do repositório

Este arquivo orienta o Claude Code (e outros agentes de IA) sobre como colaborar neste repositório.

---

## 1. O que é este repositório

Estudo técnico de arquitetura para seleção de plataforma de **Web Analytics** para o Governo do Estado de Mato Grosso do Sul, conduzido pela **Secretaria-Executiva de Transformação Digital — SETDIG**.

- Não é código de aplicação. É **documentação técnica em Markdown**, curta, macro-estratégica, formal onde precisa (TOGAF + ATAM + ADR + MAUT) e didática onde importa (para gestor decidir em 15 minutos).
- Público-alvo: Comitê de Arquitetura, gestores públicos, times técnicos das Secretarias.
- Publicação-alvo: GitHub, Azure DevOps Wiki, MkDocs Material — sem reescrita.

### 1.1 Situação técnica de origem

- O Estado usa **Matomo On-Premise** hoje para monitorar portal institucional e sites do governo. **Não houve estudo prévio** de alternativas na implantação — decisão herdada.
- O portal institucional migrará para o **novo portal Xvia** (superapp cidadão). O líder técnico do Xvia informou que a stack de Web Analytics **on-premise gratuita** adotada é **PostHog**.
- Pergunta central deste estudo: **manter Matomo, substituir por PostHog, ou operar os dois em coexistência?** Antes de bater martelo, benchmark documental das ferramentas **on-premise gratuitas** que rodam dentro da infra do Estado.
- Escopo fechado deste ciclo: **Matomo, PostHog, Plausible, Umami**. Alternativas SaaS ou proprietárias saíram do escopo (arquivadas em `anexos/historico/`).
- Direção estratégica: coexistência de curto prazo (Matomo parque + PostHog Xvia) → Matomo como camada de consulta histórica → convergência em uma única ferramenta a médio prazo.

---

## 2. Estrutura obrigatória

```
analytics-platform-study/
├── README.md                       # índice executivo (curto)
├── CLAUDE.md                       # este arquivo
├── docs/                           # núcleo do estudo (01 a 08 + 99-referencias)
├── plataformas/                    # 4 fichas: matomo, posthog, plausible, umami
├── comparativos/                   # 3 cortes: matomo-vs-posthog, on-premise-gratis, coexistencia
├── anexos/
│   ├── matriz.md
│   ├── benchmark.md
│   ├── glossario.md
│   └── historico/                  # material arquivado (fora do nav MkDocs)
└── apresentacoes/                  # Marp + slidão HTML standalone
```

Índice canônico e status de cada arquivo: [`README.md`](README.md) §Índice.

---

## 3. Convenções editoriais (obrigatórias)

| Regra | Detalhe |
|-------|---------|
| Idioma | Português do Brasil (pt-BR), com acentuação completa |
| Nome oficial | Sempre "Secretaria-Executiva de Transformação Digital — SETDIG". Nunca "Governo Digital" |
| Tom | Macro-estratégico, didático, curto. Prosa densa e fragmentada. Se cabe em tabela, vira tabela. Se cabe em bullet, vira bullet. |
| Blocos semânticos | `> **📌 Observação**`, `> **✅ Bloco de Decisão**`, `> **⚠️ Bloco de Risco**`, `> **🚨 Alerta**`, `> **🔍 Evidência**` — em citação, nunca `!!! note` / `> [!NOTE]` |
| Links | Sempre **relativos**, para funcionar em GitHub + Wiki + MkDocs |
| Diagramas | Mermaid dentro de fenced code blocks ` ```mermaid ` |
| Notas | Escala inteira 1–5. Sem meia-nota. Justificativa textual obrigatória |
| Sumário | Todo documento tem seu próprio `## Sumário` no topo |
| Frontmatter | Cabeçalho padrão com pontuação, situação, e link `[← Voltar ao índice]` |
| Anti-inflação | Se um fato já está em tabela ou matriz, **linka**, não reescreve |

---

## 4. Regra de evidência

> **📌 Observação**
> Toda afirmação técnica deve ser rastreável a **fonte primária**: documentação oficial do fornecedor, código-fonte, licença, especificação de API, norma técnica ou instrumento normativo. Blogs, artigos de opinião e marketing **não** são fonte primária — se citados, rotular explicitamente como fonte secundária.

- Fontes agregadas em [`docs/99-referencias.md`](docs/99-referencias.md).
- Ao adicionar afirmação técnica nova, adicione a fonte no mesmo commit.

---

## 5. Ao editar / criar conteúdo

### 5.1 Antes de escrever

1. Ler o arquivo relacionado em `docs/` para não contradizer decisões já tomadas (ADR-002 vigente, matriz, roadmap).
2. Ler pelo menos uma ficha de plataforma existente (`plataformas/plausible.md` é referência estrutural) para preservar o padrão de 10 seções.
3. Se a mudança afeta a matriz ([`docs/05-matriz.md`](docs/05-matriz.md)), atualizar também [`anexos/matriz.md`](anexos/matriz.md) e o Resumo Executivo do [`README.md`](README.md) no mesmo commit.

### 5.2 Fichas de plataforma (`plataformas/*.md`)

Seções obrigatórias (10, nesta ordem):

1. Visão geral (identificação, licença, comunidade, modelo — 1 parágrafo)
2. Licença e modelo de negócio
3. Hospedagem on-premise (stack, requisitos mínimos, complexidade)
4. Funcionalidades essenciais (tabela sim/não/pago)
5. APIs e integrações (BI + GTM + IdP em uma seção)
6. Segurança e LGPD
7. Custos (TCO 5 anos — uma tabela)
8. Pontos fortes / pontos fracos (bullet list)
9. Quando usar / quando evitar
10. Notas do avaliador (opcional)

Meta de tamanho por ficha: **250–400 linhas**. Se passar, cortou pouco.

Cabeçalho padronizado:

```markdown
# Nome da Plataforma

> **Ficha técnica de plataforma** · Pontuação na matriz: **XXX/600 (XX,X %)** — posição · status
> **Situação:** ✅/⚠️/🚨 <descrição curta do papel na arquitetura recomendada>
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/05-matriz.md)
```

### 5.3 Comparativos (`comparativos/*.md`)

Três cortes canônicos:

- `matomo-vs-posthog.md` — comparativo binário direto (incumbente × entrante Xvia)
- `on-premise-gratis.md` — quatro plataformas do escopo × dimensões estratégicas (LGPD, custo, complexidade, cobertura funcional)
- `coexistencia-matomo-posthog.md` — cenário híbrido (papéis complementares, governança)

Regra: **não repetir** o que já está na ficha da plataforma nem na matriz. Tabela sinóptica no topo + análise curta abaixo.

### 5.4 Anexos (`anexos/*.md`)

- `matriz.md` — matriz completa em Markdown, exportável para CSV/XLSX. Cada linha (plataforma × critério) tem nota, peso e produto.
- `benchmark.md` — metodologia + cenários de carga + resultados (podem ser projeções fundamentadas, marcadas como tal).
- `glossario.md` — termo, sigla, definição, referência cruzada.
- `historico/` — material arquivado (ADR-001 deprecado, comparativos redundantes, fichas de plataformas fora do escopo atual). **Fora do nav do MkDocs**, mas preservado no git para rastreabilidade.

---

## 6. Metodologia sob a qual o estudo opera

| Arcabouço | Uso | Onde |
|-----------|-----|------|
| TOGAF 10 ADM (fases A e B) | Contexto + requisitos | `docs/01`, `docs/02` |
| ATAM (SEI/CMU) | Árvore de atributos de qualidade + trade-offs | `docs/03`, `docs/05` |
| Gartner Decision Framework | Critérios ponderados, must-have vs nice-to-have | `docs/03`, `docs/04` |
| MAUT | Normalização + agregação da matriz | `docs/05`, `anexos/matriz.md` |
| ADR (Nygard / MADR 4.0) | Registro formal da decisão | `docs/06-adr-002.md` |

Não introduzir metodologia nova sem justificar em `docs/03`.

---

## 7. O que NÃO fazer

- ❌ Nunca substituir "Secretaria-Executiva de Transformação Digital" por "Governo Digital" ou variações.
- ❌ Nunca ASCII-fiar acentos (não → nao, decisão → decisao). pt-BR completo, sempre.
- ❌ Nunca usar emojis fora dos blocos semânticos canônicos.
- ❌ Nunca inserir assinatura de IA ("Co-Authored-By: Claude", "Generated by...") em commits, PRs ou documentos.
- ❌ Nunca usar sintaxe de admonition proprietária (`!!! note`, `> [!NOTE]`). Só blocos em citação padronizados.
- ❌ Nunca inventar preço, benchmark ou pontuação. Todo número tem fonte ou é declarado como projeção fundamentada.
- ❌ Nunca alterar peso de critério na matriz sem revisar o ADR e o `README.md` no mesmo commit.
- ❌ Nunca "endossar" ou "invalidar" a escolha atual do Matomo. A avaliação é neutra em relação ao status quo.
- ❌ Nunca reintroduzir duplicação entre matriz × comparativos × fichas. Se um fato precisa aparecer em dois lugares, **linka**. Se está em tabela, **não vira parágrafo em prosa**.
- ❌ Nunca reintroduzir plataformas fora do escopo atual (GA4, Adobe, Clarity, Cloudflare, Simple Analytics, OWA) sem revisão explícita do escopo no ADR e no README.

---

## 8. Ciclo de vida

- Cadência de revisão do estudo: **24 meses** (contados a partir da data-base declarada no `README.md`).
- Ao atualizar informação de fornecedor (preço, licença, funcionalidade), atualizar a **data-base** do `README.md` e citar a data da consulta na fonte em `docs/99-referencias.md`.

---

## 9. Comandos úteis

Este repositório não tem build de código. Comandos aplicáveis:

```bash
# Preview local com MkDocs Material (requer Python 3.10+)
pip install mkdocs-material mkdocs-same-dir
mkdocs serve      # http://127.0.0.1:8000

# Build estrito (falha em link quebrado)
mkdocs build --strict

# Lint de Markdown (opcional)
npx markdownlint-cli2 "**/*.md"
```

---

## 10. Contato / governança do documento

Responsável técnico e canal de contribuição declarados em [`README.md`](README.md). Alterações estruturais (nova plataforma, novo critério, mudança de peso) exigem aprovação do Comitê de Arquitetura da SETDIG.
