# Escolaridade e Empregabilidade Juvenil no Brasil

**Global Solution — Modelagem Linear para Aprendizado de Máquina · Desafio CitizenTech**

## 1. Integrantes

| Nome completo | RM |
| --- | --- |
| João Pedro Sousa | 573962 |
| Matheus Evangelista | 568593 |
| Murilo Lima de Carvalho | 570156 |

---

## 2. Apresentação do problema

A inserção dos jovens no mercado de trabalho é influenciada por diferentes fatores, entre eles a escolaridade, a idade, as condições socioeconômicas e as características do próprio mercado de trabalho. Este projeto investiga a relação entre o nível de escolaridade e a inserção profissional de jovens brasileiros, considerando também os rendimentos habituais do trabalho.

**Pergunta de pesquisa:** existe associação entre o nível de escolaridade e a inserção dos jovens brasileiros de 18 a 24 anos no mercado de trabalho? Como a escolaridade se relaciona com seus rendimentos?

### Objetivo geral

Analisar estatisticamente a associação entre escolaridade, condição de ocupação e rendimento habitual do trabalho entre jovens brasileiros de 18 a 24 anos, utilizando microdados da PNAD Contínua referentes ao 4º trimestre de 2024.

### Objetivos específicos

- Descrever o perfil demográfico, educacional e ocupacional dos jovens da amostra.
- Investigar a distribuição dos rendimentos e realizar os cálculos probabilísticos e testes de média propostos.
- Medir a associação entre anos de estudo e rendimento por meio de covariância, correlação e regressão linear simples.
- Avaliar a relação entre rendimento, escolaridade, idade e horas habitualmente trabalhadas por meio de regressão linear múltipla.
- Estimar a probabilidade de ocupação entre jovens que fazem parte da força de trabalho por meio de regressão logística binária.
- Interpretar os resultados, discutir as limitações e produzir evidências que possam apoiar reflexões e iniciativas relacionadas à empregabilidade juvenil.

### Hipóteses de pesquisa

- **H1-A:** maior escolaridade está associada a maior probabilidade de ocupação, considerando os jovens que estão na força de trabalho.
- **H1-B:** maior escolaridade está associada a maior rendimento habitual, considerando os jovens ocupados.

O estudo é observacional. Portanto, os resultados indicam **associações estatísticas**, e não demonstram que a escolaridade, isoladamente, cause maior ocupação ou rendimento.

## 3. Base de dados e fonte

O projeto utiliza microdados da **Pesquisa Nacional por Amostra de Domicílios Contínua (PNAD Contínua)**, produzida pelo **Instituto Brasileiro de Geografia e Estatística (IBGE)**.

| Característica | Descrição |
|---|---|
| Pesquisa | PNAD Contínua — divulgação trimestral |
| Instituição responsável | Instituto Brasileiro de Geografia e Estatística (IBGE) |
| Período analisado | 4º trimestre de 2024 |
| Abrangência | Brasil |
| População de interesse | Pessoas de 18 a 24 anos |
| Unidade de análise | Pessoa entrevistada |
| Base utilizada no notebook | Arquivo CSV disponibilizado no GitHub |
| Quantidade inicial analisada | 44.020 registros e 14 colunas |

### Fontes oficiais

