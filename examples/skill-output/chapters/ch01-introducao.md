# Seção 1: Introdução

## Ideia Central
Apresenta o Model Variability Problem (MVP) em análise de sentimentos baseada em LLMs: a produção de saídas inconsistentes para a mesma entrada em múltiplas execuções, causada por mecanismos estocásticos de inferência, sensibilidade a prompts e vieses de treinamento.

## Hipótese ou Questão de Pesquisa
Por que LLMs produzem classificações de sentimento inconsistentes, e quais estratégias de mitigação e frameworks de explicabilidade podem torná-los confiáveis em domínios de alto risco?

## Principais Conceitos Introduzidos
- **MVP (Model Variability Problem)**: fenômeno em que um LLM produz saídas inconsistentes para a mesma entrada em múltiplas execuções
- **Incerteza aleatória**: variabilidade inerente aos dados — ambiguidade, sarcasmo, expressões mistas
- **Incerteza epistêmica**: variabilidade causada por lacunas de conhecimento no modelo — dados de treino insuficientes
- **ESA-CDM (Ensemble Sentiment Analysis – Crowd Decision Making)**: análise de sentimentos em contextos de tomada de decisão coletiva com LLMs
- **XAI (Explainable AI)**: frameworks que tornam as decisões do modelo compreensíveis para humanos
- **Polaridade de sentimento**: pontuação contínua 0–1 onde 0 = totalmente negativo e 1 = totalmente positivo

## Frameworks ou Taxonomias
- **Classificação de incerteza (aleatória vs. epistêmica)**:
  - Quando usar: ao diagnosticar a causa de instabilidade em predições de sentimento
  - Como: identificar se a fonte é ruído nos dados (aleatória) ou lacuna de conhecimento do modelo (epistêmica)

## Resultados ou Argumentos Centrais
- LLMs superam abordagens tradicionais em análise de sentimentos, mas introduzem instabilidade estocástica que classifiers determinísticos não têm
- Em domínios de alto risco (finanças, saúde, policy), mesmo variação pequena de polaridade pode gerar decisões incorretas e perdas significativas
- A solução requer: quantificação de incerteza + calibração do modelo + averaging por ensemble + explicabilidade

## Limitações Desta Seção
Introdução conceitual; não fornece evidências empíricas — estas vêm nas seções 2 e 3.

## Conecta Com
- **Seção 2**: demonstra o MVP com dois estudos de caso empíricos
- **Seção 3**: enumera as 12 razões fundamentais para o MVP
- **Seção 4**: analisa como a explicabilidade endereça o MVP
