# Rossmann Store Sales

## 1. Problema de negócio

A Rossmann opera mais de 3.000 drogarias em 7 países europeus. Atualmente, os gerentes de loja da Rossmann têm a tarefa de prever suas vendas diárias com até seis semanas de antecedência. As vendas da loja são influenciadas por muitos fatores, incluindo promoções, concorrência, feriados escolares e estaduais, sazonalidade e localidade. Com milhares de gerentes individuais prevendo vendas com base em suas circunstâncias únicas, a precisão dos resultados pode variar bastante.

Problema:
- Previsão de vendas das lojas para as próximas 6 semanas para alocação de recursos para reforma destas lojas.

Causas:
- As previsões de vendas atuais apresentam muita divergência nos resultados em comparação com as vendas reais.
- As previsões de vendas atuais são baseadas apenas na experiência da equipe de negócios, de maneira muito empírica.
- As previsões de vendas atuais são feitas individualmente para cada loja, faltando sincronismo.
- As previsões de vendas atuais são limitadas às máquinas locais.

Solução:
- Modelo de previsão de vendas utilizando Machine Learning.
- Bot no telegram contendo as previsões de vendas para cada loja.

Fonte do problema:
- https://www.kaggle.com/c/rossmann-store-sales/overview/description

Ferramentas utilizadas:
- Python 3.9.16

