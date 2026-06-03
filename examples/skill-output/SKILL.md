---
name: herrera-llm-variability
description: "Base de conhecimento de \"An overview of model uncertainty and variability in LLM-based sentiment analysis\" por Herrera-Poyatos et al. (2025). Use quando raciocinar sobre MVP (Model Variability Problem), confiabilidade de LLMs, sensibilidade a prompts, variabilidade de análise de sentimentos, calibração de modelos, quantificação de incerteza (UQ), explicabilidade (XAI) e estratégias de mitigação para deployment responsável de LLMs em domínios de alto risco."
allowed-tools:
  - Read
  - Grep
argument-hint: [tópico, framework, número de seção, ou 'metodologia' / 'descobertas' / 'referencias' / 'lacunas']
---

# An overview of model uncertainty and variability in LLM-based sentiment analysis
**Autores**: David Herrera-Poyatos, Carlos Peláez-González, Cristina Zuheros, Andrés Herrera-Poyatos, Virilo Tejedor, Francisco Herrera, Rosana Montes | **Páginas**: 24 | **Seções**: 6 principais | **Gerado em**: 2026-06-02

## Como Usar Esta Skill

- **Sem argumentos** — frameworks centrais do MVP e taxonomia de causas
- **Com tópico** — perguntar sobre `temperatura`, `calibração`, `XAI`, `ensemble`, `benchmark`
- **Com seção** — pedir `sec03` para as 12 razões fundamentais, `sec05` para os 14 desafios
- **Arquivos especiais** — `metodologia` para estudos de caso, `descobertas` para resultados, `referencias` para mapa de citações, `lacunas` para oportunidades de pesquisa

---

## Frameworks Centrais e Conceitos Principais

### MVP — Model Variability Problem
O problema central: LLM produz saídas inconsistentes para a mesma entrada em múltiplas execuções. Causas: stochasticidade de inferência (temperature, top-k), sensibilidade a prompt, vieses de treino, miscalibração, fine-tuning multiplicity, RLHF, domain drift, black-box nature.

**Evidências empíricas**:
- GPT-4o, 100 execuções, T=0.0: polaridade oscila 0.3–0.6 mesmo com temperatura zero (71% em 0.4, mas extremos persistem)
- Mixtral 8x22B: score numérico e label categórico sistematicamente inconsistentes para as mesmas entradas
- LLMs genéricos: flutuações de acurácia de até 10% entre execuções supostamente idênticas (Atil et al., 2024)

### Taxonomia de Causas — 12 Razões Fundamentais (Seção 3)
| # | Causa | Fator principal |
|---|-------|----------------|
| 3.1 | Incerteza aleatória | Ambiguidade nos dados |
| 3.2 | Incerteza epistêmica | Lacunas no modelo |
| 3.2 | Temperatura | Driver direto de variância |
| 3.3 | Stochasticidade de sampling | top-k, temperature scaling |
| 3.4 | Viés e escala | Label bias + model size |
| 3.5 | Instabilidade espectral | Desvio do Heavy-Tailed SR |
| 3.6 | Sensibilidade a prompt | Fraseamento → label change |
| 3.7 | Domain drift | Vocabulário especializado |
| 3.8 | RLHF | Alinhamento introduce variabilidade |
| 3.9 | Interação humano-IA | Automation bias / aversion |
| 3.10 | Miscalibração | Confiança ≠ acurácia |
| 3.11 | Métricas insuficientes | F1 não captura instabilidade |
| 3.12 | Black-box | Pooling + opacidade |

### Framework de Temperatura — Regras Práticas
- **T=0.0**: reduz variância mas NÃO elimina MVP; use para auditoria e compliance
- **T=0.3–0.7**: equilíbrio para análise exploratória
- **T=1.0**: referência para benchmarking de variabilidade
- **Multi-sample aggregation**: amostrar com T variadas + majority vote = melhor robustez
- **Interação crítica**: T alta + prompt sensitivity = MVP máximo

### Framework de XAI para MVP (Seção 4)
Dois eixos complementares:
1. **Diagnóstico espectral** (global, sem ground truth): WeightWatcher + α-Shatten norm + weighted-α → detecta modelos brittle antes do deploy; α > 2.3 sinaliza instabilidade
2. **Mechanistic interpretability** (local, causal): circuit tracing + attribution graphs + activation patching → revela caminhos de decisão; circuit extraction isola subnetwork de sentimento sem retreinamento

Integração = "full-stack explainability architecture": espectral identifica risco global; MI valida localmente.

