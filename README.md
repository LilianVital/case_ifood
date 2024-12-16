# Objetivo

Projeto desenvolvido para a avaliação técnica de data analyst do Ifood. 

O objetivo do projeto é desenvolver uma estratégia de oferta automatizada por um modelo de machine learning, afim de otimizar o ARPU (average revenue per user). 

## Dataset overview
Os dados se referem a eventos de transações, ofertas e informações de clientes de uma empresa.

**offers.jsons** - Dataset contém os IDs das ofertas e metadados de cada uma delas:

*  ```id``` [string]: ID da oferta

* ```offertype``` [string]: Tipo da oferta, i.e., BOGO, discount, informational

* ```minvalue``` [int]: Valor mínimo que precisa ser gasto para que a oferta seja ativada

* ```discountvalue``` [int]: Valor do desconto a ser aplicado, caso a oferta seja ativada

* ```duration``` [int]: Tempo durante o qual a oferta está disponível para o cliente agir, i.e., prazo para que o cliente utilize a oferta recebida


**customers.jsons** - Dataset contém informações sobre aproximadamente 17k clientes:

* ```id``` [string]: ID do cliente

* ```age``` [int]: Idade do cliente no momento da criação da conta

* ```registered_on``` [string]: Data em que o cliente criou a conta

* ```gender``` [string]: gênero do cliente (algumas entradas podem conter '0' para outras opções além de M ou F)

* ```credit_card_limit``` [float]: Limite do cartão de crédito do cliente registrado no momento da criação da conta.

**transactions.jsons** - Dataset contém informações sobre aproximadamente 300k eventos:

* ```account_id``` [string]: ID do cliente

* ```event``` [string]: Descrição do evento (transação, oferta recebida, oferta visualizada, oferta concluída)

* ```time_since_test_start``` [int]: Tempo passado desde o começo do teste, em dias. Os dados começam em t=0

* ```value ``` [json]: Pode registrar o offer_id de oferta, o desconto concedido (reward) ou o valor da transação, dependendo do tipo de evento
    * ```offer id``` [string]
    * ```offer_id``` [string]
    * ```amount``` [float]
    * ```reward``` [float]


## Manipulação dos dados
O projeto apresenta desafios significativos no processo de modelagem, especialmente na transformação dos dados de transações. Um dos principais obstáculos foi a presença de dois campos distintos de ```offer_id``` e a ausência de uma associação direta entre os eventos de transação e um ```offer_id```.

Para resolver esses problemas, foi realizada uma análise detalhada dos campos de ```offer_id```, identificando que ambos representavam a mesma informação. Além disso, para estabelecer uma relação entre ofertas e transações, foi desenvolvida uma abordagem proxy. Essa proxy considerou uma janela temporal de influência para vincular uma transação a uma oferta específica.

O processo completo de manipulação e limpeza dos dados está documentado no notebook localizado em ```/notebooks/00_data_cleaning```.

## Análise exploratória dos dados
O notebook contendo a análise exploratória dos dados está disponível em ```/notebooks/01_exploratory_analysis```

## Segmentação de clientes
A segmentação de clientes foi realizada utilizando o algoritmo K-means. O notebook detalhando o desenvolvimento do modelo está disponível em ```/notebooks/03_customer_segmentation```

## Sistema de recomendação
Duas abordagens principais foram desenvolvidas para o sistema de recomendação, cada uma documentada em notebooks distintos:

	1.	Análise de ofertas com base em dados históricos:

        Notebook: ```/notebooks/02_recsys_cold_start```
        Essa abordagem analisa o desempenho histórico das ofertas, considerando o ARPU (Average Revenue Per User) como métrica principal. A metodologia seleciona as ofertas com maior ARPU histórico para recomendar a novos clientes. Uma simulação foi realizada para avaliar o impacto da implementação das três ofertas com melhor desempenho em termos de ARPU para clientes novos.

	2.	Modelo de Recomendação com funkSVD

        Notebook: ```/notebooks/04_recsys_model```
        Essa abordagem utiliza o modelo funkSVD para prever a probabilidade de um cliente aceitar uma determinada oferta. A partir dessas previsões, é possível recomendar as ofertas com maior probabilidade de aceitação, otimizando a personalização das sugestões para cada cliente. Uma simulação foi realizada para avaliar o impacto da implementação do modelo no desempenho em termos de ARPU. 