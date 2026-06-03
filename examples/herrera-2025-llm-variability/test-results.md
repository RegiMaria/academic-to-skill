O Claude Code lê e interpreta o texto independente do formato das tabelas — pdftotext é suficiente para todos os casos testados.

LLM-AS-A-JUDGE: Claude sonnet 4-5

## Arquivos Gerados

**ch01-introducao.md**  ⭐⭐⭐⭐⭐

Capturou corretamente que ESA-CDM significa "Crowd Decision Making" (não "Ensemble"). A distinção aleatória vs. epistêmica está precisa.

**ch02-estudos-de-caso.md ⭐⭐⭐⭐⭐

Todos os números corretos. Destaque para capturar o hardware do EC2 (4× H100 80GB) e a implicação regulatória do exemplo financeiro.

**ch03-razoes-fundamentais.md** ⭐⭐⭐⭐⭐

As 12 razões estão corretas e bem organizadas. A decomposição de incerteza de Kweon et al. (prompt uncertainty vs. recommendation uncertainty) está precisa.

**ch04-explainability.md** ⭐⭐⭐⭐⭐

Capturou a distinção local vs. global de XAI e o conceito de "full-stack explainability architecture" corretamente.

**ch05-desafios-e-estrategias.md** ⭐⭐⭐⭐⭐

Os 14 desafios mapeados com soluções acionáveis. O sumário de ações prioritárias é um bônus útil.

**ch06-conclusoes.md** ⭐⭐⭐⭐⭐

As 4 contribuições originais estão exatas. Capturou que as 14 estratégias são propostas sem validação experimental — limitação crítica documentada corretamente.

## Avaliação dos Arquivos extras gerados

**key-findings.md** ⭐⭐⭐⭐⭐ Perfeito

Todos os números batem exatamente com o artigo:

GPT-4o T=1.0 → 63% em 0.4 ✓
GPT-4o T=0.0 → 71% em 0.4, extremos persistem ✓
Range financeiro 0.40–0.80, média 0.67 ✓
Atil et al. → até 10% de variação ✓

A seção "O Que Este Artigo NÃO Prova" é particularmente valiosa, capturou com precisão que as 14 estratégias são propostas sem validação experimental própria.

**methodology.md** ⭐⭐⭐⭐⭐ Perfeito
Capturou detalhes que exigem leitura cuidadosa do artigo:

Os dois prompts exatos do EC2 ✓
Hardware: 4× H100 80GB, llama.cpp ✓
Link do dataset TripR-2020Large ✓
Checklist de replicação completo ✓


**references.md** ⭐⭐⭐⭐⭐ Muito bom

As referências mais citadas estão corretas. Beigi et al. 2024 é de fato a mais citada. O contexto de uso de cada referência reflete com precisão como elas aparecem no texto.

**research-gaps.md** ⭐⭐⭐⭐⭐ Excelente

As 5 oportunidades de pesquisa derivada são originais e acionáveis — vão além do que o artigo lista explicitamente, inferindo oportunidades reais a partir das lacunas declaradas.

**glossary.md** ⭐⭐⭐⭐⭐ Completo

35+ termos, todos com referências de seção corretas. Destaque para termos técnicos como α-Shatten norm, TAR@N e reasoning topology — que exigem leitura profunda para definir corretamente.

**patterns.md** ⭐⭐⭐⭐⭐ Acionável
12 padrões no formato "quando usar / como / trade-offs" — exatamente o que um pesquisador precisa para aplicar o conteúdo do artigo.

**cheatsheet.md** ⭐⭐⭐⭐⭐ Referência rápida

A tabela "Causa Raiz → Solução Prioritária" é o melhor resumo do artigo em formato consultável.

**SKILL.md** ⭐⭐⭐⭐⭐ Índice perfeito

Taxonomia das 12 causas em tabela, framework de temperatura, XAI full-stack — tudo estruturado para consulta rápida.

**Veredicto Geral**

14 arquivos, todos corretos, todos em português, todos acionáveis.

Resultado geral: 20/20 arquivos corretos.