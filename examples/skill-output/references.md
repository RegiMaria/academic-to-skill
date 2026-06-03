# Referências — An overview of model uncertainty and variability in LLM-based sentiment analysis

> Gerado em português. As referências mais citadas no texto, com título completo e contexto de uso.
> Ordenadas por número de citações (estimado a partir das ocorrências no texto).

---

## [Beigi et al., 2024] — Rethinking the Uncertainty: A Critical Review and Analysis in the Era of Large Language Models

**Citada**: ~8 vezes | **Seções**: 3.1, 3.2, 3.4, 3.8, 3.10, 3.12, 5.3, 5.6, 5.9, 5.14

**Contexto de uso**:
Referência central do artigo para fundamentar múltiplas dimensões do MVP. Citada ao tratar de incerteza causada por slang e domain shifts (3.1), variância de sentimento em modelos como GPT-3.5 e LLaMA a T=0.7 (3.2), label bias em dados de treino (3.4), e mecanismos de alinhamento que distorcem interpretações de sentimento (3.8). Também citada em calibração de confiança (3.10) e como evidência da falta de mecanismos de calibração por incerteza nos LLMs.

---

## [van der Veen and Bleich, 2025] — The Advantages of Lexicon-based Sentiment Analysis in an Age of Machine Learning

**Citada**: 4 vezes | **Seções**: 3.7, 3.12, 5.9, 5.14

**Contexto de uso**:
Usada como contraponto: modelos baseados em léxico são mais estáveis em ambientes estruturados que LLMs. Citada para demonstrar que domain drift (3.7) faz LLMs mais instáveis que abordagens determinísticas em finanças, saúde e direito. Também usada para contrastar a falta de interpretabilidade dos LLMs (3.12) com a transparência de mapeamentos léxicos explícitos.

---

## [Reveilhac and Morselli, 2024] — ChatGPT as a Voting Application in Direct Democracy

**Citada**: 4 vezes | **Seções**: 3.1, 3.6, 5.2, 5.3, 5.9

**Contexto de uso**:
Estudo empírico sobre como LLMs como ChatGPT exibem instabilidade em tomada de decisão quando o posicionamento ideológico dos prompts varia. Citada para evidenciar que incerteza epistêmica afeta sistemas de decision-making com LLMs (3.1), que LLMs mudam comportamento baseados em cues culturais e linguísticos do prompt (3.6), e que GPT-3.5 e GPT-4 divergem em posicionamento ideológico — implicando problemas de reprodutibilidade longitudinal (5.9).

---

## [Passerini et al., 2025] — Fostering Effective Hybrid Human-LLM Reasoning and Decision Making

**Citada**: 4 vezes | **Seções**: 3.1, 3.9, 5.3, 5.8

**Contexto de uso**:
Fundamenta como a interação humano-IA pode amplificar ou mitigar incerteza. Citada para mostrar que usuários que confiam sistematicamente em LLMs amplificam vieses do modelo (3.9), e que a adaptação mútua humano-LLM pode distorcer a objetividade da análise de sentimentos ao longo do tempo (5.8). Também usada como evidência de que a interação humana pode reduzir ou amplificar incerteza epistêmica dependendo da qualidade dos dados de treino.

---

## [Herrera, 2025] — Reflections and Attentiveness on eXplainable Artificial Intelligence (XAI)

**Citada**: 4 vezes | **Seções**: 4.1, 4.2, 4.3

**Contexto de uso**:
Referência filosófica e técnica central para a Seção 4. Citada para argumentar que XAI não é apenas desafio técnico, mas empreendimento centrado no humano e filosófico, essencial para interação responsável humano-IA (4.1). Também para embasar a urgência de frameworks de explicabilidade que vão além da transparência abstrata para fornecer orientação prática e contextualizada por tipo de usuário.

---

## [Ye et al., 2024] — Benchmarking LLMs via Uncertainty Quantification

**Citada**: 3 vezes | **Seções**: 3.3, 3.4, 3.11, 5.1, 5.6

**Contexto de uso**:
Estudo empírico que demonstra que ranking de LLMs em um benchmark não se transfere para outros (leaderboard discrepancy). Citada para evidenciar que stochasticity do sampling afeta classificações de sentimento (3.3), que modelos maiores têm maior variância com expressões ambíguas (3.4), e que benchmarks tradicionais não capturam instabilidade. Propõe métricas de estabilidade como base para o que os autores chamam de "stability-aware benchmarking".

