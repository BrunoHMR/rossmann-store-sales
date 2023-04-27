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
- 'SchoolHoliday': férias escolares ou não,

Variável resposta:
- 'Sales': quantidade de vendas da loja.
