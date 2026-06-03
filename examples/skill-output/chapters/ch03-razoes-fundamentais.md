# Seção 3: Doze Razões Fundamentais para o MVP

## Ideia Central
Taxonomia de 12 causas raiz que explicam por que LLMs produzem classificações de sentimento inconsistentes — da física do sampling à estrutura espectral dos pesos.

## Principais Conceitos por Razão

### 3.1 Incerteza Aleatória e Epistêmica
- **Aleatória**: ruído inerente nos dados — sarcasmo, ambiguidade, expressões mistas; requer embeddings contextuais avançados
- **Epistêmica**: lacunas no pré-treino — modelo não generalizou para vocabulário específico; requer fine-tuning e data augmentation

### 3.2 Temperatura como Driver de Variabilidade
- Temperatura escala os logits antes do softmax — controla o quão determinístico é o sampling
- **T baixa (0.1–0.3)**: mais determinístico, menor variância; **T alta (0.8–1.5)**: mais criativo, mais variância
- **Regra prática**: mesmo T=0.7 causa variância significativa em polaridade, justificativas e fidelidade factual (Beigi et al., 2024)
- MVP piora com interação entre temperatura e sensibilidade de prompt: pequenas reformulações + T alta = trajetória de sampling radicalmente diferente

### 3.3 Stochasticidade de Inferência e Mecanismos de Sampling
- Top-k sampling e temperature scaling introduzem não-determinismo; beam search com parâmetros fixos é determinístico
- Atil et al. (2024): flutuações de acurácia de até 10% entre execuções idênticas, mesmo com configurações determinísticas
- **TAR@N (TARr@N e TARa@N)**: métricas de estabilidade que medem concordância entre execuções repetidas

### 3.4 Viés, Escala e Inconsistências Multimodais
- Modelos maiores = maior variância em expressões ambíguas (sarcasmo), apesar de melhor performance geral
- GPT-3.5 vs GPT-4: classificações divergentes para entradas idênticas por diferenças arquiteturais e de fine-tuning
- Conflito textual-visual: tweet sarcástico + imagem alegre → classificação incorreta por desalinhamento de modalidades

### 3.5 Instabilidade Espectral Implícita
- Heavy-Tailed Self-Regularization (Martin & Mahoney, 2021): modelos bem-treinados desenvolvem distribuições de valores singulares com cauda pesada
- Desvios desta assinatura espectral → instabilidade nas saídas, mesmo sem benchmark visível
- **weighted-α** e **α-Shatten norm**: métricas independentes de dados de teste para diagnosticar qualidade interna do modelo

### 3.6 Sensibilidade a Prompts
- Mesmo significado semântico, fraseamento diferente → label oscila entre positivo/neutro/negativo
- **Decomposição de incerteza** (Kweon et al., 2025): "recommendation uncertainty" (complexidade da tarefa) vs. "prompt uncertainty" (variabilidade de formulação)
- Prompts estruturados explicitamente geram maior consistência que prompts vagos (Krugmann & Hartmann, 2024)

### 3.7 Desafios Específicos de Domínio (Domain Drift)
- LLMs de propósito geral falham em vocabulário técnico de finanças, saúde e direito
- Modelos baseados em léxico são mais estáveis em ambientes estruturados com regras determinísticas
- Solução: fine-tuning adaptativo de domínio + modelos híbridos léxico+LLM

### 3.8 RLHF e Fine-Tuning por Reforço
- RLHF melhora alinhamento mas introduz imprevisibilidade: ajustes de fine-tuning por feedback humano subjetivo deslocam predições inconsistentemente
- **Fine-tuning multiplicity** (Hamman et al., 2024): múltiplos modelos igualmente performáticos geram classificações conflitantes para mesma entrada por diferenças sutis de configuração de treino

### 3.9 Vieses de Interação Humano-AI
- **Automation bias**: usuário aceita saída do modelo sem escrutínio → amplifica erros sistemáticos
- **Algorithm aversion**: usuário reverte decisão do modelo mesmo quando correto → introduz instabilidade na pipeline

### 3.10 Falta de Calibração nos Scores de Confiança
- LLMs frequentemente superestimam confiança em predições incorretas e subestimam em corretas
- Miscalibração → scores de sentimento instáveis entre execuções + menor confiança na aplicação
- Solução: temperature scaling, ajustes Bayesianos de confiança, métodos baseados em quantis

### 3.11 Limitações de Métricas e Benchmarks
- F1, precision, recall não capturam variabilidade entre execuções — medem performance estática
- Leaderboard discrepancy: modelo top em um benchmark pode ser mediano em outro (Ye et al., 2024)
- Necessidade de métricas de estabilidade + avaliação por incerteza

### 3.12 Natureza Black-Box da Tomada de Decisão
- Diferentes mecanismos de pooling (mean, max, weighted sum) geram variâncias distintas na classificação
- SHAP e LIME: aplicáveis a análise de sentimentos para clarificar contribuição individual de palavras
- Pooling híbrido (mean + weighted sum): maior consistência preservando profundidade contextual

## Conecta Com
- **Seção 4**: explainability como resposta ao problema black-box (razão 3.12)
- **Seção 5**: cada razão mapeia para 1+ desafios específicos com soluções potenciais
- **Seção 2**: razões 3.2 e 3.3 explicam os resultados empíricos dos estudos de caso
