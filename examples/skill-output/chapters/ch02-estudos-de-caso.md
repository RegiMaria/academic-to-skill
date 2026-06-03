# Seção 2: Estudos de Caso e Exemplos Ilustrativos

## Ideia Central
Demonstra o MVP empiricamente com dois estudos de caso reais e três exemplos ilustrativos, mostrando que variabilidade e inconsistência são desafios sistemáticos, não anomalias ocasionais.

## Hipótese ou Questão de Pesquisa
O MVP é mensurável e quantificável em cenários reais? Como se manifesta em diferentes configurações de modelos e tarefas?

## Principais Conceitos Introduzidos
- **TripR-2020Large**: dataset com 474 avaliações reais de restaurantes de 132 usuários do TripAdvisor — usado para quantificar variabilidade
- **Temperature = 0.0 vs 1.0**: mesmo com temperatura zero, 29% das execuções ainda divergem da moda; temperatura não elimina MVP
- **TAR@N (Total Agreement Rate at N)**: métrica que mede percentual de execuções repetidas que concordam com o rótulo mais frequente
- **Inconsistência score/label**: o mesmo modelo atribui score alto (>0.6) mas label "negativo" para a mesma entrada — incoerência interna

## Evidências Empíricas Centrais

### Estudo de Caso 1 — GPT-4o, dataset TripR-2020Large
- **Setup**: mesma avaliação repetida 100 vezes, via API OpenAI, temperatura 1.0 e 0.0
- **Resultado com T=1.0**: polaridade oscila entre 0.3 (negativo) e 0.6 (positivo) — 63% das saídas em 0.4
- **Resultado com T=0.0**: 71% colapsa em 0.4, mas extremos (0.3 e 0.6) ainda ocorrem
- **Conclusão**: temperatura zero reduz variância mas não elimina MVP; range de oscilação permanece idêntico

### Estudo de Caso 2 — Mixtral 8x22B, restaurante "The Wolseley"
- **Setup**: avaliação de todas as opiniões com dois prompts separados (score numérico vs. label categórico)
- **Resultado**: inconsistência sistemática entre score e label — avaliações classificadas como "negativo" com score > 0.6
- **Hardware**: 4× H100 80GB; parâmetros padrão (T=1.0, top-p=1.0, top-k=40)

### Exemplo Financeiro (Hipotético/Reproduzível)
- **Headline**: "Central bank hints at surprise rate cut next quarter"
- **Setup**: GPT-4O, 100 execuções, T=1.0
- **Resultado**: scores 0.40–0.80, média 0.67 — mesma headline dispara "sell", "hold" e "buy" dependendo da execução
- **Implicação**: exposição intraday a risco e possível atenção regulatória

## Limitações Desta Seção
Estudo de caso 1 usa apenas GPT-4o; estudo de caso 2 usa apenas Mixtral. Exemplos financeiros são hipotéticos, não baseados em dados reais de trading.

## Conecta Com
- **Seção 3.2**: temperatura como driver de variabilidade — aprofunda o mecanismo por trás dos resultados aqui
- **Seção 5.7**: reprodutibilidade e estabilidade — desafio operacional derivado destas evidências
- **Seção 3.3**: stochasticity de inferência e mecanismos de sampling
