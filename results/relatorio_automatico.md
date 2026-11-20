# Relatório automático dos achados — Escolaridade e Empregabilidade Juvenil (PNAD Contínua 2024/T4)

Gerado pelo notebook `GS_MLAM_completo.ipynb`. Os números abaixo vêm diretamente da execução; revise a redação antes de incorporá-la ao relatório final em PDF.

### Relatório da inspecção

- A base tem **44.020** registros e **14** colunas, todas do **4º trimestre de 2024**.
- Composição por condição de trabalho: **24.682** ocupados (56,1%), **3.797** desocupados (8,6%) e **15.541** fora da força de trabalho (35,3%).
- As colunas `V4039`, `VD4016` e `VD4031` têm valores ausentes **estruturais**: só existem para ocupados. Isso explica a maior parte das ausências na tabela e **não** indica erro de coleta.
- Checagem sinalizada: *Ocupados sem rendimento (VD4016 ausente)* → **966** registros (2,19% da base).
- Checagem sinalizada: *Ocupados com mais de 84 h/semana (implausível ou muito extremo)* → **18** registros (0,04% da base).
- Há 108 linhas idênticas; elas serão **mantidas** pelo motivo explicado na decisão 4.

### Relatório do tratamento de dados

- Partindo de **44.020** registros, a **Base A** ficou com **44.020** jovens (0 excluídos por falhas de filtro ou validade).
- A **Base B** (ocupados com rendimento e horas válidos) tem **23.716** observações; foram retirados **966** ocupados por rendimento ausente/≤ 0 ou horas inválidas.
- A **Base C** (força de trabalho) tem **28.479** observações, das quais **86,7%** ocupadas. Esse desbalanceamento de classes será tratado na Etapa 8.
- Jornadas acima de 70 h/semana: **98** casos (0,41% da Base B). Foram **mantidos** (decisão 5), pois são possíveis.
- O percentil 99 do rendimento é **R$ 6.000,00**; **225** observações estão acima dele. Elas permanecem na base principal e serão removidas apenas na **análise de robustez** (decisão 6).
- As três bases foram exportadas em `data/processed/` para que qualquer pessoa possa reproduzir as análises sem repetir o tratamento.

### Relatório da Análise Exploratória

- **Perfil:** 49,4% da amostra são mulheres, 75,2% moram em área urbana e a região mais representada é Nordeste (35,1%).
- **Rendimento:** média de R\$ 1.703,65 e mediana de R\$ 1.412,00. A média é 20,7% maior que a mediana, e a assimetria é 12,03 — distribuição fortemente assimétrica à direita, o que já antecipa que a hipótese de normalidade será frágil (Etapa 4).
- **50,2%** dos jovens ocupados recebem até um salário mínimo (R\$ 1.412).
- **Ocupação × escolaridade (H1-A):** a taxa de ocupação na força de trabalho vai de 85,2% (faixa 0–8 anos) a 90,7% (faixa 16 anos); ela não cresce de forma monotônica entre as faixas, o que deve ser lido com cuidado. Qui-quadrado: p < 0,0001, V de Cramér = 0,056 (efeito pequeno).
- **Rendimento × escolaridade (H1-B):** a mediana vai de R\$ 1.412 (faixa 0–8 anos) a R\$ 2.000 (faixa 16 anos), uma razão de 1,42×; ela cresce monotonicamente entre as faixas.
- **Atenção — seleção:** a parcela fora da força de trabalho vai de 46,1% na faixa 0–8 anos a 22,1% na faixa 16 anos. A taxa de ocupação da Base C ignora esse grupo, então não descreve todos os jovens com cada nível de escolaridade.
- Estes são resultados **descritivos** e sem controle de outras variáveis; os modelos das Etapas 6 a 8 refinam a leitura.

### Relatório