Passos para a resolução do problema:
![crips_ds](https://user-images.githubusercontent.com/108444459/234751189-81fd246d-95e9-4483-933a-1a272f73855a.PNG)

## 2. Descrição dos dados

Atributos das lojas:
- 'Store': código da loja,
- 'StoreType': tipo da loja,
- 'Assortment': sortimento (básico, extra ou extendido),
- 'CompetitionDistance': distância da loja competidora mais próxima,
- 'CompetitionOpenSinceMonth': tempo, em meses, desde que existem lojas competidoras,
- 'CompetitionOpenSinceYear': tempo, em anos, desde que existem lojas competidoras,
- 'Promo2': loja em período de promoção extendida ou não,
- 'Promo2SinceWeek': tempo, em semanas, desde que a loja esta em promoção extendida,
- 'Promo2SinceYear': tempo, em anos, desde que a loja esta em promoção extendida,
- 'PromoInterval': meses de intervalo da promoção.

Atributos das vendas:
- 'Store': código da loja,
- 'DayOfWeek': dia da semana em que ocorreu a venda,
- 'Date': data da venda,
- 'Customers': clientes que entraram na loja naquela data,
- 'Open': loja aberta ou não,
- 'Promo': loja em período de promoção ou não,
- 'StateHoliday': feriado ou não,
- 'SchoolHoliday': férias escolares ou não.

Variável resposta:
- 'Sales': quantidade de vendas da loja.

Tratativa de NaNs:
- competition_distance: se o valor for NA, substituir por um valor muito alto, pois se é NA significa que não tem competidor próximo ou a distância é muito grande.
- competition_open_since_month/year: se o valor for NA, substituir pelo mês/ano referente à coluna 'date'.
- promo2_since_week/year: se o valor for NA, substituir pela semana/ano referente à coluna 'date'.
- promo_interval: se o valor for NA, substituir por 0.

## 3. Engenharia de atributos

Premissas:
- As linhas com a variável 'open' = 0 foram eliminadas, visto que indicam que a loja em questão está fechada,
- As linhas com a variável 'sales' = 0 foram eliminadas, visto que indicam que não ocorreu nenhuma venda,
- A coluna customers foi eliminada, visto que não é possível saber quantos clientes estarão na loja em 6 semanas.

## 4. Análise exploratória dos dados

Mindmap do negócio:
![mindmap_hipoteses](https://user-images.githubusercontent.com/108444459/234752258-be01dae8-7547-4c2f-b143-ac74363d1f19.png)

Hipóteses analisadas:
1. Lojas com maior sortimento vendem mais.
2. Lojas com competidores mais próximos vendem menos.
3. Lojas com competidores a mais tempo vendem mais.
4. Lojas com promoções ativas por mais tempo vendem mais.
5. Lojas com mais dias de promoção vendem mais.*
6. Lojas com mais promoções consecutivas vendem menos.
7. Lojas abertas durante o feriado do Natal vendem mais.
8. Lojas vendem mais ao longo dos anos.
9. Lojas vendem menos no segundo semestre do ano.
10. Lojas vendem mais depois do dia 10 de cada mês.
11. Lojas vendem menos nos finais de semana.
12. Lojas vendem menos durante os feriados escolares, exceto nos meses de Julho e Agosto.

Validação das hipóteses:

![validacao_hipoteses](https://user-images.githubusercontent.com/108444459/234752889-5cd47b7b-89c3-47fd-9073-6aa338e3aeca.PNG)

Legendas:
- Conclusão: V para verdadeira, F para falsa.
- Relevância: B para baixa, M para média, A para alta.
- Insight: I para improvável, P para provável.

Principais considerações:
- Na hipótese 3, há um pico de vendas no mês em que abrem novos competidores.
- A hipótese 5 foi desconsiderada deste ciclo do CRISP, pois é muito parecida com a hipótese 4 e demoraria muito mais para comprová-la, pois seria necessário derivar todos os dias de promoção por loja e fazer a contagem.
- Portanto, é melhor deixar para um próximo ciclo do CRISP.
- Na hipótese 7, como ainda não ocorreu o natal em 2015, obviamente as vendas acumuladas no natal são menores.
- Na hipótese 8, como ainda não acabou o ano em 2015, obviamente as vendas são menores ao longo dos anos.
- Na hipótese 11, lojas vendem menos nos finais de semana, porém a proporção de vendas no domingo é a maior entre todos os dias da semana.

## 5. Preparação dos dados

- Nas variáveis numéricas não cíclicas foi aplicada a técnica de rescala (MinMax Scaler e Robust Scaler).
- Nas variáveis numéricas cíclicas foram aplicadas transformações de natureza utilizando funções seno e cosseno.
- Nas variáveis categóricas foram aplicadas técnicas de encodings (One Hot Encoding, Label Encoder e Ordinal Encoder).
- Na variável resposta foi aplicada uma transformação logaritmica, a fim de deixá-la com uma distribuição mais próxima de uma distribuição normal.

## 6. Seleção de atributos

Nesta etapa foi utilizado um algoritmo de seleção de features chamado Boruta, o qual rankeia os atributos de acordo com a relevância de cada um e elimina os atributos julgados irrelevantes para a modelagem de Machine Learning. É necessário dividir o conjunto de dados em treino e teste e escolher um algoritmo regressor (neste caso foi definida uma Random Forest) para aplicar o Boruta.

Como a previsão de vendas ocorre com base nas 6 semanas seguintes a última data de venda do conjunto de dados, foram separadas para teste todas as linhas cuja a coluna 'date' esteja entre as 6 semanas mais próximas à data da venda mais recente, enquanto as demais linhas ficaram para treino.

## 7. Modelagem

A escolha dos modelos foi realizada pensando na característica do problema, ou seja, a variável resposta é contínua e é um problema de série temporal. Sendo assim, foram definidas as seguintes opções iniciais:
- Average Model,
- Linear Regression,
- Random Forest Regressor,
- XGBoost Regressor.

A escolha dos erros foi baseada tanto na avaliação do ponto de vista de negócio quanto do ponto de vista da modelagem de Machine Learning. Deste modo, foram definidas as seguintes opções de erros:
- Mean Absolute Error (MAE): indica o quanto as previsões erram, em valor absoluto, em relação a média de venda de todas as lojas.
- Mean Average Percentage Error (MAPE): indica o quanto as previsões erram, em valor percentual, em relação a média de venda de todas as lojas.
- Root Mean Squared Error (RMSE): mais robusto contra outliers, pois o erro é dado pela raíz quadrada do quadrado da diferença entre o valor real e o valor predito.

Foi definido como baseline a média de vendas entre as lojas (Average Model), o qual apresentou um erro MAE de 45%.

Para os demais modelos foi aplicada a técnica de Cross-Validation. Para aplicar o Cross-Validation em uma série temporal deve ser separado, para uma primeira interação, um conjunto pequeno para treino com as datas mais antigas e um conjunto para validação com as datas posteriores sequenciais às datas separadas para treino. Na segunda interação, utiliza-se como base de treino todos os dados de treino e validação utilizados na primeira interação e como novo conjunto de validação usa-se as datas posteriores sequenciais às datas separadas para o novo treino. As demais interações seguem a mesma lógica, conforme mostrado na figura abaixo:
![ts_cv](https://user-images.githubusercontent.com/108444459/234761672-96d908bb-e70e-49e1-b0a9-e86508ab3462.png)
fonte: https://towardsdatascience.com/4-things-to-do-when-applying-cross-validation-with-time-series-c6a5674ebf3a

Após o treinamento dos modelos foi escolhido seguir com o XGBoost Regressor. A diferença percentual entre o erro MAE do XGBoost e da Random Forest foi de aproximadamente 2% em favor da Random Forest. Porém, por ser treinado de maneira muito mais rápida e apresentar melhores resultados após a tunagem, foi escolhido seguir com o XGBoost.

## 8. Tunagem dos hiperparâmetros

Para a tunagem dos hiperparâmetros foi escolhida a técnica de Random Search, a qual é mais performática que a Grid Search, pois realiza apenas uma quantidade limitada de interações com base nas possibilidades de parâmetros definidas como base, enquanto a Grid Search utiliza todos os valores possíveis, levando muito tempo para ser finalizada.





