# Análise de Roubos e Risco Territorial: Insurtech (Seguros de Smartphones)

## Contexto

Este projeto simula uma análise de dados feita para o Diretor de Riscos e Precificação de uma **Insurtech** especializada em seguros de bens portáteis (smartphones, notebooks) e proteção pessoal.

O ponto de partida é um problema de negócio real: a empresa notou uma queda expressiva nas margens de lucro do seguro de smartphones, associada ao aumento de sinistros por **roubo em vias públicas** no estado do Rio de Janeiro. O objetivo é cruzar dados das delegacias (DPs) com o histórico criminal mensal para:

- Entender a dinâmica territorial dos crimes de oportunidade nas ruas;
- Identificar quais regiões/delegacias são **zonas de risco (outliers)**, para eventual reajuste de prêmio por CEP;
- Verificar se existe correlação direta entre `roubo_transeunte` (roubo a pedestre em via pública) e `roubo_celular` (roubo de celular).

## Dados utilizados

| Arquivo | Descrição | Separador / Encoding |
|---|---|---|
| `03.BaseDPEvolucaoMensalCisp.csv` | Evolução mensal de ocorrências por delegacia (CISP), incluindo `roubo_celular` e `roubo_transeunte` | `;` / ISO-8859-1 |
| `08.DP.csv` | Cadastro das delegacias (código, nome, região) | `,` / UTF-8 |

As bases são unidas via `merge` entre `cisp` (base mensal) e `codDP` (base de delegacias).

## O que o notebook faz

1. **Carregamento e limpeza dos dados**: leitura dos dois CSVs, checagem de valores nulos e duplicatas (nenhum encontrado nas colunas de interesse).
2. **Junção das bases** (`merge`) para associar cada ocorrência mensal à delegacia, nome e região correspondentes.
3. **Agregação por delegacia**: soma total de `roubo_celular` e `roubo_transeunte` por delegacia, ranqueadas da maior para a menor.
4. **Estatística descritiva** para as duas variáveis: média, mediana, moda, amplitude, quartis (Q1/Q2/Q3), IQR, limites de outlier (regra 1.5×IQR), desvio padrão, variância, coeficiente de variação, assimetria (skewness) e curtose.
5. **Detecção de outliers** (delegacias/meses fora dos limites inferior/superior do IQR): identifica **Duque de Caxias** e **São João de Meriti** como as delegacias com mais meses atípicos em roubo de celular.
6. **Visualizações**:
   - Boxplots comparando a distribuição de `roubo_celular` e `roubo_transeunte`;
   - Gráfico de dispersão entre as duas variáveis;
   - Séries temporais (média anual) de cada tipo de roubo;
   - Gráfico de barras com o Top 5 delegacias por média histórica de roubo a transeunte.

## Principais conclusões

- A média é maior que a mediana em ambas as variáveis, com assimetria positiva: a maioria dos meses tem níveis moderados de crime, mas meses extremos "puxam" a média para cima, indicando que o risco não é constante ao longo do tempo.
- Existe uma **correlação positiva, mas não perfeita**, entre roubo a transeunte e roubo de celular: os dois crimes tendem a crescer juntos, porém outros fatores também influenciam cada um isoladamente.
- Não há uma tendência de crescimento constante ao longo dos anos; ambos os crimes oscilam, com picos entre 2016–2019 e queda de correlação a partir de 2021/2022.
- Duque de Caxias e São João de Meriti se destacam como as regiões com maior recorrência de meses atípicos (outliers) em roubo de celular: candidatas naturais a reajuste de prêmio por risco territorial.

## Tecnologias

- Python 3
- `pandas`, `numpy`, `matplotlib`
- Power BI

## Como executar

1. Coloque os arquivos `03.BaseDPEvolucaoMensalCisp.csv` e `08.DP.csv` na mesma pasta do notebook.
2. Instale as bibliotecas no VS Code: `pip install pandas numpy matplotlib`.
3. Execute as células do notebook `dadosInsurtech.ipynb` em ordem.
4. Baixe o Power BI para os dashboards