- [IBGE — PNAD Contínua Trimestral](https://www.ibge.gov.br/estatisticas/sociais/trabalho/9173-pesquisa-nacional-por-amostra-de-domicilios-continua-trimestral.html)
- [IBGE — PNAD Contínua Trimestral - Trimestres de 2024](https://www.ibge.gov.br/estatisticas/downloads-estatisticas.html?caminho=Trabalho_e_Rendimento/Pesquisa_Nacional_por_Amostra_de_Domicilios_continua/Trimestral/Microdados/2024)
- [IBGE — FTP de microdados trimestrais](https://ftp.ibge.gov.br/Trabalho_e_Rendimento/Pesquisa_Nacional_por_Amostra_de_Domicilios_continua/Trimestral/Microdados/)

### Arquivo de dados utilizado pelo notebook

O notebook carrega a base já preparada diretamente do endereço Raw do GitHub:

`https://raw.githubusercontent.com/JP-Sousa/GS-MLAM-Citizen-Tech/refs/heads/main/data/source/pnad_jovens_18_24_2024q4.csv`

O arquivo contém o recorte de jovens de 18 a 24 anos utilizado nas análises.

### Bases de análise construídas

Durante o tratamento, o notebook organiza os dados em três bases, de acordo com a população adequada para cada análise:

| Base | População | Utilização |
|---|---|---|
| **Base A** | Jovens de 18 a 24 anos com informações essenciais válidas | Perfil e análises descritivas |
| **Base B** | Jovens ocupados com rendimento positivo e horas habituais válidas | Distribuição de rendimento, testes de média e regressões lineares |
| **Base C** | Jovens na força de trabalho: ocupados e desocupados | Regressão logística binária |

A Base A preserva os jovens fora da força de trabalho para as análises descritivas. A Base B restringe as análises de rendimento aos ocupados com informações válidas. A Base C não considera como desocupados os jovens que estão fora da força de trabalho.

O notebook documenta as regras de seleção e exporta as três bases tratadas para `data/processed/`:

- `base_A_jovens_18_24.csv`
- `base_B_ocupados_rendimento.csv`
- `base_C_forca_de_trabalho.csv`

Os nomes das variáveis são simplificados no tratamento, por exemplo: `idade`, `anos_estudo`, `rendimento`, `horas_semanais`, `peso`, `tamanho_domicilio` e `status_ocupacao`.

## 4. Organização dos arquivos

Os principais arquivos do projeto são:

- `notebook/GS_MLAM.ipynb` — notebook Python com carregamento dos dados, inspeção, tratamento, análises estatísticas, modelos, gráficos, tabelas e síntese dos resultados.
- `src/preparar_dados.ipynb` — notebook responsável pela preparação e recorte inicial dos microdados da PNAD Contínua.

```text
projeto/
├── data/
│   ├── processed/
│   │   ├── base_A_jovens_18_24.csv
│   │   ├── base_B_ocupados_rendimento.csv
│   │   └── base_C_forca_de_trabalho.csv
│   └── source/
│       └── pnad_jovens_18_24_2024q4.csv
├── notebook/
│   └── GS_MLAM.ipynb
├── report/
│   └── Relatorio_Estatistico.pdf
├── results/
│   ├── figures/
│   │   └── arquivos .png gerados pelas análises
│   ├── tables/
│   │   └── arquivos .csv gerados pelas análises
│   └── relatorio_automatico.md
├── src/
│   ├── preparar_dados.ipynb
│   └── README.md
└── README.md
```

## 5. Como executar o projeto

A execução foi organizada para o **Google Colab**, podendo também ser adaptada a um ambiente Jupyter Notebook local.

### Opção recomendada: Google Colab

1. Baixe ou clone este repositório para ter acesso ao notebook.
2. Abra o [Google Colab](https://colab.research.google.com/).
3. Selecione **Arquivo → Fazer upload de notebook** e envie `notebook/GS_MLAM.ipynb`, ou abra o notebook a partir do GitHub.
4. Confirme que o ambiente está conectado à internet, pois o notebook busca o CSV diretamente no GitHub.
5. Execute as células em ordem, de cima para baixo, usando **Ambiente de execução → Executar tudo**.
6. Aguarde a conclusão de cada etapa. O notebook apresenta tabelas, gráficos, métricas e interpretações ao longo da execução.
7. Ao final, consulte os arquivos gerados nas pastas `data/processed/`, `results/figures/` e `results/tables/`. No Colab, eles ficam no diretório de trabalho da sessão e podem ser baixados ou copiados para o repositório.

**Importante:** como a base é carregada por uma URL Raw do GitHub, o endereço precisa continuar público e válido. Se o arquivo for movido ou renomeado no repositório, atualize a URL na célula de carregamento do notebook.

### Execução local com Jupyter Notebook

1. Instale Python 3.10 ou superior e as dependências listadas na próxima seção.
2. Clone ou baixe o repositório.
3. Abra um terminal na pasta do projeto.
4. Instale as dependências com:

   ```bash
   python -m pip install -r requirements.txt
   ```

   Caso o repositório ainda não tenha um `requirements.txt`, instale as bibliotecas listadas na seção 6.

5. Inicie o Jupyter:

   ```bash
   jupyter notebook
   ```

6. Abra `notebook/GS_MLAM.ipynb` e execute todas as células em ordem.

A execução local também precisa de acesso à internet para obter o CSV no GitHub.

## 6. Bibliotecas e dependências

O notebook utiliza as seguintes bibliotecas Python:

| Biblioteca | Finalidade |
|---|---|
| `pandas` | Leitura, organização, transformação e exportação dos dados |
| `numpy` | Operações numéricas, cálculos e geração de amostras |
| `matplotlib` | Construção e salvamento de gráficos |
| `seaborn` | Visualizações estatísticas e mapas de calor |
| `scipy` | Distribuições probabilísticas, testes estatísticos e correlações |
| `statsmodels` | Regressões, testes, intervalos de confiança e diagnósticos estatísticos |
| `scikit-learn` | Regressão linear de verificação, separação treino/teste e métricas de classificação |
| `IPython` | Exibição de tabelas e textos formatados no notebook |
| `os` e `warnings` | Criação de diretórios e controle de avisos; fazem parte da biblioteca padrão do Python |

### Instalação

No Google Colab, muitas dessas bibliotecas já estão disponíveis. Se necessário, execute em uma célula:

```python
%pip install pandas numpy matplotlib seaborn scipy statsmodels scikit-learn ipython
```

Em ambiente local, o mesmo comando pode ser executado no terminal:

```bash
python -m pip install pandas numpy matplotlib seaborn scipy statsmodels scikit-learn ipython
```

## 7. Síntese dos principais resultados

### Perfil da amostra e tratamento

- A base contém **44.020 jovens**, com idades entre 18 e 24 anos, referentes ao 4º trimestre de 2024.
- Foram identificados **24.682 ocupados (56,1%)**, **3.797 desocupados (8,6%)** e **15.541 pessoas fora da força de trabalho (35,3%)**.
- A Base B, formada por ocupados com rendimento e horas válidos, contém **23.716 observações**.
- A Base C, formada por pessoas na força de trabalho, contém **28.479 observações**; nessa base, 86,7% estão ocupadas.
- As ausências em rendimento e horas trabalhadas são, em grande parte, estruturais: essas informações não se aplicam a pessoas que não estão ocupadas.

### Escolaridade e probabilidade de ocupação — H1-A

A hipótese de que maior escolaridade está associada a maior probabilidade de ocupação foi **suportada pelos resultados**:

- A taxa de ocupação na força de trabalho variou de **85,2%** na faixa de 0 a 8 anos de estudo a **90,7%** na faixa de 16 anos de estudo.
- O teste qui-quadrado identificou associação estatisticamente significativa entre escolaridade e ocupação (`p < 0,0001`), mas o V de Cramér foi **0,056**, indicando associação de pequena intensidade.
- Na regressão logística, a razão de chances estimada foi **1,031 por ano adicional de estudo**, controlando idade e número de moradores no domicílio.
- Apesar da associação estatística, o modelo apresentou capacidade limitada de discriminação: **AUC = 0,582**. No limiar de 0,5, a acurácia foi semelhante à de um modelo ingênuo que sempre prevê a classe majoritária, e a especificidade foi de 0,0%.

### Escolaridade e rendimento — H1-B

A hipótese de que maior escolaridade está associada a maior rendimento habitual foi **suportada pelos resultados**:

- O rendimento habitual apresentou média de **R$ 1.703,65** e mediana de **R$ 1.412,00**.
- A correlação de Pearson entre anos de estudo e rendimento foi **r = 0,171**, indicando associação positiva fraca.
- Na regressão linear simples, cada ano adicional de estudo esteve associado a aproximadamente **R$ 102,54 a mais** no rendimento mensal. O modelo apresentou `R² = 0,029`.
- Na regressão linear múltipla, controlando idade e horas semanais habituais, a associação estimada foi de aproximadamente **R$ 98,95 por ano adicional de estudo**. O modelo apresentou `R² = 0,128`.
- Na especificação logarítmica, cada ano adicional de estudo esteve associado a um rendimento aproximadamente **6,5% maior**.

### Outros resultados

- A distribuição de rendimentos é fortemente assimétrica à direita. Os testes de normalidade rejeitaram a hipótese de distribuição Normal.
- Os testes Z regionais, com amostras pequenas, rejeitaram as hipóteses nulas nos dois casos conforme os níveis de significância definidos no projeto: Nordeste, `α = 6%` e `n = 20`; Sudeste, `α = 9%` e `n = 25`. A repetição de 2.000 sorteios mostrou que as decisões podem variar entre amostras.
- Para o cenário de 22 anos, 12 anos de estudo e 40 horas semanais, a regressão múltipla estimou rendimento médio de **R$ 1.852,40**.
- No cenário de classificação com 22 anos, 12 anos de estudo e quatro moradores no domicílio, a probabilidade estimada de ocupação foi de **88,0%**.

Os valores de previsão representam estimativas condicionadas aos modelos e às variáveis utilizadas. Não devem ser interpretados como garantia de rendimento ou situação profissional de uma pessoa específica.

## 8. Limitações identificadas

1. **Associação não significa causalidade:** os dados são observacionais e não permitem afirmar que anos adicionais de estudo causam, por si só, maior ocupação ou rendimento.
2. **Recorte temporal e etário:** a análise considera somente jovens de 18 a 24 anos no 4º trimestre de 2024. Os resultados não representam automaticamente outras faixas etárias ou períodos.
3. **Variáveis não observadas:** fatores como experiência profissional, setor de atividade, formalidade, região, qualidade da educação, área de formação e condições socioeconômicas podem influenciar os resultados e não foram integralmente controlados.
4. **Desenho amostral da PNAD:** a pesquisa possui pesos, estratos e conglomerados. Os modelos e testes principais seguem procedimentos convencionais e não incorporam integralmente o desenho amostral complexo. As análises ponderadas servem como verificações de estabilidade dos coeficientes, mas não substituem uma inferência apropriada ao desenho da pesquisa.
5. **Assimetria e valores extremos do rendimento:** a distribuição é fortemente assimétrica, o que limita a adequação da Normal e dos pressupostos usuais da regressão linear. Foram realizadas análises de robustez, transformação logarítmica e uso de erros-padrão robustos, mas as limitações não desaparecem completamente.
6. **Testes Z com amostras pequenas:** os testes regionais utilizam amostras de 20 e 25 observações, e suas decisões podem ser sensíveis ao sorteio e à definição do grupo de referência.
7. **Seleção das bases:** as análises de rendimento consideram apenas ocupados com informações válidas; a regressão logística considera apenas pessoas na força de trabalho. Portanto, os resultados desses modelos não se aplicam indistintamente a todos os jovens.
8. **Desbalanceamento das classes:** na regressão logística, a maioria dos indivíduos da Base C está ocupada. Por isso, a acurácia isolada pode ser enganosa; a baixa especificidade e a AUC de 0,582 indicam desempenho limitado para identificar desocupados.
9. **Limite da variável de escolaridade:** a categoria de 16 anos agrupa pessoas com 16 anos ou mais de estudo, não permitindo distinguir níveis superiores a esse limite.
10. **Registros idênticos:** as linhas totalmente idênticas foram mantidas, pois a base não oferece identificador individual que permita concluir que sejam duplicações indevidas.

## 9. Conclusão

Os resultados encontrados indicam que, na amostra de jovens brasileiros de 18 a 24 anos da PNAD Contínua do 4º trimestre de 2024, a escolaridade está positivamente associada tanto à probabilidade de ocupação — entre pessoas na força de trabalho — quanto ao rendimento habitual — entre pessoas ocupadas.

A associação com o rendimento permaneceu positiva após o controle por idade e horas semanais de trabalho. Entretanto, os modelos explicam apenas parte da variação observada, reforçando que a inserção profissional juvenil depende de múltiplos fatores.

No contexto do desafio CitizenTech, os achados apoiam a relevância de iniciativas voltadas à permanência escolar, à conclusão da educação básica, à qualificação profissional e à articulação dessas ações com políticas de geração de oportunidades, formalização e desenvolvimento regional. As evidências devem ser utilizadas para reflexão e planejamento coletivo, e não para decisões automatizadas sobre indivíduos.

## 10. Referências

- **IBGE.** Pesquisa Nacional por Amostra de Domicílios Contínua — PNAD Contínua Trimestral. [Página oficial da pesquisa](https://www.ibge.gov.br/estatisticas/sociais/trabalho/9173-pesquisa-nacional-por-amostra-de-domicilios-continua-trimestral.html).
- **IBGE.** Microdados trimestrais da PNAD Contínua. [Repositório oficial de microdados](https://ftp.ibge.gov.br/Trabalho_e_Rendimento/Pesquisa_Nacional_por_Amostra_de_Domicilios_continua/Trimestral/Microdados/).
- `notebook/GS_MLAM.ipynb`. Notebook de análise estatística e preditiva do projeto.
- `results/relatorio_automatico.md`. Relatório automático dos resultados da execução do notebook.