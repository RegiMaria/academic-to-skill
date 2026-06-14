---
name: academic-to-skill
description: "Converte artigos científicos, livros e documentos técnicos (PDF, EPUB, DOCX, HTML, Markdown, texto simples, RTF, MOBI/AZW com Calibre) em skills estruturadas e reutilizáveis para agentes de IA. Use quando quiser estudar um documento pelo Claude Code, aplicar os frameworks do autor enquanto trabalha, ou construir uma base de conhecimento reutilizável a partir de um arquivo. Versão acadêmica com suporte a references.md, methodology.md, key-findings.md e research-gaps.md e replication-checklist.md."
compatibility: "Diretórios de skill do Claude Code (~/.claude/skills) e do Amp (.agents/skills, ~/.config/agents/skills, ~/.config/amp/skills)."
allowed-tools:
  - shell_command
  - Read
  - Write
  - Glob
  - Grep
argument-hint: <caminho-do-documento> [nome-da-skill]
---

# Conversor Acadêmico para Skill

Transforma conhecimento escrito em skills acionáveis para agentes de IA, extraindo estrutura, não produzindo resumos genéricos.

## Filosofia

Artigos científicos e livros técnicos contêm expertise cristalizada: frameworks, metodologias e descobertas que levaram anos para ser desenvolvidos. Esta skill extrai esse conhecimento em um formato que o Claude Code pode usar repetidamente, sem precisar reler o documento a cada sessão.

**Extraia estrutura, não resumos.** Uma skill não é um relatório de leitura. É um conjunto de:
- Frameworks nomeados (modelos mentais com aplicação clara)
- Princípios acionáveis (regras que guiam decisões)
- Descobertas-chave com evidências numéricas
- Metodologia e contexto experimental
- Mapa de referências com contexto de uso

**Preserve a precisão do autor.** Terminologia científica tem nomes específicos por razões específicas. Capture a formulação exata — "Model Variability Problem (MVP)" não é intercambiável com "o modelo varia".

**Camadas de profundidade.** Documentos simples → skills simples. Artigos com múltiplos experimentos → skills com arquivos de referência e seções sob demanda.

---

## Modos de Operação

### 1. Conversão Completa (Padrão)
**Gatilho:** Usuário fornece um caminho de documento suportado sem instruções especiais
**Ação:** Executar todos os passos abaixo (Passos 0–10)
**Saída:** Skill completa com SKILL.md, chapters/, glossary, patterns, cheatsheet + arquivos acadêmicos opcionais

### 2. Apenas Análise
**Gatilho:** Usuário diz "analisar", "só extrair", ou "quero revisar antes de gerar"
**Ação:** Executar Passos 0–3, produzir relatório de extração estruturado e parar
**Saída:** Relatório de análise para revisão do usuário

### 3. Gerar a partir de Análise Prévia
**Gatilho:** Usuário tem notas de análise existentes ou executou apenas análise anteriormente
**Ação:** Pular Passos 0–3, usar a análise fornecida como entrada, executar Passos 4–10
**Saída:** Arquivos de skill a partir da análise fornecida

---

## Localizações das Skills

Ao procurar o script auxiliar ou gravar a skill gerada, preferir estas localizações nesta ordem:

1. Skills locais do projeto Amp: `.agents/skills/`
2. Skills globais do Amp: `~/.config/agents/skills/`
3. Skills legadas do Amp: `~/.config/amp/skills/`
4. Skills do Claude Code: `~/.claude/skills/`

Skills geradas devem usar `~/.claude/skills/` por padrão.

---

## Passo 0 — Verificação de escopo

Se o argumento NÃO for um caminho para um arquivo de documento suportado, parar e responder:
> "O academic-to-skill requer um caminho de documento suportado. Uso: `academic-to-skill /caminho/para/artigo.pdf [nome-da-skill]`, ou outro formato suportado: `.epub`, `.docx`, `.md`, `.txt`, `.html`, `.rtf`, `.mobi`, `.azw3`."

Tratar o primeiro argumento como `BOOK_PATH` e o segundo argumento opcional como `SKILL_NAME`.

