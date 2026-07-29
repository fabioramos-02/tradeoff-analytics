# CLAUDE.md — Instruções do repositório

Este arquivo orienta o Claude Code (e outros agentes de IA) sobre como colaborar neste repositório.

---

## 1. O que é este repositório

Estudo técnico de arquitetura para seleção de plataforma de **Web Analytics** para o Governo do Estado de Mato Grosso do Sul, conduzido pela **Secretaria-Executiva de Transformação Digital — SETDIG**.

- Não é código de aplicação. É **documentação técnica em Markdown** organizada como artefato arquitetural formal (TOGAF + ATAM + ADR + MAUT).
- Público-alvo: Comitê de Arquitetura, gestores públicos, times técnicos das Secretarias.
- Publicação-alvo: GitHub, Azure DevOps Wiki, MkDocs Material ou Docusaurus — sem reescrita.

---

## 2. Estrutura obrigatória

```
analytics-platform-study/
├── README.md                # índice principal
├── CLAUDE.md                # este arquivo
├── docs/                    # núcleo do estudo (01 a 12)
├── plataformas/             # 10 fichas técnicas individuais
├── comparativos/            # 6 cortes transversais
└── anexos/                  # matriz, benchmark, glossário
```

Índice canônico e status de cada arquivo: [`README.md`](README.md) §5.

---

## 3. Convenções editoriais (obrigatórias)

Definidas em [`README.md`](README.md) §7. Resumo:

| Regra | Detalhe |
|-------|---------|
| Idioma | Português do Brasil (pt-BR), com acentuação completa |
| Nome oficial | Sempre "Secretaria-Executiva de Transformação Digital — SETDIG". Nunca "Governo Digital" |
| Blocos semânticos | `> **📌 Observação**`, `> **✅ Bloco de Decisão**`, `> **⚠️ Bloco de Risco**`, `> **🚨 Alerta**`, `> **🔍 Evidência**` — em citação, nunca `!!! note` / `> [!NOTE]` |
| Links | Sempre **relativos**, para funcionar em GitHub + Wiki + MkDocs + Docusaurus |
| Diagramas | Mermaid dentro de fenced code blocks ` ```mermaid ` |
| Notas | Escala inteira 1–5. Sem meia-nota. Justificativa textual obrigatória |
| Sumário | Todo documento tem seu próprio Sumário em `## Sumário` no topo |
| Frontmatter | Cabeçalho padrão com pontuação, situação, e link `[← Voltar ao índice]` |

---

## 4. Regra de evidência

> **📌 Observação**
> Toda afirmação técnica deve ser rastreável a **fonte primária**: documentação oficial do fornecedor, código-fonte, licença, especificação de API, norma técnica ou instrumento normativo. Blogs, artigos de opinião e marketing **não** são fonte primária — se citados, rotular explicitamente como fonte secundária.

- Fontes agregadas em [`docs/12-referencias.md`](docs/12-referencias.md).
- Ao adicionar afirmação técnica nova, adicione a fonte à `docs/12-referencias.md` no mesmo commit.

---

## 5. Ao editar / criar conteúdo

### 5.1 Antes de escrever

1. Ler o arquivo relacionado em `docs/` para não contradizer decisões já tomadas (ADR-001, matriz, roadmap).
2. Ler pelo menos uma ficha de plataforma existente (`plataformas/plausible.md` é referência estrutural) para preservar o padrão de seções.
3. Se a mudança afeta a matriz ([`docs/07-matriz-decisao.md`](docs/07-matriz-decisao.md)), atualizar também [`anexos/matriz.md`](anexos/matriz.md) e o Resumo Executivo do [`README.md`](README.md) no mesmo commit.

### 5.2 Fichas de plataforma (`plataformas/*.md`)

Seções obrigatórias, nesta ordem:

1. Visão geral (identificação, história, comunidade, modelo de negócio, casos de uso)
2. Licenciamento
3. Hospedagem
4. Funcionalidades
5. APIs
6. Exemplos de integração (Python, Node.js, e ao menos um BI: Power BI / Grafana / Superset / Metabase / Looker Studio)
7. Integrações (GTM, Consent Manager, IdP, OAuth/OIDC)
8. Infraestrutura
9. Segurança
10. Governança
11. Performance
12. Custos (com TCO em 5 anos)
13. Pontos fortes
14. Pontos fracos
15. Quando utilizar
16. Quando evitar
17. Notas do avaliador (opcional, para contexto/ressalvas)

Cabeçalho padronizado:

```markdown
# Nome da Plataforma

> **Ficha técnica de plataforma** · Pontuação na matriz: **XXX/600 (XX,X %)** — posição · status
> **Situação:** ✅/⚠️/🚨 <descrição curta do papel na arquitetura recomendada>
> [← Voltar ao índice](../README.md) · [Matriz de decisão](../docs/07-matriz-decisao.md)
```

### 5.3 Comparativos (`comparativos/*.md`)

Cortes transversais: **não repetir** o que já está na ficha da plataforma. Comparar dimensões lado a lado, com tabela sinóptica no topo e análise por plataforma logo abaixo.

### 5.4 Anexos (`anexos/*.md`)

- `matriz.md` — matriz completa em Markdown, exportável para CSV/XLSX. Cada linha (plataforma × critério) tem coluna de nota, peso e produto.
- `benchmark.md` — metodologia + cenários de carga + resultados (podem ser projeções fundamentadas, marcadas como tal).
- `glossario.md` — termo, sigla, definição, referência cruzada.

---

## 6. Metodologia sob a qual o estudo opera

| Arcabouço | Uso | Onde |
|-----------|-----|------|
| TOGAF 10 ADM (fases A e B) | Contexto + requisitos | `docs/01`, `docs/02` |
| ATAM (SEI/CMU) | Árvore de atributos de qualidade + trade-offs | `docs/03`, `docs/06` |
| Gartner Decision Framework | Critérios ponderados, must-have vs nice-to-have | `docs/03`, `docs/04` |
| MAUT | Normalização + agregação da matriz | `docs/07`, `anexos/matriz.md` |
| ADR (Nygard / MADR 4.0) | Registro formal da decisão | `docs/08` |

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
- ❌ Nunca "endossar" ou "invalidar" a escolha atual do Matomo. A avaliação é neutra em relação ao status quo (ver `README.md` §1).

---

## 8. Ciclo de vida

- Cadência de revisão do estudo: **24 meses** (contados a partir da data-base declarada no `README.md`).
- Ao atualizar informação de fornecedor (preço, licença, funcionalidade), atualizar a **data-base** do `README.md` e citar a data da consulta na fonte em `docs/12-referencias.md`.

---

## 9. Comandos úteis

Este repositório não tem build de código. Comandos aplicáveis:

```bash
# Preview local com MkDocs Material (requer Python 3.10+)
pip install mkdocs-material
mkdocs serve      # http://127.0.0.1:8000

# Lint de Markdown (opcional)
npx markdownlint-cli2 "**/*.md"

# Verificar links relativos quebrados (opcional)
npx markdown-link-check README.md
```

---

## 10. Contato / governança do documento

Responsável técnico e canal de contribuição declarados em [`README.md`](README.md) §9. Alterações estruturais (nova plataforma, novo critério, mudança de peso) exigem aprovação do Comitê de Arquitetura da SETDIG.
