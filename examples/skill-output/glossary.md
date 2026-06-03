# Glossário — Herrera-Poyatos et al. (2025)

**α-Shatten norm** — métrica escalar que quantifica a saúde espectral de uma camada de rede neural; valores α > 2.3 sinalizam potencial instabilidade ou overfitting (Seção 3.5, 4.3)

**Algorithm aversion** — tendência de usuários de rejeitar outputs de IA após experimentar erros, mesmo quando o modelo está correto; introduz variabilidade na pipeline humano-IA (Seção 3.9)

**Aleatoric uncertainty** — incerteza inerente aos dados causada por ambiguidade, sarcasmo, expressões mistas e anotações inconsistentes; não pode ser eliminada com mais treino (Seção 3.1)

**Attention visualization** — técnica de XAI que mapeia quais tokens do input mais influenciam a predição de sentimento; aplicável a LLMs para diagnóstico de variabilidade (Seção 4.2)

**Automation bias** — tendência de usuários de aceitar outputs de IA sem escrutínio crítico; amplifica erros sistemáticos do modelo (Seção 3.9)

**Bayesian deep learning** — framework que incorpora distribuições de probabilidade sobre pesos do modelo para quantificar incerteza epistêmica (Seção 5.3)

**Calibração de confiança** — alinhamento entre o nível de confiança predito pelo modelo e sua acurácia real; LLMs tendem a ser miscalibrados — superconfiantes em erros (Seção 3.10)

**Circuit extraction** — técnica de mechanistic interpretability que isola subnetworks compactas responsáveis por comportamentos específicos sem retreinamento (Seção 4.3)

**CompactifAI** — framework de compressão de LLMs usando redes tensoriais inspiradas em computação quântica; pode amplificar variabilidade se não controlado (Seção 5.13)

**Confidence calibration** — ver Calibração de confiança

**Domain drift** — falha de generalização de LLMs de propósito geral para vocabulário e semântica específicos de finanças, saúde ou direito (Seção 3.7)

**Epistemic uncertainty** — incerteza causada por lacunas no conhecimento do modelo — dados de treino insuficientes, underrepresentation de domínios; redutível com mais dados (Seção 3.1)

**ESA-CDM (Enhanced Sentiment Analysis – Crowd Decision Making)** — análise de sentimentos aplicada a tomada de decisão coletiva com LLMs atuando como proxies de opinião (Seção 1)

**Fine-tuning multiplicity** — fenômeno em que múltiplos modelos treinados com leves variações de configuração geram classificações conflitantes para a mesma entrada (Seção 3.8)

**Heavy-Tailed Self-Regularization** — propriedade de modelos bem-treinados de desenvolver distribuições de valores singulares com cauda pesada; indica boa regularização implícita (Seção 3.5)

**Isotonic regression** — técnica de calibração pós-treino que mapeia scores de confiança do modelo para probabilidades empíricas sem assumir forma funcional (Seção 5.9)

**Knowledge distillation** — técnica onde um modelo "student" menor aprende de um modelo "teacher" maior; desafios incluem transferência de incerteza e preservação de nuança (Seção 5.11)

**LIME (Local Interpretable Model-Agnostic Explanations)** — método de XAI que explica predições individuais com modelos lineares locais; escalabilidade limitada em LLMs grandes (Seção 3.12)

**Mechanistic interpretability (MI)** — campo que reverse-engineer circuitos causais internos de LLMs via activation patching, circuit tracing e attribution graphs (Seção 4.3)

**MiniPLM** — framework de knowledge distillation que refina distribuição de treino do student usando insight do teacher, melhorando performance com menor complexidade (Seção 5.11)

**Monte Carlo dropout** — técnica de UQ que ativa dropout durante inferência para amostrar distribuição de predições, estimando incerteza epistêmica (Seção 5.3)

**MVP (Model Variability Problem)** — fenômeno central do artigo: LLM produz saídas inconsistentes para a mesma entrada em múltiplas execuções (Seção 1)

**Pooling mechanism** — método de agregação de embeddings de tokens em representação de sentença: mean pooling (estável, dilui extremos), max pooling (captura features fortes, mais variância), weighted sum (preciso, difícil de interpretar) (Seção 3.12)

**Polaridade de sentimento** — score contínuo 0–1 onde 0 = totalmente negativo e 1 = totalmente positivo (Seção 2.1)

**Prompt sensitivity** — variação significativa na classificação de sentimento causada por pequenas mudanças na formulação do prompt, mesmo com semântica idêntica (Seção 3.6)

**Prompt uncertainty** — componente de incerteza atribuível especificamente a variações de formulação de prompt, distinto da incerteza intrínseca da tarefa (Seção 3.6)

**RLHF (Reinforcement Learning from Human Feedback)** — método de alinhamento de LLMs por feedback humano; melhora comportamento mas introduz nova variabilidade via subjetividade do feedback (Seção 3.8)

**Reasoning topology** — framework (Da et al., 2025) que decompõe o processo de raciocínio do LLM em estrutura de dependências lógicas; diferentes topologias = diferentes conclusões (Seção 4.3)

**SHAP (SHapley Additive Explanations)** — método de XAI baseado em teoria dos jogos que atribui contribuição de cada feature à predição; computacionalmente caro em LLMs grandes (Seção 3.12)

**Stability index** — métrica que mede o percentual de execuções repetidas que concordam com o rótulo mais frequente; quantifica reprodutibilidade run-to-run (Seção 5.1)

**TAR@N (Total Agreement Rate at N)** — família de métricas (TARr@N, TARa@N) que medem consistência de inferência repetida; proposta em Atil et al. (2024) (Seção 3.3)

**Temperature scaling** — técnica de calibração pós-treino que divide logits por uma temperatura aprendida antes do softmax para melhor calibrar probabilidades (Seção 3.10)

**TripR-2020Large** — dataset com 474 avaliações de restaurantes de 132 usuários do TripAdvisor; usado no Estudo de Caso 1 para demonstrar MVP (Seção 2.1)

**Uncertainty quantification (UQ)** — conjunto de técnicas para estimar e comunicar confiança em predições de ML/LLM; inclui Bayesian methods, MC dropout, ensemble averaging (Seção 1)

**WeightWatcher** — ferramenta open-source para monitorar saúde espectral de pesos de LLMs sem necessidade de dados de teste (Seção 5.4)

**weighted-α** — métrica scale-invariant de qualidade de model layer baseada em expoentes de power-law da distribuição espectral dos pesos (Seção 3.5)

**XAI (Explainable Artificial Intelligence)** — campo que desenvolve métodos para tornar decisões de IA compreensíveis para humanos; essencial para trust building e deployment responsável (Seção 4)
