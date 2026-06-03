# Cheatsheet — MVP em LLMs para Análise de Sentimentos

## Tabela 1: Temperatura vs. Contexto de Uso

| Temperatura | Comportamento | Use quando | Risco |
|-------------|--------------|------------|-------|
| T = 0.0 | ~determinístico | Auditoria, finanças, compliance | Amplifica erros de alta confiança |
| T = 0.1–0.3 | Muito consistente | Alta-stakes com alguma variação | Menor diversidade de resposta |
| T = 0.7 | Balanceado | Análise exploratória | Variância significativa (Beigi et al.) |
| T = 1.0 | Estocástico padrão | Benchmarking de variabilidade | MVP máximo |
| T > 1.0 | Criativo | Geração de dados sintéticos | Nunca em classificação de sentimento |

## Tabela 2: Mecanismos de Pooling vs. Trade-offs

| Pooling | Estabilidade | Interpretabilidade | Quando usar |
|---------|-------------|-------------------|-------------|
| Mean | Alta | Média | Sentimento geral, evitar extremos |
| Max | Baixa | Baixa | Detectar features dominantes |
| Weighted sum | Média | Baixa | Máxima precisão + aceitar custo de interpretabilidade |
| Mean + Weighted sum (híbrido) | Alta | Média | Equilíbrio consistência + profundidade |

## Tabela 3: Técnicas de UQ por Custo/Benefício

| Técnica | Custo | Tipo de incerteza | Requer retreino? |
|---------|-------|-------------------|-----------------|
| Temperature scaling | Muito baixo | Calibração | Não |
| Isotonic regression | Baixo | Calibração | Não |
| Monte Carlo Dropout | Médio (N× inferência) | Epistêmica | Não (dropout já presente) |
| Ensemble averaging | Alto (N modelos) | Ambas | Não (modelos independentes) |
| Bayesian deep learning | Muito alto | Ambas | Sim |

## Tabela 4: Mapa Causa Raiz → Solução Prioritária

| Causa (Seção 3) | Solução Prioritária |
|-----------------|---------------------|
| Temperatura alta (3.2) | T calibration curves + multi-sample aggregation |
| Stochasticidade de sampling (3.3) | Ensemble + TAR@N como métrica |
| Sensibilidade a prompt (3.6) | Templates padronizados + decomposição de incerteza |
| Domain drift (3.7) | Fine-tuning específico de domínio + híbrido léxico+LLM |
| RLHF (3.8) | Confidence-aware RLHF + ensemble de checkpoints |
| Miscalibração (3.10) | Temperature scaling + isotonic regression |
| Black-box (3.12) | SHAP/LIME + circuit extraction + atenção |
| Instabilidade espectral (3.5) | WeightWatcher + α-Shatten norm monitoring |

## Tabela 5: Diagnóstico Rápido de MVP

| Sintoma | Causa Provável | Diagnóstico | Ação |
|---------|---------------|-------------|------|
| Score varia ±0.3 mesmo T=0 | Inferência estocástica residual | TAR@N < 70% | Multi-sample + ensemble |
| Score e label divergem | Prompt sensitivity | Teste prompt variations | Padronizar template |
| Modelo performa bem em benchmark A, mal em B | Leaderboard discrepancy | Cross-benchmark validation | Stability-aware metrics |
| Fine-tuned model regride | Fine-tuning multiplicity | Prediction consistency metric | Progressive fine-tuning |
| Usuário não confia no output | Miscalibração + black-box | Calibration error + SHAP | Temperature scaling + LIME |

## Decisão Rápida: Qual técnica de XAI usar?

```
Preciso explicar UMA predição específica?
  → SHAP ou LIME (local)

Preciso entender comportamento GERAL do modelo?
  → Mechanistic interpretability / circuit extraction (global)

Não tenho dados de teste anotados?
  → WeightWatcher + α-Shatten norm (espectral)

Preciso explicar para usuário final (não técnico)?
  → Attention visualization + rationale-augmented output

Domínio de alto risco sem ground truth?
  → WeightWatcher (global) + circuit tracing (local) = full-stack
```