---

## Passo 1 — Validar entrada

```bash
test -f "$BOOK_PATH" && echo "ARQUIVO_OK" || echo "ARQUIVO_NAO_ENCONTRADO: $BOOK_PATH"
case "${BOOK_PATH##*.}" in
  pdf|PDF|epub|EPUB|docx|DOCX|txt|TXT|md|MD|markdown|MARKDOWN|rst|RST|adoc|ADOC|asciidoc|ASCIIDOC|html|HTML|htm|HTM|rtf|RTF|mobi|MOBI|azw|AZW|azw3|AZW3) echo "FORMATO_OK" ;;
  *) echo "FORMATO_DESCONHECIDO" ;;
esac
```

Se o arquivo não for encontrado ou o formato não for suportado, parar com mensagem de erro clara.

---

## Passo 1.5 — Identificar tipo de documento

Perguntar ao usuário:

> "Que tipo de conteúdo este documento tem? Isso me ajuda a escolher o melhor método de extração.
>
> 1. **Técnico** — tem tabelas, fórmulas, diagramas, código (ex: artigos acadêmicos, papers de pesquisa)
> 2. **Texto corrido** — principalmente prosa, poucas tabelas (ex: livros de gestão, ensaios)
> 3. **Não tenho certeza** — usarei o método rápido e avisarei se a qualidade parecer limitada"

Armazenar a resposta como `BOOK_TYPE`:
- Opção 1 → `BOOK_TYPE=technical`
- Opção 2 → `BOOK_TYPE=text`
- Opção 3 → `BOOK_TYPE=text`

**Se `BOOK_TYPE=technical`:**
> "📐 Modo técnico selecionado — usando Docling para extração com reconhecimento de layout (tabelas, fórmulas e figuras preservadas como markdown). Isso leva ~1,5s por página. Iniciando agora…"

**Se `BOOK_TYPE=text`:**
> "📄 Modo texto selecionado — usando o extrator mais rápido disponível para este tipo de arquivo."

---

## Passo 2 — Extrair texto do documento fonte

```bash
SCRIPT_PATH=""
for candidate in \
  ".agents/skills/academic-to-skill/scripts/extract.py" \
  "$HOME/.config/agents/skills/academic-to-skill/scripts/extract.py" \
  "$HOME/.config/amp/skills/academic-to-skill/scripts/extract.py" \
  "$HOME/.claude/skills/academic-to-skill/scripts/extract.py"
do
  if [ -f "$candidate" ]; then
    SCRIPT_PATH="$candidate"
    break
  fi
done

if [ -z "$SCRIPT_PATH" ]; then
  echo "Não foi possível encontrar scripts/extract.py para academic-to-skill" >&2
  exit 1
fi

PYTHON_BIN="${PYTHON_BIN:-python3}"
if ! command -v "$PYTHON_BIN" >/dev/null 2>&1; then
  PYTHON_BIN="python"
fi

"$PYTHON_BIN" "$SCRIPT_PATH" "$BOOK_PATH" --mode <BOOK_TYPE> --install-missing ask
```

Isso cria:
- `<tempdir>/book_skill_work/full_text.txt` — texto completo extraído
- `<tempdir>/book_skill_work/metadata.json` — título, páginas estimadas, contagem de tokens, tamanho, modo de extração

---

## Passo 2.6 — Acesso eficiente para documentos grandes (> 50k tokens)

Para documentos acima de ~50k tokens, preferir sondagens programáticas:

```bash
# Verificar tamanho antes de qualquer leitura
wc -w "$FULL_TEXT_PATH"

# Encontrar offsets de seções sem carregar o arquivo inteiro
grep -n -E "^\s*(Abstract|Introduction|Methods|Results|Discussion|References|Chapter|Section|[0-9]+\.)\s" "$FULL_TEXT_PATH" | head -40

# Extrair apenas a seção necessária
sed -n '<inicio>,<fim>p' "$FULL_TEXT_PATH"

# Verificar se um termo é mencionado antes de incluí-lo
grep -c -i "termo_buscado" "$FULL_TEXT_PATH"
```