---

## [Wankhade et al., 2022] — A Survey on Sentiment Analysis Methods, Applications, and Challenges

**Citada**: 3 vezes | **Seções**: 1, 3.1, 6

**Contexto de uso**:
Referência fundamental que conecta o MVP à literatura clássica de análise de sentimentos. Citada na introdução para estabelecer que challenges históricos (ambiguidade, sarcasmo, domain-specificity) são precursores do MVP moderno em LLMs. O artigo usa esta referência para argumentar que soluções para MVP devem integrar insights de métodos tradicionais e deep learning.

---

## [Hamman et al., 2024] — Quantifying Prediction Consistency Under Model Multiplicity in Tabular LLMs

**Citada**: 3 vezes | **Seções**: 3.8, 5.6

**Contexto de uso**:
Formaliza o conceito de "fine-tuning multiplicity": múltiplos modelos igualmente performáticos geram classificações conflitantes por diferenças sutis de configuração de treino. Citada para demonstrar que RLHF introduz instabilidade imprevisível (3.8) e que variações de seed, learning rate e hiperparâmetros geram divergências significativas em predições de sentimento. Propõe "prediction consistency measure" como métrica de robustez.

---

## [Martin et al., 2021] — Predicting Trends in the Quality of State-of-the-Art Neural Networks Without Access to Training or Testing Data

**Citada**: 3 vezes | **Seções**: 3.5, 4.3, 5.4

**Contexto de uso**:
Base do framework de diagnóstico espectral sem ground truth. Citada para introduzir Heavy-Tailed Self-Regularization como assinatura de modelos bem-treinados (3.5), e para apresentar weighted-α e α-Shatten norm como métricas práticas de qualidade de modelo que não requerem dados de teste (5.4). Fundamental para o argumento de que é possível diagnosticar instabilidade do MVP sem anotações.

---

## [Zhang et al., 2024] — Sentiment Analysis in the Era of Large Language Models: A Reality Check

**Citada**: ~3 vezes | **Seções**: 1, 3.4, 3.6, 3.7, 3.11

**Contexto de uso**:
"Reality check" empírico sobre capacidades reais de LLMs em análise de sentimentos. Citada para demonstrar que polaridade de sentimento oscila significativamente com mudanças de prompt (3.6), que modelos de datasets mistos têm maior variabilidade em campos especializados (3.7), e que GPT-3.5 e GPT-4 divergem em classificações por diferenças arquiteturais (3.4).

---

## [Herrera-Poyatos et al., 2025] — LLM-based Sentiment Analysis with Crowd Decision Making Using Prompt Design Strategies

**Citada**: 2 vezes | **Seções**: 1, 5.12

**Contexto de uso**:
Paper dos próprios autores que motiva este trabalho. Demonstra que abordagens prompt-based para ESA-CDM são eficazes mas expõem o sistema a MVP por sensibilidade de prompt e inferência estocástica. Citada para fundamentar o contexto de crowd decision-making como domínio de alta importância do MVP (1), e como referência para os desafios de consistência em ESA-CDM (5.12).

---

## [Krugmann and Hartmann, 2024] — Sentiment Analysis in the Age of Generative AI

**Citada**: 2 vezes | **Seções**: 3.4, 3.6, 3.11, 5.2, 5.9

**Contexto de uso**:
Analisa como prompts mais estruturados e específicos geram maior consistência que prompts vagos em GPT-3.5 e GPT-4. Também demonstra que GPT-3.5 exibe viés positivo enquanto GPT-4 tende a neutro/negativo — evidência de version-dependent bias (5.9). Citada para criticar benchmarks de sentimento que falham em capturar transições sutis e dependências contextuais (3.11).

---

## [Loya et al., 2023] — Exploring the Sensitivity of LLMs' Decision-Making Capabilities

**Citada**: 2 vezes | **Seções**: 3.3, 3.6, 5.1

**Contexto de uso**:
Demonstra que mesmo com configurações de inferência fixas, variações mínimas de prompt causam diferentes classificações de sentimento — a variabilidade não é apenas função de temperatura e top-k, mas também de dependências contextuais de prompt (3.3). Usada para validar que sensibilidade de prompt é causa independente de MVP além da stochasticidade de sampling.

