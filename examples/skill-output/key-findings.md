# Principais Descobertas — An overview of model uncertainty and variability in LLM-based sentiment analysis

> Gerado em português. Resultados com evidências numéricas.
> Use para comparações entre artigos e para embasar afirmações em revisão de literatura.

---

## Descoberta 1 — MVP persiste mesmo com T=0.0 em GPT-4o

- **O que foi testado**: mesma avaliação do TripR-2020Large repetida 100 vezes com GPT-4o em T=1.0 e T=0.0
- **Resultado com T=1.0**: polaridade oscila entre 0.3 e 0.6; 63% das saídas colapsam em 0.4
- **Resultado com T=0.0**: 71% colapsam em 0.4; extremos (0.3 e 0.6) ainda ocorrem
- **Condições**: API OpenAI, top-p=1.0, outros parâmetros padrão
- **Significância**: resultado qualitativo (sem p-value reportado) — a redução de variância com T=0 é real mas não elimina o range de oscilação
- **Interpretação dos autores**: temperatura zero reduz variância mas não elimina MVP; o range possível de oscilação para decisões downstream permanece idêntico entre T=0 e T=1

---

## Descoberta 2 — Inconsistência sistemática entre score numérico e label categórico em Mixtral 8x22B

- **O que foi testado**: avaliações do restaurante "The Wolseley" classificadas por dois prompts distintos (score 0–1 vs. label positivo/negativo/neutro)
- **Resultado**: inconsistências sistemáticas — avaliações com label "negativo" apresentam scores > 0.6; avaliações com label "positivo" aparecem em scores baixos
- **Condições**: Mixtral 8x22B local, 4× H100 80GB, llama.cpp, T=1.0, top-p=1.0, top-k=40
- **Significância**: não reportada quantitativamente; evidência visual via histograma colorido
- **Interpretação dos autores**: variabilidade e inconsistência entre representação quantitativa e qualitativa de sentimento são desafio sistemático, não anomalia — evidência de prompt sensitivity e variabilidade contextual

---

## Descoberta 3 — Mesma headline financeira gera ações de trading conflitantes em GPT-4o

- **O que foi testado**: headline hipotética "Central bank hints at surprise rate cut next quarter" executada 100 vezes
- **Resultado**: scores 0.40–0.80, média 0.67 — a mesma headline dispara "hold" (0.4–0.6) e "buy" (>0.6) dependendo da execução
- **Condições**: GPT-4O, API OpenAI, T=1.0, top-p=1.0
- **Significância**: cenário hipotético mas reproduzível com a infraestrutura descrita
- **Interpretação dos autores**: instabilidade desta magnitude expõe mesas de trading a risco intraday significativo e potencial atenção regulatória; reportar apenas a média oculta esta vulnerabilidade crítica

---

## Descoberta 4 — Flutuações de acurácia de até 10% entre execuções idênticas (Atil et al., 2024)

- **O que foi testado**: execuções repetidas de classificação de sentimento com configurações determinísticas (T=0, seeds fixos)
- **Resultado**: variações de até 10% em acurácia entre execuções supostamente idênticas
- **Condições**: múltiplos LLMs com configurações determinísticas (terceiros, não experimento próprio do paper)
- **Significância**: não reportada pelo paper-fonte
- **Interpretação dos autores**: stochasticidade residual existe mesmo com configurações nominalmente determinísticas; outputs mais longos correlacionam negativamente com estabilidade

---

## Descoberta 5 — GPT-3.5 tem viés positivo; GPT-4 tem viés neutro/negativo (Krugmann and Hartmann, 2024)

- **O que foi testado**: comparação de classificações de sentimento entre GPT-3.5 e GPT-4 com entradas idênticas
- **Resultado**: GPT-3.5 exibe viés sistemático positivo; GPT-4 tende a neutro ou negativo — mesma entrada, polaridades diferentes
- **Condições**: mesmos inputs, diferentes versões do modelo
- **Significância**: empírico; detalhes estatísticos não reportados neste paper
- **Interpretação dos autores**: version-dependent bias compromete reprodutibilidade — estudos que mudam de GPT-3.5 para GPT-4 podem ver inversão de tendências sem nenhuma mudança nos dados

---

## Descoberta 6 — Modelos maiores têm maior variância em expressões ambíguas (Ye et al., 2024)

- **O que foi testado**: análise de variância de output em função de escala do modelo, especialmente em sarcasmo
- **Resultado**: à medida que o tamanho do modelo aumenta, a variância nos outputs de sentimento aumenta para expressões ambíguas
- **Condições**: não especificado neste paper — citação de resultado de Ye et al., 2024
- **Interpretação dos autores**: modelos maiores são mais sensíveis a phrasing e contexto, apesar de maior capacidade representacional — trade-off entre expressividade e estabilidade

---

## Tabela Resumo de Resultados

| Experimento | Modelo / Método | Métrica | Resultado | Condição |
|-------------|-----------------|---------|-----------|----------|
| EC1 — TripR, T=1.0 | GPT-4o | Range de polaridade | 0.3–0.6 | 100 execuções, T=1.0 |
| EC1 — TripR, T=0.0 | GPT-4o | Concentração na moda | 71% em 0.4 | 100 execuções, T=0.0 |
| EC1 — TripR, T=0.0 | GPT-4o | Extremos ainda presentes | Sim (0.3 e 0.6) | T=0.0 não elimina MVP |
| EC2 — The Wolseley | Mixtral 8x22B | Inconsistência score/label | Sistemática | T=1.0, dois prompts |
| Financeiro hipotético | GPT-4O | Range de scores | 0.40–0.80 | 100 exec, T=1.0 |
| Financeiro hipotético | GPT-4O | Média de score | 0.67 | Regra: sell<0.4, hold 0.4–0.6, buy>0.6 |
| Atil et al., 2024 | Múltiplos LLMs | Variação de acurácia | Até 10% | Configurações nominalmente determinísticas |

---

## O Que Este Artigo Prova

1. MVP é fenômeno **sistemático e mensurável**, não anomalia ocasional — demonstrado empiricamente em dois modelos diferentes (GPT-4o e Mixtral 8x22B) com experimentos reproduzíveis
2. **Temperatura zero não elimina MVP** — apenas reduz variância mantendo o range possível de oscilação idêntico
3. **Inconsistência score/label é forma específica de MVP** causada por prompt sensitivity: o mesmo modelo avalia sentimento de forma diferente dependendo de como a pergunta é formulada

## O Que Este Artigo NÃO Prova

- Não propõe ou valida experimentalmente nenhuma técnica de mitigação — as 14 estratégias da Seção 5 são propostas baseadas em literatura, sem experimentos próprios de validação
- Não generaliza resultados além dos modelos testados (GPT-4o, Mixtral 8x22B) — outros LLMs podem exibir padrões diferentes
- O experimento financeiro é hipotético — não demonstra impacto real em dados de mercado
- Não quantifica a redução de MVP alcançável com as estratégias propostas — fica como agenda de pesquisa futura