---

## Passo 3 — Analisar estrutura do documento

Ler os primeiros 8.000 caracteres do `full_text.txt` para identificar:
- **Título** e **autor(es)**
- **Estrutura de seções** (Abstract, Introduction, Methods, Results, Discussion, References — ou capítulos numerados)
- **Temas centrais** e domínio do assunto
- Se é um **artigo científico** (tem Abstract + seção References estruturada)

**Se for artigo científico, ativar `IS_PAPER=true`** — isso habilita os arquivos acadêmicos extras no Passo 4.5.

**Se modo for "Apenas Análise":** produzir o relatório de extração agora e parar:

```
## Relatório de Extração — <Título>

### Tipo de documento
<artigo científico / livro técnico / outro>

### Estrutura detectada
| # | Seção/Capítulo | Conteúdo principal |
|---|---------------|-------------------|

### Frameworks e Conceitos Centrais
- **<Nome>**: <o que é e quando aplicar>

### Metodologia (se artigo)
- Dataset: <nome>
- Modelos testados: <lista>
- Métricas: <lista>

### Nome sugerido para a skill
`{sobrenome-autor}-{conceito-central}`
```

---

## Passo 4 — Perguntar finalidade (apenas Conversão Completa)

> "Para que deve servir esta skill? (Escolha uma ou mais)
> 1. Aplicar os frameworks do autor enquanto trabalho
> 2. Consultar metodologia e resultados experimentais
> 3. Referenciar seções e conceitos específicos
> 4. Todas as opções acima"

Usar a resposta para pesar o que é destacado na seção Core do SKILL.md.

---

## Passo 4.5 — Selecionar arquivos acadêmicos (se IS_PAPER=true)

Este passo ocorre ANTES da estimativa de custo para que o total reflita os arquivos escolhidos.

> "Este documento é um artigo científico. Quais arquivos acadêmicos extras deseja gerar?
>
> 1. **references.md** — as 20 referências mais citadas no texto, com título completo e contexto de uso em português
> 2. **methodology.md** — dataset, métricas, baseline, configuração experimental e checklist de replicação
> 3. **key-findings.md** — resultados principais com números e evidências, prontos para citar em revisão de literatura
> 4. **research-gaps.md** — lacunas e trabalhos futuros apontados pelos próprios autores
> 5. **replication-checklist.md** — checklist operacional, standalone, para reproduzir os experimentos (dataset, modelo, hiperparâmetros, prompts, métricas)
> 6. **Todos os anteriores** (recomendado — custo adicional estimado: ~$0,06–0,18 USD)
> 7. **Nenhum** — pular esta etapa"

Armazenar seleção como `ACADEMIC_FILES`. Aguardar resposta antes de prosseguir.

---

## Passo 5 — Estimativa de custo antes de prosseguir

Ler `<tempdir>/book_skill_work/metadata.json` e apresentar ao usuário:

```
📖 Documento detectado: <nome_do_arquivo> (<formato>)
📄 Páginas: ~<N> | Palavras: ~<N> | Tokens da fonte: ~<N>K

💰 Estimativa de custo (Conversão Completa):
   Entrada  (leitura + prompts): ~<N>K tokens
   Saída    (arquivos gerados):  ~<N>K tokens
   Arquivos acadêmicos extras:   ~<N>K tokens  [se selecionados]
   Total:                        ~<N>K tokens

   Preços de referência (2025):
   Claude Sonnet 4.5 → ~$<X> USD
   Claude Haiku 4.5  → ~$<X> USD

   ⏱  Tempo estimado: ~<N> minutos

📁 Arquivos a serem gerados:
   SKILL.md + <N> arquivos de seção + glossary + patterns + cheatsheet
   <+ references.md / methodology.md / key-findings.md / research-gaps.md se selecionados>

➡  Prosseguir com a Conversão Completa?
```

