# Padrões e Técnicas — Herrera-Poyatos et al. (2025)

## Padrão 1: Ensemble Multi-Prompt para Estabilidade
**Quando usar**: ao precisar de classificações de sentimento reproduzíveis com LLMs em produção  
**Como**: gerar N variações semânticas do mesmo prompt → executar cada uma → agregar por majority vote ou confidence-weighted averaging  
**Trade-offs**: custo computacional N×; melhora estabilidade mas não elimina viés sistêmico  
**Referência**: Seções 5.2, 5.10, 5.12

---

## Padrão 2: Calibração de Temperatura por Domínio
**Quando usar**: ao selecionar temperatura para tarefas de análise de sentimentos em produção  
**Como**:
- T ≈ 0.0: outputs determinísticos, auditáveis, alta-stakes (finanças, saúde) — risco: amplifica erros de alta confiança
- T = 0.3–0.7: equilíbrio entre consistência e diversidade para análise exploratória
- Multi-sample aggregation: amostrar com T variadas + majority vote para cenários que exigem robustez  
**Trade-offs**: T=0 reduz variância mas não elimina MVP; T alta aumenta criatividade mas reduz reprodutibilidade  
**Referência**: Seções 3.2, 5.7

---

## Padrão 3: Diagnóstico Espectral sem Ground Truth (WeightWatcher)
**Quando usar**: ao avaliar qualidade de LLM para deployment em domínio sem dados de teste anotados  
**Como**: instalar WeightWatcher → executar análise espectral dos pesos → verificar se α-Shatten norms estão dentro do range esperado (α < 2.3 para modelos bem regularizados)  
**Trade-offs**: diagnóstico global sem custo de anotação; não revela circuitos causais específicos  
**Referência**: Seções 3.5, 4.3, 5.4

---

## Padrão 4: Framework de UQ por Monte Carlo Dropout
**Quando usar**: ao quantificar incerteza epistêmica em predições de sentimento sem retreinamento  
**Como**: manter dropout ativo durante inferência → executar M forward passes → calcular distribuição de predições → usar entropia como proxy de incerteza  
**Trade-offs**: custo M× na inferência; fornece intervalo de confiança interpretável  
**Referência**: Seções 3.3, 5.3, 5.6

---

## Padrão 5: Avaliação de Estabilidade com TAR@N
**Quando usar**: ao construir benchmarks de avaliação de LLMs para análise de sentimentos  
**Como**: executar mesmo input N vezes → calcular TARr@N (concordância com rótulo mais frequente) e TARa@N (concordância com média) → incluir stability index como métrica primária ao lado de F1  
**Trade-offs**: custo N× de avaliação; essencial para detectar MVP que métricas estáticas ocultam  
**Referência**: Seção 3.3

---

## Padrão 6: Calibração Pós-Treino com Temperature Scaling
**Quando usar**: após fine-tuning de LLM para corrigir miscalibração de confiança  
**Como**: dividir dataset de validação em calibration set → otimizar temperatura T que minimiza NLL (Negative Log-Likelihood) das probabilidades calibradas → aplicar T como divisor de logits em inferência  
**Trade-offs**: método simples e eficaz; assume que miscalibração é uniforme (não captura variações por classe)  
**Referência**: Seções 3.10, 5.6, 5.9

---

## Padrão 7: Sistema Híbrido Léxico+LLM
**Quando usar**: em domínios estruturados onde regras determinísticas são confiáveis (finanças, direito)  
**Como**: definir dicionário de sentimento específico do domínio → usar LLM apenas para casos que o léxico não cobre → combinar score léxico + LLM com peso ajustável  
**Trade-offs**: maior estabilidade e interpretabilidade; menor flexibilidade para linguagem nova ou não coberta pelo léxico  
**Referência**: Seções 3.7, 5.14

---

## Padrão 8: Circuit Extraction para Interpretabilidade de Tarefa
**Quando usar**: ao precisar de modelo de sentimento mais leve e interpretável sem retreinamento  
**Como**: aplicar attribution graphs para identificar subnetwork responsável por detecção de sentimento → podar componentes irrelevantes → validar performance na tarefa-alvo  
**Trade-offs**: reduz custo de inferência + aumenta interpretabilidade; pode degradar generalização para inputs out-of-distribution  
**Referência**: Seção 4.3

---

## Padrão 9: Decomposição de Incerteza de Prompt
**Quando usar**: ao diagnosticar se variabilidade vem do design do prompt ou da tarefa em si  
**Como**: separar "prompt uncertainty" (testar variações de prompt no mesmo input) de "recommendation uncertainty" (testar variações do input com mesmo prompt) → endereçar cada fonte separadamente  
**Trade-offs**: diagnóstico preciso da causa raiz; requer instrumentação adicional na pipeline  
**Referência**: Seção 3.6

---

## Padrão 10: Progressive Fine-Tuning para Minimizar Multiplicity
**Quando usar**: ao atualizar modelo de sentimento com novos dados em produção  
**Como**: aplicar atualizações em incrementos pequenos e controlados → verificar consistência de predições entre checkpoint anterior e atual antes de promover para produção → usar ensemble de checkpoints para transição gradual  
**Trade-offs**: reduz risco de deriva comportamental abrupta; aumenta complexidade de gerenciamento de versões  
**Referência**: Seção 5.6

---

## Padrão 11: Counter-Bias em Feedback Loops Humano-IA
**Quando usar**: em sistemas onde usuários interagem repetidamente com o mesmo modelo de sentimento  
**Como**: introduzir periodicamente perspectivas neutras ou contrárias nas respostas → implementar diversity-driven prompts → adicionar explainability interativa para promover decisão balanceada  
**Trade-offs**: melhora objetividade; pode reduzir satisfação de usuários que preferem confirmação  
**Referência**: Seção 5.8

---

## Padrão 12: Protocolos de Reprodutibilidade para Open-Source LLMs
**Quando usar**: ao fazer fine-tuning ou compressão de modelos open-source (DeepSeek, LLaMA, Falcon)  
**Como**: documentar model cards com configuração completa → usar response entropy tracking para detectar behavioral drift → implementar prompt response unit tests antes de deploy → versionar forks com configuração rastreável  
**Trade-offs**: overhead de documentação; essencial para rastreabilidade e compliance regulatório  
**Referência**: Seção 5.13