---

## [Kweon et al., 2025] — Uncertainty Quantification and Decomposition for LLM-based Recommendation

**Citada**: 2 vezes | **Seções**: 3.6, 5.6

**Contexto de uso**:
Propõe decomposição de incerteza em dois componentes: "recommendation uncertainty" (ambiguidade intrínseca da tarefa) e "prompt uncertainty" (variabilidade específica de formulação). Citada para mostrar que prompt sensitivity é uma fonte quantificável e separável de variabilidade nos outputs de LLMs — base para estratégias de padronização de prompts.

---

## [Da et al., 2025] — Understanding the Uncertainty of LLM Explanations: A Perspective Based on Reasoning Topology

**Citada**: 2 vezes | **Seções**: 4.3, 5.14

**Contexto de uso**:
Introduce "reasoning topology" — framework que modela o processo de raciocínio do LLM como estrutura de dependências lógicas. Diferentes caminhos de raciocínio para a mesma conclusão geram incerteza estrutural que se manifesta como variabilidade em análise de sentimentos. Citada para propor quantificação estruturada de incerteza baseada em explicações — uma forma de tornar a inconsistência do MVP mais diagnosticável e mitigável.

---

## [Zhao et al., 2024] — Explainability for Large Language Models: A Survey

**Citada**: 2 vezes | **Seções**: 4.1, 4.2

**Contexto de uso**:
Taxonomia abrangente de técnicas de XAI para LLMs, categorizadas em explicações locais (feature attribution, attention, counterfactuals) e globais (mechanistic interpretability, representation analysis). Citada para estruturar o framework de XAI da Seção 4 e para argumentar que decisões opacas em LLMs levam a viés não intencional, geração de conteúdo prejudicial e alucinações.

---

## [Ameisen et al., 2025] — Circuit Tracing: Revealing Computational Graphs in Language Models

**Citada**: 2 vezes | **Seções**: 4.3, 5.4

**Contexto de uso**:
Paper da Anthropic que operacionaliza mechanistic interpretability via attribution graphs e circuit tracing em modelos de linguagem reais. Citada para apresentar as técnicas de MI como ferramentas práticas para desvendar caminhos causais de decisão em LLMs — complemento às ferramentas espectrais de Martin et al. na construção de um framework de explicabilidade full-stack.

---

## [Arrieta et al., 2020] — Explainable Artificial Intelligence (XAI): Concepts, Taxonomies, Opportunities and Challenges

**Citada**: ~2 vezes | **Seções**: 4.1, 4.2

**Contexto de uso**:
Referência fundacional de XAI que define o conceito: "dado um audiência, um sistema de IA explicável produz detalhes ou razões que tornam seu funcionamento claro ou fácil de entender." Citada para estabelecer a base conceitual de explicabilidade — audiência + compreensibilidade — que orienta toda a análise da Seção 4.

---

## [Shorinwa et al., 2024] — A Survey on Uncertainty Quantification of Large Language Models

**Citada**: ~2 vezes | **Seções**: 3.1, 3.4, 3.8, 5.9

**Contexto de uso**:
Survey abrangente sobre técnicas de UQ para LLMs. Citada para evidenciar que ruído em dados (anotações inconsistentes, ambiguidade linguística) é fonte primária de incerteza aleatória em análise de sentimentos (3.1), e que RLHF models tendem a deslocar predições imprevisívelmente com o feedback humano subjetivo (3.8). Também citada para fundamentar adversarial debiasing como estratégia de mitigação de viés.

---

## [Atil et al., 2024] — LLM Stability: A Detailed Analysis with Some Surprises

**Citada**: ~2 vezes | **Seções**: 3.3, 3.8, 5.6

**Contexto de uso**:
Demonstra empiricamente que LLMs exibem flutuações de acurácia de até 10% entre execuções idênticas, mesmo com configurações determinísticas — um resultado surpreendente que nomeia o paper. Propõe TAR@N (TARr@N e TARa@N) como métricas específicas de estabilidade. Também mostra que sequências mais longas correlacionam negativamente com estabilidade — modelos que geram explicações verbosas são mais variáveis.