**Como estimar:**
- Tokens de entrada ≈ `estimated_tokens` × 1,3 (overhead de prompts)
- Tokens de saída base ≈ seções × 1.000 + 4.000 (SKILL.md) + 4.500 (glossary + patterns + cheatsheet)
- Tokens de saída acadêmicos ≈ 800 por arquivo selecionado (references.md, methodology.md, key-findings.md, research-gaps.md), ≈ 600 para replication-checklist.md (formato mais compacto, sem prosa)
- Preço: Sonnet entrada=$3/MTok saída=$15/MTok — Haiku entrada=$0,80/MTok saída=$4/MTok

Aguardar confirmação do usuário antes de prosseguir.


---

## Passo 6 — Determinar nome e criar estrutura da skill

Se `SKILL_NAME` foi fornecido, usá-lo como slug.
Caso contrário, propor duas opções:
- **Por autor-conceito**: `{sobrenome-autor}-{conceito-central}` (ex: `herrera-llm-variability`)
- **Por título**: letras minúsculas com hífens (ex: `domain-drift-sentiment-analysis`)

Verificar que `$SKILLS_HOME/<skill_name>/` NÃO existe. Se existir, adicionar `-2` ou perguntar antes de sobrescrever.

```bash
mkdir -p "$SKILLS_HOME/<skill_name>/chapters"
```

---

## Passo 7 — Gerar resumos de seções

**REGRA DE ORÇAMENTO DE TOKENS — CRÍTICA:**
- Cada arquivo de seção: **800–1.200 tokens** (denso, não verboso)

Para CADA seção/capítulo principal identificado no Passo 3, ler a seção correspondente do `full_text.txt` e criar `$SKILLS_HOME/<skill_name>/chapters/ch<NN>-<slug>.md`.

> ⚠️ **Todo o conteúdo dos arquivos de seção deve ser gerado em português**, independentemente do idioma original do documento.

**Adaptar o template conforme o tipo de documento:**

### Template para artigos científicos (IS_PAPER=true)

```markdown
# Seção N: <Título Completo>

## Ideia Central
<1–2 frases: o que esta seção argumenta ou demonstra>

## Hipótese ou Questão de Pesquisa
<A pergunta que esta seção tenta responder, se aplicável>

## Principais Conceitos Introduzidos
- **<Termo>**: <definição precisa em 1 frase>
(5–10 termos mais importantes desta seção)

## Frameworks ou Taxonomias
- **<Nome do Framework>**: <formulação exata — preservar nomenclatura do autor>
  - Quando usar: <situação específica>
  - Como: <passos ou critérios>

## Resultados ou Argumentos Centrais
<O que esta seção demonstra, com números se disponíveis>

## Tabelas e Figuras Relevantes *(modo técnico)*
<Reproduzir em markdown tabelas de resultados importantes desta seção>

## Limitações Desta Seção
<O que os autores reconhecem como limitação neste ponto do argumento>

## Conecta Com
- **Seção N**: <por que esta seção se relaciona>
- **<Conceito externo>**: <conexão com literatura mais ampla>
```

### Template para livros técnicos (IS_PAPER=false)

```markdown
# Capítulo N: <Título Completo>

## Ideia Central
<1–2 frases: a coisa mais importante que este capítulo ensina>

## Frameworks Introduzidos
- **<Nome do Framework>**: <formulação exata — preservar o nome do autor>
  - Quando usar: <situação específica>
  - Como: <passos ou critérios>

## Conceitos-Chave
- **<Termo>**: <definição precisa em 1 frase>

## Modelos Mentais
<2–4 ferramentas de pensamento. Escrever como "Use X quando Y">

## Anti-padrões
- **<O que evitar>**: <por que falha>

## Exemplos de Código *(modo técnico — omitir se BOOK_TYPE=text)*
```<linguagem>
<exemplo chave deste capítulo>
```

## Principais Conclusões
1. <Insight acionável>
2. <Insight acionável>
3. <Insight acionável>

## Conecta Com
- **Cap N**: <por que este capítulo se relaciona>
```

---

## Passo 8 — Gerar arquivos de suporte padrão

> ⚠️ **Todo o conteúdo dos arquivos abaixo deve ser gerado em português.**

