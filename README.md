<h1 align="center">📚 academic-to-skill</h1>

<p align="center">
  <strong>Transforme artigos científicos e livros técnicos em skills reutilizáveis para agentes de IA.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Claude_Code-Skill-blueviolet?style=for-the-badge" alt="Claude Code Skill">
  <img src="https://img.shields.io/badge/PDF%20%E2%80%A2%20EPUB%20%E2%80%A2%20DOCX%20%E2%80%A2%20MD%20%E2%80%A2%20HTML%20%E2%80%A2%20RTF%20%E2%80%A2%20MOBI-suportado-green?style=for-the-badge" alt="Formatos suportados">
  <img src="https://img.shields.io/badge/Licença-MIT-blue?style=for-the-badge" alt="MIT License">
</p>

<p align="center">
  <a href="#-por-que">Por que</a> ·
  <a href="#-o-que-gera">O que gera</a> ·
  <a href="#-uso">Uso</a> ·
  <a href="#-requisitos">Requisitos</a> ·
  <a href="#-como-funciona">Como funciona</a> ·
  <a href="#-instalação">Instalação</a> ·
  <a href="#-perguntas-frequentes">FAQ</a>
</p>

---

> **Fork acadêmico** do projeto original [book-to-skill](https://github.com/virgiliojr94/book-to-skill) por virgiliojr94 (MIT License).
> Este fork adiciona funcionalidades específicas para pesquisa acadêmica: `references.md`, `methodology.md`, `key-findings.md` e `research-gaps.md`.

---

## 🤔 Por que

Você leu um artigo científico. Precisa das métricas dele semanas depois. Abre o PDF, procura a tabela de resultados, tenta lembrar qual baseline eles usaram.

Os atalhos habituais não ajudam:
- 📄 "Vou jogar o PDF no Claude" → gasta centenas de milhares de tokens a cada sessão
- 🧠 "Vou perguntar ao Claude sobre este artigo" → ele alucina ou diz que não tem o conteúdo
- 📝 "Vou fazer anotações enquanto leio" → você acaba com um doc de 200 linhas que nunca mais abre

**academic-to-skill resolve isso transformando o documento em uma skill estruturada que o agente carrega sob demanda.**

Uma vez instalada, você só digita `/nome-da-skill metodologia` e o Claude lê o arquivo certo e responde a partir do conteúdo real. Sem alucinação. Sem fuçar em PDFs. O artigo vira parte do seu fluxo de trabalho.

---

## 📦 O que gera

Executar `/academic-to-skill seu-artigo.pdf` cria uma skill completa em `~/.claude/skills/<slug>/`:

### Arquivos padrão (todos os documentos)

| Arquivo | Finalidade | Tamanho |
|---------|-----------|---------|
| `SKILL.md` | Frameworks centrais + índice de seções | ~4.000 tokens |
| `chapters/ch01-*.md` … | Um arquivo por seção, carregado sob demanda | ~1.000 tokens cada |
| `glossary.md` | Todos os termos-chave em ordem alfabética com refs de seção | ~1.500 tokens |
| `patterns.md` | Todas as técnicas, algoritmos e padrões | ~2.000 tokens |
| `cheatsheet.md` | Tabelas de decisão e regras de referência rápida | ~1.000 tokens |

### Arquivos acadêmicos extras (artigos científicos — opcionais)

| Arquivo | Finalidade | Tamanho |
|---------|-----------|---------|
| `references.md` | As 20 referências mais citadas, com título completo e contexto de uso em português | ~2.000 tokens |
| `methodology.md` | Dataset, métricas, baseline, configuração experimental e checklist de replicação | ~1.500 tokens |
| `key-findings.md` | Resultados principais com números e evidências — pronto para citar em revisão de literatura | ~1.500 tokens |
| `research-gaps.md` | Lacunas e trabalhos futuros apontados pelos próprios autores | ~1.000 tokens |

**Os arquivos de seção são carregados sob demanda** — não consomem tokens até você perguntar sobre aquele tópico.

---

## 🚀 Uso

```
/academic-to-skill <caminho-do-documento> [nome-da-skill]
```

Formatos suportados: PDF, EPUB, DOCX, TXT, Markdown, reStructuredText, AsciiDoc, HTML, RTF, MOBI/AZW/AZW3.

**Exemplos:**

```bash
# Artigo científico em PDF
/academic-to-skill ~/Downloads/domain-drift-sentiment-analysis.pdf

# Com nome personalizado
/academic-to-skill ~/artigos/few-shot-specialized.pdf few-shot-especializado

# Caminho completo com nome
/academic-to-skill /mnt/c/Users/voce/Documents/artigo.pdf meu-artigo
```

Após a skill ser criada, use como qualquer outra skill do Claude Code:

```bash
/domain-drift-sentiment                    # carregar frameworks centrais
/domain-drift-sentiment metodologia        # ver configuração experimental
/domain-drift-sentiment descobertas        # ver resultados principais
/domain-drift-sentiment referencias        # consultar mapa de referências
/domain-drift-sentiment sec03              # mergulhar na seção 3
/domain-drift-sentiment "quais seções você tem?"
```

---

## 🔧 Requisitos

O extrator tenta ferramentas na ordem por formato e usa a primeira disponível.

**PDF — escolha pelo tipo de documento:**

| Tipo | Ferramenta | Instalar | Velocidade |
|------|-----------|---------|-----------|
| Texto corrido (prosa, poucas tabelas) | `pdftotext` (poppler) | `sudo apt install poppler-utils` | ⚡ instantâneo |
| Fallback texto | `PyPDF2` | `pip3 install PyPDF2` | ⚡ instantâneo |
| Fallback texto | `pdfminer.six` | `pip3 install pdfminer.six` | ⚡ instantâneo |
| **Técnico (tabelas, fórmulas, figuras)** | **`docling`** | `pip3 install docling` | ~1,5s/página |

> Antes da extração, a skill pergunta se o documento é **técnico** ou **texto corrido** e escolhe a ferramenta automaticamente.

ℹ️ **pdftotext vs Docling — qual usar?**

 O academic-to-skill suporta ambos, mas **recomendamos pdftotext para a maioria dos casos**, especialmente artigos científicos.

**pdftotext** (`sudo apt install poppler-utils`)
- Extrai texto em segundos
- Preserva o layout com a flag `-layout`
- Suficiente para artigos acadêmicos — o Claude lê e interpreta o texto independente do formato das tabelas
- Desvantagem: tabelas saem como texto simples, não como markdown estruturado

 **Docling** (`pip3 install docling`)
- Reconhecimento de layout com IA — preserva tabelas como markdown e fórmulas como texto
- Desvantagem: instala um ecossistema completo de machine learning (~500 MB ou mais), incluindo PyTorch, torchvision, transformers, drivers CUDA, OpenCV e scipy. Leva vários minutos na primeira execução para baixar modelos
- Velocidade: ~1,5s por página (um artigo de 24 páginas leva ~36s só de extração)

**Use Docling apenas se:**
- O artigo tem tabelas de resultados complexas que você precisa consultar estruturadas
- E o pdftotext não consegue extrair essas tabelas de forma legível

> Para a maioria dos artigos acadêmicos, o pdftotext é a escolha certa.

**EPUB:**

| Ferramenta | Instalar | Qualidade |
|-----------|---------|---------|
| `ebooklib` + `beautifulsoup4` | `pip3 install ebooklib beautifulsoup4` | ⭐⭐⭐ Melhor |
| stdlib `zipfile` | embutido — sem instalação | ⭐⭐ Sempre disponível |

**Outros formatos:**

| Formato | Ferramenta | Instalar |
|---------|-----------|---------|
| DOCX | `python-docx` (fallback: stdlib ZIP/XML) | `pip3 install python-docx` |
| HTML | `beautifulsoup4` (fallback: stdlib `html.parser`) | `pip3 install beautifulsoup4` |
| RTF | `striprtf` (fallback: regex) | `pip3 install striprtf` |
| MOBI / AZW / AZW3 | Calibre `ebook-convert` | https://calibre-ebook.com/download |
| TXT / Markdown / reStructuredText / AsciiDoc | embutido | — |

---

## ⚙️ Como funciona

```
PDF ou EPUB
     │
     ▼
Passo 1.5 — "Técnico ou texto corrido?"
     │
     ├── técnico → Docling  (tabelas + fórmulas como markdown, ~1,5s/página)
     └── texto   → pdftotext → PyPDF2 → pdfminer  (instantâneo)
     │
     ▼
scripts/extract.py --mode <technical|text>
  EPUB → ebooklib → stdlib zipfile
     │
     ├── /tmp/book_skill_work/full_text.txt
     └── /tmp/book_skill_work/metadata.json
               │
               ▼
          Claude analisa estrutura
          (título, autor, seções)
          Detecta se é artigo científico (IS_PAPER)
               │
               ▼  (se IS_PAPER=true — ANTES da estimativa de custo)
          Pergunta quais arquivos acadêmicos gerar:
          references.md / methodology.md /
          key-findings.md / research-gaps.md
               │
               ▼
          Estimativa de custo total
          (já inclui arquivos acadêmicos selecionados)
               │
               ▼
          Aguarda confirmação do usuário
               │
               ▼
          Gera resumos por seção
          Gera glossary, patterns, cheatsheet
          Gera arquivos acadêmicos selecionados
          Gera SKILL.md principal com índice
               │
               ▼
          ~/.claude/skills/<slug>/  ✅ gravado
          /tmp/book_skill_work/     🗑️  limpo
```

---

## 📥 Instalação

### 1. Clonar o repositório

```bash
git clone https://github.com/SEU_USUARIO/academic-to-skill.git
cd academic-to-skill
```

### 2. Copiar os arquivos para o Claude Code

```bash
mkdir -p ~/.claude/skills/academic-to-skill/scripts

cp academic-to-skill/SKILL.md ~/.claude/skills/academic-to-skill/SKILL.md
cp academic-to-skill/scripts/extract.py ~/.claude/skills/academic-to-skill/scripts/extract.py
```

### 3. Instalar dependências mínimas para PDF

```bash
pip3 install PyPDF2
```

Para EPUBs:
```bash
pip3 install ebooklib beautifulsoup4
```

### 4. Verificar instalação

```bash
ls ~/.claude/skills/academic-to-skill/
# Deve mostrar: SKILL.md  scripts/
```

### Caminho do arquivo no WSL (Windows)

Se seu arquivo está no Windows, o caminho fica assim no WSL:

| Windows | WSL |
|---------|-----|
| `C:\Users\voce\Documents\artigo.pdf` | `/mnt/c/Users/voce/Documents/artigo.pdf` |

Use sempre aspas duplas se o caminho tiver espaços.

---

## ❓ Perguntas Frequentes

**"Não posso simplesmente jogar o PDF no contexto do Claude?"**

Pode — mas cada conversa vai queimar esse orçamento de tokens no início. Um artigo de 30 páginas é ~20K tokens. Com uma skill, apenas as seções relevantes para sua pergunta são carregadas. O resto fica no disco até você precisar.

Mais importante: injeção de texto bruto é recuperação. Uma skill é raciocínio. Quando você carrega um arquivo de seção, o Claude não está buscando correspondências de palavras-chave — está trabalhando com frameworks pré-extraídos, princípios e descobertas estruturadas para aplicação.

**"Isso não é RAG?"**

RAG funciona em tempo de consulta: fragmentar o documento → embedar tudo → encontrar vetores similares → injetar no prompt. É otimizado para "encontre a parte que fala sobre X."

academic-to-skill funciona em tempo de compilação: uma rodada de análise profunda extrai os frameworks reais do autor, nomeia-os, descreve quando usar cada um, captura metodologia e resultados. A saída é estrutura que o autor levou anos construindo — não uma busca por similaridade em suas frases.

**"Por que os arquivos acadêmicos são em português?"**

Porque o objetivo é reduzir a barreira de uso para pesquisadores brasileiros. O documento original pode estar em inglês — os arquivos gerados pela skill estarão em português, prontos para uso direto.

**"references.md lista todas as referências do artigo?"**

Não — lista as **20 mais citadas dentro do texto**. Artigos científicos podem ter 60-80 referências, mas a maioria aparece uma única vez. As 20 mais citadas são as que o autor considerou mais centrais para o argumento.

**"Posso usar com múltiplos artigos?"**

Sim. Converta cada artigo separadamente — cada um vira uma skill independente. Depois você pode pedir ao Claude Code para cruzar informações entre elas:

```
Compare o que /domain-drift-sentiment e /few-shot-especializado dizem sobre domínios difíceis
```

---

## 📁 Estrutura do Repositório

```
academic-to-skill/
├── SKILL.md              # Definição da skill + instruções passo a passo
├── scripts/
│   └── extract.py        # Extração de PDF + EPUB (pdftotext / PyPDF2 / pdfminer / ebooklib)
└── README.md             # Este arquivo
```

---

## Créditos

Este projeto é um fork acadêmico do projeto [book-to-skill](https://github.com/virgiliojr94/book-to-skill) criado por [virgiliojr94](https://github.com/virgiliojr94), licenciado sob MIT.

As funcionalidades acadêmicas (`references.md`, `methodology.md`, `key-findings.md`, `research-gaps.md`) foram adicionadas para atender às necessidades de pesquisadores e acadêmicos brasileiros.

## Licença

MIT