# Metodologia — An overview of model uncertainty and variability in LLM-based sentiment analysis

> Gerado em português. Extraído das seções de metodologia e estudos de caso do artigo.
> Use para comparar abordagens entre artigos ou para replicar os experimentos.

---

## Natureza do Artigo

Este é um **artigo de survey/perspective** — não apresenta uma metodologia experimental única, mas combina análise de literatura com dois estudos de caso empíricos próprios. O núcleo metodológico está nos estudos de caso da Seção 2.

---

## Estudo de Caso 1 — Variabilidade de Inferência Repetida

### Dataset
- **Nome**: TripR-2020Large
- **Tamanho**: 474 avaliações escritas em inglês
- **Fonte**: GitHub — `https://github.com/ari-dasci/OD-TripR-2020Large`
- **Período**: coletado aproximadamente 2020 (dataset de 2021)
- **Características**: reviews de 132 usuários do TripAdvisor sobre 4 restaurantes em Londres: The Oxo Tower, The Wolseley, The Ivy, J. Sheekey

### Modelo Testado
- **Modelo principal**: GPT-4o
- **Acesso**: API OpenAI
- **Parâmetros**: top-p = 1.0; outros parâmetros padrão da API; seed em Beta (mantido padrão); top-k não exposto pela API

### Configuração Experimental
- **Número de execuções**: 100 execuções da mesma avaliação
- **Variação de temperatura**: T=1.0 (estocástico) vs. T=0.0 (quase-determinístico)
- **Divisão**: não aplicável (avaliação única repetida N vezes)
- **Prompt utilizado**:
  > "Rate the sentiment of this review on a continuous scale from 0 to 1, where 0 means entirely negative, and 1 means entirely positive. The answer must be only a number: [DOCUMENT]"

### Métricas de Avaliação
- **Distribuição de polaridade**: histograma de scores 0–1 ao longo das 100 execuções
- **Concentração na moda**: % de execuções que colapsam para o valor mais frequente
- **Range de oscilação**: diferença entre score mínimo e máximo observados

---

## Estudo de Caso 2 — Inconsistência Score vs. Label

### Dataset
- **Nome**: Avaliações do restaurante "The Wolseley" (subconjunto do TripR-2020Large)
- **Tamanho**: todas as opiniões do restaurante (subconjunto dos 474 reviews)

### Modelo Testado
- **Modelo principal**: Mixtral 8x22B
- **Infraestrutura**: servido localmente via llama.cpp em 4× H100 GPUs com 80 GB VRAM cada
- **Parâmetros**: temperature=1.0, top-p=1.0, top-k=40; random seed não definido (padrão)

### Configuração Experimental
- **Prompts utilizados** (dois separados, resultados combinados):
  1. > "Classify the sentiment of the following text as positive, neutral or negative, the answer must be a single label and one word: [DOCUMENT]"
  2. > "Classify the sentiment of the following text using a score between 0 and 1, where 0 represents a completely negative sentiment and 1 represents a completely positive sentiment. The answer must be only a number: [DOCUMENT]"
- **Análise**: resultados dos dois prompts emparelhados por review para visualizar inconsistências score vs. label

### Métricas de Avaliação
- **Consistência score/label**: verificação se o score numérico concorda com o label categórico
- **Histograma colorido**: barras = frequência de scores; cor = label predito — visualização de inconsistência

---

## Exemplo Hipotético Reproduzível — Cenário Financeiro

### Setup
- **Headline**: "Central bank hints at surprise rate cut next quarter"
- **Modelo**: GPT-4O
- **Execuções**: 100 via API OpenAI, parâmetros idênticos ao Estudo de Caso 1 (T=1.0, top-p=1.0)
- **Regra de trading hipotética**: sell < 0.4 | hold 0.4–0.6 | buy > 0.6

### Resultados
- Range de scores: 0.40–0.80
- Média: 0.67
- Implicação: mesma headline dispara ações "hold" e "buy" dependendo da execução

---

## Métricas de Avaliação Propostas (não aplicadas no paper, mas definidas)

| Métrica | Definição | Origem |
|---------|-----------|--------|
| TARr@N | % execuções que concordam com rótulo mais frequente | Atil et al., 2024 |
| TARa@N | % execuções que concordam com média | Atil et al., 2024 |
| Stability index | % de N execuções que concordam com a moda | Ye et al., 2024 |
| Entropy-based confidence | Entropia baixa = alta confiança (baseado em distribuição de N execuções) | Ye et al., 2024 |

---

## Limitações Metodológicas Declaradas

- Estudo de caso 1 usa apenas GPT-4o; estudo de caso 2 usa apenas Mixtral 8x22B — generalização limitada
- Experimento financeiro é hipotético, não baseado em dados reais de mercado
- Top-k não é exposto pela API OpenAI — não pode ser controlado experimentalmente
- Seed em Beta na API — comportamento determinístico não garantido mesmo com T=0

---

## Checklist de Replicação

- [ ] **Dataset**: download do TripR-2020Large em `https://github.com/ari-dasci/OD-TripR-2020Large`
- [ ] **Modelo EC1**: acesso à API OpenAI com modelo GPT-4o
- [ ] **Modelo EC2**: instalar llama.cpp + baixar Mixtral 8x22B weights + 4× GPUs com ≥80GB VRAM
- [ ] **Configuração EC1**: top-p=1.0, temperature=[0.0, 1.0]; outros parâmetros padrão
- [ ] **Configuração EC2**: temperature=1.0, top-p=1.0, top-k=40
- [ ] **Prompts**: usar exatamente os prompts citados na Seção 2.1 e 2.2
- [ ] **N execuções**: ≥100 repetições da mesma entrada
- [ ] **Métricas**: calcular distribuição de scores + % concentração na moda + range de oscilação