### glossary.md
Criar `$SKILLS_HOME/<skill_name>/glossary.md`:
- Todos os termos significativos do documento, em ordem alfabética
- Formato: `**Termo** — definição (Seção N)`
- Máx 1.500 tokens

### patterns.md
Criar `$SKILLS_HOME/<skill_name>/patterns.md`:
- Todas as técnicas concretas, padrões e algoritmos do documento
- Formato: `## Nome do Padrão\n**Quando usar**: ...\n**Como**: ...\n**Trade-offs**: ...`
- Máx 2.000 tokens

### cheatsheet.md
Criar `$SKILLS_HOME/<skill_name>/cheatsheet.md`:
- Tabelas de decisão, matrizes de comparação, regras de referência rápida
- O conteúdo que você gostaria em uma única página impressa
- Máx 1.000 tokens

---

## Passo 8.5 — Gerar arquivos acadêmicos (se ACADEMIC_FILES selecionado)

Gerar apenas os arquivos escolhidos no Passo 4.5.

> ⚠️ **Todo o conteúdo dos arquivos acadêmicos deve ser gerado em português**, mesmo que o documento original esteja em inglês ou outro idioma.

---

### references.md (se selecionado)

Criar `$SKILLS_HOME/<skill_name>/references.md`.

**Como gerar — seguir esta ordem:**
1. Localizar a seção "References" ou "Bibliography" no final do `full_text.txt`
2. Extrair todas as referências listadas
3. Varrer o corpo do texto identificando cada citação — capturar o parágrafo onde ela aparece
4. Contar quantas vezes cada referência é citada no corpo do texto
5. Selecionar as **20 mais citadas** (não as 20 primeiras da lista)
6. Cruzar cada identificador com o título completo na seção References
7. Escrever o contexto de uso baseado nas passagens reais do texto — não em inferências gerais

```markdown
# Referências — <Título do Artigo>

> Gerado em português. As 20 referências mais citadas no texto,
> com título completo e contexto de uso no argumento do artigo.
> Ordenadas por número de citações (mais citada primeiro).

---

## [Autor et al., ANO] — <Título completo do paper>

**Citada**: <N> vezes | **Seções**: <lista de seções onde aparece>

**Contexto de uso**:
<3–4 linhas em português explicando: em que momento do argumento
esta referência aparece, que afirmação ela sustenta, e por que
o autor a considera relevante para o argumento central do artigo.
Baseado nas passagens reais do texto, não em suposições.>

---
```

---

### methodology.md (se selecionado)

Criar `$SKILLS_HOME/<skill_name>/methodology.md`.

```markdown
# Metodologia — <Título do Artigo>

> Gerado em português. Extraído da seção de metodologia do artigo.
> Use para comparar abordagens entre artigos ou replicar experimentos.

## Dataset
- **Nome**: <nome do dataset>
- **Tamanho**: <número de amostras>
- **Fonte**: <de onde vem / como foi coletado>
- **Período**: <quando foi coletado, se mencionado>
- **Características**: <idioma, domínio, tipo de dado>

## Modelos Testados
- **Modelo principal**: <nome e versão>
- **Modelos de comparação (baseline)**: <lista>
- **Configuração**: <temperatura, parâmetros relevantes>

## Métricas de Avaliação
- **Métrica principal**: <nome + o que mede>
- **Métricas secundárias**: <lista>
- **Como são calculadas**: <fórmula ou descrição breve>

## Configuração Experimental
- **Hardware**: <se mencionado>
- **Número de execuções / seeds**: <quantas rodadas>
- **Divisão treino/teste/validação**: <se aplicável>
- **Hiperparâmetros relevantes**: <lista>

## Limitações Metodológicas Declaradas
- <Limitação 1 apontada pelos autores>
- <Limitação 2>
- <Limitação 3>

## Checklist de Replicação
Mínimo necessário para replicar os experimentos:
- [ ] Dataset: <onde obter>
- [ ] Modelo: <versão exata>
- [ ] Configuração: <parâmetros principais>
- [ ] Métrica: <como calcular>
- [ ] <outros passos críticos>
```

---

### key-findings.md (se selecionado)

