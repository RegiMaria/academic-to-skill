# Seção 4: Análise do Papel da Explicabilidade

## Ideia Central
XAI é componente essencial para endereçar o MVP — não apenas como ferramenta técnica, mas como infraestrutura necessária para confiança, auditoria e implantação responsável de LLMs em domínios críticos.

## Hipótese ou Questão de Pesquisa
Como frameworks de XAI podem tornar os LLMs mais transparentes, estáveis e confiáveis em análise de sentimentos?

## Principais Conceitos Introduzidos
- **XAI (Explainable AI)**: "dado um audiência, um sistema de IA explicável produz detalhes ou razões que tornam seu funcionamento claro ou fácil de entender" (Arrieta et al., 2020)
- **Explicações locais**: justificam predições específicas — feature attribution, attention visualization, counterfactual explanations
- **Explicações globais**: explicam comportamentos gerais do modelo — mechanistic interpretability, representation analysis, classifier investigation
- **Heavy-Tailed Self-Regularization**: modelos bem-treinados desenvolvem distribuições de valores singulares com cauda pesada; desvios sinalizam instabilidade interna
- **Mechanistic Interpretability (MI)**: reverse-engineering de circuitos internos e caminhos causais em transformers — activation patching, circuit tracing
- **Topologia de raciocínio**: estrutura dos passos lógicos do LLM; diferentes caminhos de raciocínio para a mesma conclusão introduzem incerteza estrutural

## Frameworks ou Taxonomias

### Taxonomia de XAI em LLMs (Zhao et al., 2024)
- **Quando usar**: ao escolher técnica de explicabilidade para diagnóstico ou auditoria
- **Local** (caso a caso): SHAP, LIME, attention visualization, counterfactuals — mostra quais tokens influenciam a predição
- **Global** (modelo inteiro): mechanistic interpretability, análise espectral, representações — revela padrões sistemáticos e vieses

### Framework Espectral (Martin & Mahoney, 2021)
- **Quando usar**: quando não há dados de teste anotados disponíveis (comum em domínios de alto risco)
- **Como**: medir weighted-α e α-Shatten norm nos pesos do modelo; α > 2.3 sinaliza potencial instabilidade
- **Trade-off**: diagnóstico global sem precisar de ground truth, mas não revela circuitos causais específicos

### Interpretabilidade Mecanicista (García-Carrasco et al., 2024, 2025)
- **Quando usar**: ao isolar subnetworks responsáveis por uma tarefa específica (ex: detecção de sentimento)
- **Como**: circuit extraction — isola redes compactas sem retrainamento; attribution graphs traçam caminhos causais
- **Benefício adicional**: reduz custo de inferência ao podar componentes irrelevantes

## Resultados ou Argumentos Centrais
- SHAP e LIME: valiosos mas escalabilidade limitada em modelos com bilhões de parâmetros
- Atenção + teoria de gramática de construção (Weissweiler et al., 2023): esclarece como LLMs internalizam padrões linguísticos complexos
- Nguyen et al. (2024): rationale-augmented sentiment classification melhora tanto interpretabilidade quanto performance em healthcare
- Integração espectral (global) + mechanistic interpretability (local) = "full-stack explainability architecture"

## Limitações Desta Seção
Análise qualitativa e conceitual; sem experimentos próprios sobre XAI. Métricas de explicabilidade existentes ainda falham em capturar in-context learning e chain-of-thought reasoning.

## Conecta Com
- **Seção 3.12**: black-box nature — esta seção apresenta as ferramentas para endereçá-la
- **Seção 5**: desafios 5.3 e 5.4 aplicam diretamente os frameworks de XAI apresentados aqui
- **Seção 6**: integração de XAI como requisito para deployment responsável
