# Lacunas de Pesquisa — An overview of model uncertainty and variability in LLM-based sentiment analysis

> Gerado em português. O que os próprios autores apontam como incompleto, limitado ou como
> direção para trabalhos futuros. Use para identificar oportunidades de pesquisa derivada.

---

## Limitações Declaradas pelos Autores

- **Escopo experimental restrito**: os estudos de caso usam apenas dois modelos (GPT-4o e Mixtral 8x22B) — resultados podem não generalizar para outros LLMs como LLaMA, Falcon ou modelos open-source menores
- **Experimento financeiro hipotético**: o cenário de trading da Seção 2.3.3 é ilustrativo, não baseado em dados reais de mercado — não valida MVP em produção financeira real
- **Falta de validação das estratégias de mitigação**: nenhuma das 14 estratégias propostas na Seção 5 foi testada experimentalmente no paper — são propostas baseadas em literatura existente
- **Ausência de benchmarks formais de consistência**: os autores reconhecem que benchmarks formais para output consistency sob diferentes temperaturas ainda não existem — identificado explicitamente como trabalho futuro
- **Controle experimental limitado pela API**: top-k não é exposto pela API OpenAI; seed está em Beta — parâmetros de controle incompletos para reprodutibilidade total
- **Survey qualitativo para a maioria das seções**: seções 3 e 5 são análise de literatura, não experimentos próprios — profundidade empírica concentrada apenas na Seção 2

---

## Trabalhos Futuros Sugeridos pelos Autores

- Desenvolver **benchmarks formais para consistência de output** sob diferentes configurações de temperatura — prioridade explicitada na Seção 6.1
- Criar **frameworks gerais de calibração pós-treinamento** para estabilização — mencionado como necessidade urgente em domínios regulados
- Estabelecer **padrões de reprodutibilidade** e **ferramentas de avaliação ciente de compressão** para LLMs open-source (DeepSeek, LLaMA, Mistral, Falcon)
- Desenvolver **auditorias de interpretabilidade** e **protocolos de calibração** como requisitos de conformidade regulatória emergente (finanças, saúde, serviços públicos)
- Aprofundar o papel da temperatura em cenários de **crowd decision-making** com múltiplos LLMs
- Criar métricas de **estabilidade sob atualizações de modelo** para rastrear fine-tuning multiplicity em produção

---

## Perguntas em Aberto Identificadas no Texto

1. **Quanto da variância do MVP é eliminável?** Os autores mostram que T=0 reduz mas não elimina MVP — qual é o piso teórico de variabilidade residual para cada arquitetura?

2. **Como UQ escala para pipelines de CDM com dezenas de usuários?** O paper foca em avaliações individuais — como UQ frameworks se comportam quando N usuários interagem com o mesmo modelo simultaneamente?

3. **Até que ponto RLHF introduz viés não-intencional vs. melhoria intencional de alinhamento?** O paper discute o trade-off mas não quantifica o impacto líquido em estabilidade de sentimento

4. **Quando hibridizar LLM + léxico é melhor que LLM puro?** O paper sugere sistemas híbridos mas não provê critérios claros para quando cada abordagem é preferível

5. **Como certificar LLMs open-source para uso regulado?** Com proliferação de forks do DeepSeek e Falcon, quais garantias mínimas de comportamento são necessárias e como verificá-las?

---

## Oportunidades para Pesquisa Derivada

1. **Benchmark de estabilidade multi-LLM**: desenvolver dataset específico para medir TAR@N e stability index em análise de sentimentos para pelo menos 10 LLMs diferentes (GPT-4o, Claude, Gemini, LLaMA, Falcon, DeepSeek, etc.) sob configurações controladas — respondendo diretamente ao gap identificado pelos autores

2. **Framework de calibração automática por domínio**: criar sistema que automaticamente determina temperatura ótima, pooling mechanism e técnica de calibração (temperature scaling vs. isotonic regression) para um domínio específico sem dados de teste anotados, usando apenas WeightWatcher + análise espectral

3. **Avaliação de fine-tuning multiplicity em produção**: estudo longitudinal de como MVP evolui ao longo de ciclos de atualização do modelo em um sistema de análise de sentimentos em produção — medindo fine-tuning multiplicity em cenário real

4. **XAI para detecção automática de fonte de MVP**: construir pipeline que automaticamente classifica se uma inconsistência de sentimento tem origem em prompt sensitivity, stochasticidade de temperatura, fine-tuning multiplicity ou domain drift — usando combination de SHAP/LIME + reasoning topology + spectral diagnostics

5. **Impacto de MVP em sistemas ESA-CDM reais**: implementar e medir o impacto de MVP em um sistema de crowd decision-making real, comparando estratégias de mitigação (ensemble vs. temperature calibration vs. prompt standardization) em termos de qualidade de consenso final e custo computacional