Criar `$SKILLS_HOME/<skill_name>/key-findings.md`.

```markdown
# Principais Descobertas — <Título do Artigo>

> Gerado em português. Resultados com evidências numéricas.
> Use para comparações entre artigos e para embasar afirmações
> em revisão de literatura.

## Descoberta 1 — <Título descritivo>
- **O que foi testado**: <descrição>
- **Resultado**: <número / percentual / estatística principal>
- **Condições**: <contexto experimental — modelo, dataset, configuração>
- **Significância estatística**: <p-value, IC ou equivalente, se mencionado>
- **Interpretação dos autores**: <conclusão que os autores tiram deste resultado>

## Descoberta 2 — <Título descritivo>
<mesma estrutura>

## Tabela Resumo de Resultados
| Experimento | Modelo / Método | Métrica | Resultado | Condição |
|-------------|-----------------|---------|-----------|----------|
| <exp1>      | <modelo>        | <métrica> | <valor> | <config> |

## O Que Este Artigo Prova
<2–3 frases resumindo as afirmações centrais que os dados sustentam>

## O Que Este Artigo NÃO Prova
<Limitações das conclusões — o que os dados não permitem generalizar,
segundo os próprios autores ou inferível da metodologia>
```

---

### research-gaps.md (se selecionado)

Criar `$SKILLS_HOME/<skill_name>/research-gaps.md`.

```markdown
# Lacunas de Pesquisa — <Título do Artigo>

> Gerado em português. O que os próprios autores apontam como
> incompleto, limitado ou como direção para trabalhos futuros.
> Use para identificar oportunidades de pesquisa derivada.

## Limitações Declaradas pelos Autores
- **<Limitação 1>**: <descrição + por que importa para a validade dos resultados>
- **<Limitação 2>**: <descrição>
- **<Limitação 3>**: <descrição>

## Trabalhos Futuros Sugeridos pelos Autores
- <Sugestão 1 — extraída diretamente do texto>
- <Sugestão 2>
- <Sugestão 3>

## Perguntas em Aberto Identificadas no Texto
<Questões que o artigo levanta mas não responde — inferidas
a partir do texto, mesmo que não declaradas explicitamente pelos autores>

## Oportunidades para Pesquisa Derivada
<3–5 direções concretas que um pesquisador poderia explorar
a partir das lacunas deste artigo — escritas como perguntas de pesquisa>
```
### replication-checklist.md (se selecionado)

Criar `$SKILLS_HOME/<skill_name>/replication-checklist.md`.

**Diferença em relação ao checklist em `methodology.md`:** este arquivo é
**standalone** — apenas a lista de tarefas, sem prosa, pronto para abrir e
marcar item por item. É mais granular e operacional que o resumo de alto
nível dentro de `methodology.md`. Não copiar o conteúdo de um para o outro.

**Como gerar — seguir esta ordem:**
1. Para cada seção do checklist (Dados, Modelo, Hardware, Hiperparâmetros, Prompts, Execução, Métricas, Software), buscar informações explícitas no `full_text.txt`
2. **Prompts**: preservar estrutura e placeholders (ex: `[DOCUMENT]`), mas NUNCA citar mais de 15 palavras literais consecutivas — parafrasear preservando o significado operacional
3. Se uma informação não estiver no artigo, marcar explicitamente como "Não informado no artigo" — NUNCA inventar valores
4. Ao final, listar em "Itens Não Especificados no Artigo" tudo que ficou sem preenchimento — isso é tão útil quanto o que foi encontrado
5. Formato: cada item é uma linha `- [ ] ` — sem prosa explicativa entre os itens, apenas o necessário para identificar o que fazer