**Técnicas de XAI por objetivo**:
- SHAP / LIME: contribuição de tokens individuais → escala limitada em modelos grandes
- Attention visualization: quais partes do input influenciam sentimento
- Reasoning topology (Da et al., 2025): decompõe caminhos de raciocínio → quantifica incerteza estrutural
- Hybrid pooling (mean + weighted sum): consistência + profundidade contextual

### 14 Desafios e Soluções Prioritárias (Seção 5)
Top 5 mais acionáveis:

**D1 — Benchmarks sem estabilidade**: usar TAR@N + stability index + entropy-based confidence além de F1

**D2 — Prompt sensitivity**: padronizar templates + testar N variações de prompt + ensemble multi-prompt

**D4 — Sem ground truth**: WeightWatcher (diagnóstico espectral) + circuit extraction (MI)

**D7 — Reprodutibilidade**: low-T para auditoria; multi-sample aggregation para robustez; temperature calibration curves por domínio

**D10 — Ensemble consensus**: confidence-weighted aggregation ou majority vote entre N LLMs/prompts

### Calibração — Hierarquia de Técnicas
| Custo | Técnica | Quando usar |
|-------|---------|-------------|
| Mínimo | Temperature scaling | Primeiro passo sempre |
| Baixo | Isotonic regression | Quando miscalibração é não-uniforme |
| Médio | Monte Carlo Dropout | Quando incerteza epistêmica é crítica |
| Alto | Ensemble averaging | Produção de alta-stakes |
| Muito alto | Bayesian deep learning | Quando distribuição completa é necessária |

---

## Índice de Seções

| # | Título | Conteúdo Principal |
|---|--------|-------------------|
| [sec01](chapters/ch01-introducao.md) | Introdução | MVP definição, incerteza aleatória/epistêmica, ESA-CDM, XAI |
| [sec02](chapters/ch02-estudos-de-caso.md) | Estudos de Caso | TripR-2020Large, GPT-4o T=0 vs T=1, Mixtral inconsistência, exemplo financeiro |
| [sec03](chapters/ch03-razoes-fundamentais.md) | 12 Razões do MVP | Temperatura, stochasticidade, bias, RLHF, instabilidade espectral, domain drift |
| [sec04](chapters/ch04-explainability.md) | Papel da Explicabilidade | Taxonomia XAI, spectral diagnostics, mechanistic interpretability, circuit extraction |
| [sec05](chapters/ch05-desafios-e-estrategias.md) | 14 Desafios + Soluções | Benchmarking, prompt, UQ, ensemble, knowledge distillation, open-source, ESA-CDM |
| [sec06](chapters/ch06-conclusoes.md) | Conclusões | 4 contribuições originais, agenda de pesquisa futura |

## Índice de Tópicos
- **Temperatura** → sec02, sec03
- **Benchmarking / TAR@N** → sec03, sec05
- **Calibração** → sec03, sec05
- **Prompt sensitivity** → sec03, sec05
- **XAI / Explicabilidade** → sec04, sec05
- **WeightWatcher / Espectral** → sec03, sec04, sec05
- **Ensemble** → sec05
- **RLHF** → sec03, sec05
- **Domain drift** → sec03, sec05
- **Open-source LLMs** → sec05
- **ESA-CDM** → sec01, sec05
- **Knowledge distillation** → sec05
- **Fine-tuning multiplicity** → sec03, sec05

## Arquivos de Suporte
- [glossary.md](glossary.md) — 35+ termos com definições e referências de seção
- [patterns.md](patterns.md) — 12 padrões acionáveis com quando usar / como / trade-offs
- [cheatsheet.md](cheatsheet.md) — tabelas de decisão: temperatura, pooling, UQ, diagnóstico rápido

## Arquivos Acadêmicos
- [references.md](references.md) — 20 referências mais citadas com contexto de uso real
- [methodology.md](methodology.md) — estudos de caso, datasets, configurações, checklist de replicação
- [key-findings.md](key-findings.md) — resultados empíricos com números e interpretações
- [research-gaps.md](research-gaps.md) — limitações declaradas e 5 oportunidades de pesquisa derivada

---

## Escopo e Limites

Esta skill cobre apenas o conteúdo de Herrera-Poyatos et al. (2025). Para implementações específicas de calibração ou XAI, consultar os papers primários referenciados. Para comparação com outros surveys de análise de sentimentos, consultar skills relacionadas ou perguntar ao agente diretamente.