- Sob a hipótese de normalidade (μ = R$ 1.703,65, σ = R$ 1.431,51):
- **(a) P(X < média) = 50,00%** (Z = 0,00) → **Evento equiprovável (≈ 50%)**. Por construção, a média é o centro da Normal, então esse resultado é exatamente 50% e independe dos dados.
- **(b) P(X > mediana) = 58,07%** (Z = -0,204) → **Evento provável (> 50%)**. Como a mediana é menor que a média, a Normal atribui mais da metade da probabilidade à região acima da mediana.
- **Quão boa é a hipótese de normalidade?** Nos dados reais, 65,3% dos jovens ganham menos que a média (a Normal prevê 50%) e 49,8% ganham mais que a mediana (por definição, cerca de 50%). A diferença entre o previsto e o observado mostra o efeito da assimetria (12,03).
- A Normal ajustada atribui **11,7%** de probabilidade a rendimentos **negativos**, o que é impossível para este fenômeno — mais um sinal de que ela é apenas uma aproximação.
- Os testes de normalidade **rejeitam** a hipótese de que o rendimento é Normal (Shapiro-Wilk p < 0,0001; D'Agostino-Pearson p < 0,0001). O Q-Q plot confirma visualmente o afastamento nas caudas.
- **Conclusão metodológica:** as probabilidades pedidas foram calculadas e classificadas conforme o enunciado, mas devem ser lidas como resultado de um **modelo simplificador**. Isso será considerado nas limitações dos testes de média (Etapa 5) e das regressões (Etapas 6 e 7).

### Relatório

- **Teste A (Nordeste < demais regiões; α = 6%; n = 20):** média amostral de R$ 941,00 contra referência de R$ 1.898,36; Z = -2,746 (crítico -1,555); p-valor = 0,0030. **Rejeita H0** ao nível de 6%.
- **Teste B (Sudeste > demais regiões; α = 9%; n = 25):** média amostral de R$ 2.033,68 contra referência de R$ 1.653,14; Z = 1,351 (crítico 1,341); p-valor = 0,0884. **Rejeita H0** ao nível de 9%.
- **Contexto real:** na base completa, o rendimento médio do Nordeste é R$ 675,61 menor que o das demais regiões, e o do Sudeste é R$ 191,66 maior.
- **Estabilidade da decisão:** repetindo o sorteio 2.000 vezes, $H_0$ é rejeitada em 78,0% das amostras no Teste A e em 17,6% no Teste B. Valores intermediários indicam que a conclusão de uma única amostra pequena é instável.
- **Variante com referência global** (μ = R$ 1.703,65, σ = R$ 1.431,51): Teste A → Rejeita H0 (p = 0,0086); Teste B → Não rejeita H0 (p = 0,1245). Ao menos uma decisão muda conforme a definição da referência, o que mostra sensibilidade a essa escolha metodológica.
- **Limitações:** (i) a amostra de 20–25 jovens é pequena e o rendimento é assimétrico; (ii) o desvio padrão de referência é tratado como conhecido; (iii) o teste ignora pesos, estratos e conglomerados da PNAD; (iv) 'não rejeitar $H_0$' **não prova** que as médias são iguais — indica apenas que a amostra não forneceu evidência suficiente contra $H_0$ ao nível escolhido.

### Relatório

- **Dispersão:** a nuvem de pontos é ascendente, mas muito dispersa em cada nível de escolaridade (a variabilidade do rendimento é grande mesmo entre jovens com os mesmos anos de estudo); a média por ano de estudo sobe ao longo do eixo.
- **Covariância = 584,6** (sinal positivo: as variáveis tendem a variar no mesmo sentido). Sua magnitude depende das unidades e não mede a intensidade.
- **Correlação de Pearson r = 0,171** (IC 95%: 0,159 a 0,183): associação linear positiva e **fraca**. Spearman ρ = 0,209, consistente com Pearson.
- **Equação:** `rendimento = 499,43 + 102,5444·anos_estudo`. Cada ano adicional de estudo está associado, em média, a **R$ 102,54** a mais de rendimento mensal (IC 95%: R$ 95,03 a R$ 110,06); o intercepto (R$ 499,43) é o valor extrapolado para 0 ano de estudo.
- **Adequação:** R² = 0,029, isto é, a escolaridade sozinha explica 2,9% da variação do rendimento. O teste F (F = 714,6; p < 0,0001) rejeita a hipótese de que o modelo não explica nada; o teste unilateral de $\beta_1 > 0$ tem p < 0,0001.
- **Leitura conjunta:** o efeito é estatisticamente significativo, mas o R² indica que a escolaridade é apenas um dos fatores que influenciam o rendimento. Com amostra grande, até efeitos pequenos ficam significativos; por isso importa olhar o tamanho do efeito (R$ por ano) e o R².
- **Previsão:** para 12 anos de estudo, o rendimento médio previsto é **R$ 1.729,96** (IC 95% da média: R$ 1.711,91 a R$ 1.748,02). Para um jovem específico o intervalo de predição é bem mais largo (R$ -1.034,72 a R$ 4.494,65).
- **Robustez:** β1 permanece positivo e significativo nas quatro especificações; no modelo em logaritmo, cada ano de estudo se associa a um rendimento cerca de 6,3% maior.

### Relatório

- **Correlações com o rendimento:** anos de estudo r = 0,171, idade r = 0,173 e horas semanais r = 0,282. A associação linear mais forte com o rendimento é a de **horas semanais**.
- **Entre as explicativas**, a maior correlação absoluta é 0,154, e o maior VIF é 1,04 → sem sinal de multicolinearidade preocupante.
- **Equação:** `rendimento = -2.754,67 + 98,9489·anos_estudo + 89,6684·idade + 36,1744·horas_semanais`.
- **Interpretação dos coeficientes (ceteris paribus):** cada ano de estudo adicional se associa a R$ 98,95 a mais de rendimento (significativo); cada ano de idade, a R$ 89,67 (significativo); cada hora semanal a mais, a R$ 36,17 (significativo). Pelos coeficientes padronizados, a variável de maior peso relativo é **horas semanais**.
- **Comparação com o modelo simples:** o efeito de um ano de estudo foi de R$ 102,54 (simples) para R$ 98,95 (múltiplo). A redução indica que parte da associação inicial era compartilhada com idade e horas trabalhadas.
- **Adequação global:** R² = 0,128 (R² ajustado = 0,127); teste F = 1.156,2 (p < 0,0001) → rejeita-se $H_0: \beta_1=\beta_2=\beta_3=0$. Teste unilateral de escolaridade ($\beta_1 > 0$): p < 0,0001. O modelo explica 12,8% da variação do rendimento — a maior parte da variação continua sem explicação.
- **Diagnóstico:** Breusch-Pagan p < 0,0001 → há evidência de heterocedasticidade; assimetria dos resíduos = 13,66 (resíduos fortemente assimétricos, o que viola a hipótese de normalidade dos erros). Com erros-padrão robustos (HC3), as conclusões de significância não mudam.
- **Previsão (22 anos, 12 anos de estudo, 40 h/semana):** rendimento médio previsto de **R$ 1.852,40** (IC 95% da média: R$ 1.834,27 a R$ 1.870,53; intervalo de predição individual: R$ -768,60 a R$ 4.473,40). O limite inferior do intervalo individual é negativo, o que não é plausível e reflete a limitação do modelo linear para uma variável assimétrica e positiva.
- **Robustez:** o coeficiente de escolaridade é positivo e significativo em todas as especificações; no modelo em logaritmo, cada ano de estudo se associa a um rendimento cerca de 6,5% maior.
- **Limitações:** associação não é causalidade; variáveis relevantes foram omitidas (setor, formalidade, ocupação, região, experiência); o modelo linear em reais é uma aproximação para um rendimento assimétrico; as horas trabalhadas podem ser consequência, e não só causa, do rendimento.

### Relatório

- **Classes:** 86,7% dos jovens na força de trabalho estão ocupados e 13,3% desocupados. Um modelo que sempre previsse 'ocupado' acertaria 86,7%.
- **Equação estimada (treino):** `z = -0,9823 + 0,1322·idade + 0,0305·anos_estudo − 0,0758·tamanho_domicilio`, com P(ocupado) = 1/(1 + e^(−z)).
- **Escolaridade (H1-A):** razão de chances de 1,031 por ano de estudo adicional (IC 95%: 1,015 a 1,048), mantendo idade e moradores constantes; p unilateral < 0,0001. Há evidência estatística de que mais escolaridade se associa a maior chance de ocupação. Isso corresponde a uma variação de cerca de 3,1% na chance (odds) de estar ocupado por ano de estudo.
- **Outras variáveis:** idade (RC = 1,141; p < 0,0001) e moradores no domicílio (RC = 0,927; p < 0,0001).
- **Qualidade global:** pseudo-R² de McFadden = 0,015; AUC no teste = 0,582 (discriminação fraca, pouco acima do acaso).
- **Matriz de confusão (teste, limiar 0,5):** VN = 0, FP = 759, FN = 0, VP = 4.937. Acurácia = **86,7%**, contra 86,7% do modelo ingênuo; recall dos ocupados = 100,0% e **especificidade (acerto entre os desocupados) = 0,0%**.
- **Leitura crítica:** a acurácia é essencialmente a mesma do modelo ingênuo e o modelo praticamente não identifica desocupados — a acurácia, isoladamente, é enganosa neste problema.
- **Limiar de Youden (0,859):** acurácia = 60,1%, recall = 61,6%, especificidade = 50,5%. Como a maioria da base está ocupada, as probabilidades previstas ficam altas para quase todos; um limiar acima de 0,5 faz o modelo apontar mais jovens como desocupados (sobe a especificidade), ao custo de errar mais entre os ocupados (cai o recall e, em geral, a acurácia). A escolha do limiar depende do objetivo de uso: identificar desocupados ou minimizar erros totais.
- **Classificação do cenário da proposta (22 anos, 12 anos de estudo, 4 moradores):** P(ocupado) = **88,0%** → **Ocupado** (limiar 0,5).
- **Robustez:** o coeficiente de escolaridade é 0,0305 sem pesos e 0,0490 com pesos; o sinal se mantém.
- **Limitações:** a Base C exclui quem está fora da força de trabalho (muitos estudantes), o que pode enviesar a relação; faltam variáveis importantes (região, sexo, raça, experiência, contexto econômico local); os resultados indicam associação, não causa.

### Avaliação das hipóteses de pesquisa

- **H1-A (escolaridade ↑ → probabilidade de ocupação ↑):** **suportada pelos dados**. Evidências: a taxa de ocupação vai de 85,2% (0–8 anos) a 90,7% (16 anos) (qui-quadrado p < 0,0001; V de Cramér = 0,056); na regressão logística, a razão de chances por ano de estudo é 1,031 (p unilateral < 0,0001), controlando idade e moradores. Porém o poder de classificação do modelo é baixo (AUC = 0,582); a escolaridade ajuda a explicar diferenças médias, mas não prevê bem, sozinha, quem estará ocupado.
- **H1-B (escolaridade ↑ → rendimento ↑):** **suportada pelos dados**. Evidências: correlação de Pearson r = 0,171 (fraca); regressão simples: R$ 102,54 por ano de estudo (R² = 0,029; p unilateral < 0,0001); regressão múltipla: R$ 98,95 por ano de estudo, controlando idade e horas (p unilateral < 0,0001). A mediana de rendimento do grupo de maior escolaridade é 1,42× a do grupo de menor escolaridade. Mas a escolaridade explica 12,8% da variação do rendimento no modelo múltiplo; o rendimento depende fortemente de fatores não medidos aqui.
- **Distribuição Normal e testes de média:** o rendimento é assimétrico e a Normal é apenas uma aproximação (Etapa 4). Os testes de média de 20 e 25 observações dependem da amostra sorteada: a fração de rejeição em 2.000 sorteios foi de 78,0% (Teste A) e 17,6% (Teste B).
- **Previsões:** rendimento médio previsto de R$ 1.729,96 para 12 anos de estudo (modelo simples) e de R$ 1.852,40 para o jovem de 22 anos, 12 anos de estudo e 40 h/semana (modelo múltiplo). Para o cenário de classificação, P(ocupado) = 88,0%.

### Implicações para o desafio CitizenTech

- **Para o problema (CitizenTech):** nos dados de 2024/T4, jovens com mais anos de estudo tendem a estar mais ocupados e ter rendimentos maiores; isso reforça o argumento de que políticas de **permanência escolar, conclusão do ensino médio e qualificação profissional** podem estar associadas a melhores condições de entrada no mercado de trabalho — desde que acompanhadas de ações que atuem sobre os demais fatores (oferta regional de vagas, formalização, jornada).
- **O ganho médio por ano de estudo é estimado em R$ 98,95 por mês** (modelo múltiplo), valor útil como ordem de grandeza para discussões sobre retorno da escolaridade — não como previsão individual.
- **Uso responsável:** o modelo de classificação não deve ser usado para decidir sobre indivíduos; seu valor é exploratório e comunitário (identificar perfis com maior vulnerabilidade à desocupação), dada a capacidade de discriminação limitada (AUC = 0,582).