```markdown
# Checklist de Replicação — <Título do Artigo>

> Gerado em português. Checklist operacional para reproduzir os experimentos
> descritos no artigo. Marque cada item conforme avança.

## Dados

- [ ] Dataset: <nome> — <link/fonte, se disponível no texto>
- [ ] Tamanho: <N amostras/registros>
- [ ] Pré-processamento necessário: <se descrito>
- [ ] Split treino/teste/validação: <se aplicável>

## Modelo(s)

- [ ] Modelo: <nome + versão exata, ex: GPT-4o, Mixtral 8x22B>
- [ ] Acesso: <API / pesos locais / framework usado>
- [ ] Baseline(s) de comparação: <se houver>

## Hardware

- [ ] <especificação de hardware, se mencionada — GPUs, VRAM, etc.>
- [ ] <se não especificado: "Não informado no artigo">

## Hiperparâmetros

- [ ] Temperature: <valor(es)>
- [ ] Top-p: <valor>
- [ ] Top-k: <valor, se aplicável>
- [ ] Seed: <valor ou "não fixado / em beta", se mencionado>
- [ ] <outros hiperparâmetros relevantes mencionados>

## Prompts

- [ ] Prompt 1: <paráfrase do prompt — preservar estrutura e placeholders como [DOCUMENT]>
- [ ] Prompt 2: <se houver mais de um>

## Execução

- [ ] Número de execuções/repetições: <N>
- [ ] Ordem ou randomização: <se especificado>

## Métricas

- [ ] Métrica 1: <nome> — <como calcular, em uma frase>
- [ ] Métrica 2: <...>

## Software / Bibliotecas

- [ ] <nome + versão, se mencionado no artigo — ex: llama.cpp, biblioteca X>
- [ ] <se não especificado: "Versões não informadas no artigo">

## Itens Não Especificados no Artigo

Lista de itens acima que o artigo não detalha — o pesquisador precisará
decidir ou buscar em trabalhos relacionados:
- <item 1>
- <item 2>
```

---

## Passo 9 — Gerar o SKILL.md principal

**ORÇAMENTO CRÍTICO: Manter o corpo do SKILL.md abaixo de 4.000 tokens.**
A compactação trunca pelo FIM — colocar o conteúdo mais importante PRIMEIRO.

Criar `$SKILLS_HOME/<skill_name>/SKILL.md`:

```markdown
---
name: <skill_name>
description: "Base de conhecimento de \"<Título Completo>\" por <Autor(es)>. Use quando aplicar os frameworks de <autor> para <tópicos-chave, 3–6 termos>, estudar o documento, ou referenciar seus conceitos e descobertas."
allowed-tools:
  - Read
  - Grep
argument-hint: [tópico, nome do framework, ou número da seção]
---

# <Título Completo>
**Autor(es)**: <Autor(es)> | **Páginas**: ~<N> | **Seções**: <N> | **Gerado em**: <AAAA-MM-DD>

## Como Usar Esta Skill

- **Sem argumentos** — carregar frameworks centrais para referência
- **Com um tópico** — perguntar sobre `variabilidade`, `metodologia`, ou outro tópico indexado
- **Com seção** — pedir `sec03` para carregar aquela seção específica
- **Navegar** — perguntar "quais seções você tem?" para ver o índice completo

---

## Frameworks Centrais e Conceitos Principais

<gerar ~2.000 tokens dos frameworks, descobertas e insights mais críticos — em português>

---

## Índice de Seções

| # | Título | Conteúdo Principal |
|---|--------|-------------------|
| [sec01](chapters/ch01-<slug>.md) | <Título> | <conceito1>, <conceito2> |
| [sec02](chapters/ch02-<slug>.md) | <Título> | <conceito1>, <conceito2> |

## Índice de Tópicos
- **<Termo>** → sec<N>[, sec<N>]

## Arquivos de Suporte
- [glossary.md](glossary.md) — todos os termos-chave com definições
- [patterns.md](patterns.md) — técnicas e padrões
- [cheatsheet.md](cheatsheet.md) — tabelas de referência rápida

## Arquivos Acadêmicos
- [references.md](references.md) — 20 referências mais citadas com contexto de uso
- [methodology.md](methodology.md) — dataset, métricas e configuração experimental
- [key-findings.md](key-findings.md) — resultados com evidências numéricas
- [research-gaps.md](research-gaps.md) — lacunas e trabalhos futuros
- [replication-checklist.md](replication-checklist.md) — checklist operacional para reproduzir os experimentos


---

## Escopo e Limites

Esta skill cobre apenas o conteúdo do documento fornecido.
Para tópicos além deste documento, consulte skills relacionadas ou pergunte ao agente diretamente.
```
> Incluir no índice "Arquivos Acadêmicos" apenas os links dos arquivos que foram efetivamente selecionados e gerados no Passo 4.5. Não listar arquivos não gerados.

---

## Passo 10 — Limpeza e relatório final

```bash
PYTHON_BIN="${PYTHON_BIN:-python3}"
if ! command -v "$PYTHON_BIN" >/dev/null 2>&1; then
  PYTHON_BIN="python"
fi

"$PYTHON_BIN" - <<'PY'
import os, shutil, tempfile
from pathlib import Path
shutil.rmtree(
    os.environ.get("BOOK_SKILL_WORKDIR", Path(tempfile.gettempdir()) / "book_skill_work"),
    ignore_errors=True,
)
PY
```

Reportar ao usuário:

```
✅ Skill criada: $SKILLS_HOME/<skill_name>/

📚 Documento: <Título Completo> — <Autor>
📄 Páginas: ~<N> | Seções: <N>

Arquivos gerados:
  SKILL.md           — frameworks centrais + índice     (~X tokens)
  chapters/          — <N> resumos de seções            (~X tokens cada)
  glossary.md        — termos-chave                     (~X tokens)
  patterns.md        — técnicas e padrões               (~X tokens)
  cheatsheet.md      — referência rápida                (~X tokens)

Arquivos acadêmicos:
  references.md            — 20 referências mais citadas      (~X tokens)  [se gerado]
  methodology.md           — dataset e configuração           (~X tokens)  [se gerado]
  key-findings.md          — resultados com números           (~X tokens)  [se gerado]
  research-gaps.md         — lacunas e trabalhos futuros      (~X tokens)  [se gerado]
  replication-checklist.md — checklist operacional            (~X tokens)  [se gerado]
  ─────────────────────────────────────────────────────────────────
  Total: ~X tokens (carregados sob demanda, não todos de uma vez)

Como usar:
  /<skill_name>                    → carregar frameworks centrais
  /<skill_name> <tópico>           → encontrar e explicar um tópico
  /<skill_name> sec<N>             → mergulhar em uma seção específica
  /<skill_name> metodologia        → ver configuração experimental
  /<skill_name> descobertas        → ver resultados principais
  /<skill_name> referencias        → consultar mapa de referências
  /<skill_name> checklist          → consultar checklist de replicação
```

---

## Regras de Qualidade

1. **Extraia estrutura, não resumos** — capture frameworks nomeados, formulações exatas, resultados com números; não recapitulações genéricas
2. **Preserve a precisão do autor** — "Model Variability Problem (MVP)" ≠ "o modelo varia"; mantenha a nomenclatura exata
3. **Densidade acima de completude** — um resumo de 1.000 tokens supera um trecho de 10.000 tokens
4. **Voz de praticante** — escreva "Use X quando Y", não "O artigo explica X"
5. **SKILL.md na frente** — a compactação mantém os primeiros 5.000 tokens; o conteúdo mais importante vem primeiro
6. **Arquivos de seção são sob demanda** — não contam contra o orçamento da skill até serem carregados
7. **Nunca copie texto bruto do documento** — sempre sintetize, resuma, extraia o sinal
8. **Índice de tópicos é crítico** — é como o agente navega até o arquivo correto
9. **Todo o conteúdo gerado em português** — SKILL.md, chapters, glossary, patterns, cheatsheet e todos os arquivos acadêmicos devem ser escritos em português, independentemente do idioma original do documento
10. **Contexto de citações baseado no texto real** — no references.md, o contexto de uso deve vir das passagens reais onde a citação aparece, nunca de inferências gerais sobre o paper citado
11. **Template correto por tipo de documento** — usar template de artigo científico quando IS_PAPER=true; template de livro técnico quando IS_PAPER=false
12. **Estimativa de custo após seleção de arquivos** — o Passo 5 sempre reflete os arquivos acadêmicos já selecionados no Passo 4.5