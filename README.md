# Análise de E-commerce com Pandas (Olist)

Prática de limpeza, tratamento de valores ausentes e agregação com Pandas.

> Objetivo: Ter uma visão geral do negócio, com foco em performance de entrega dos produtos para os clientes.

## Dados
Brazilian E-Commerce Public Dataset by Olist (Kaggle).

## Tratamento de dados ausentes
As colunas de data têm nulos (ex: 2.965 em `order_delivered_customer_date`).
Investigando por status, esses nulos são **estruturais**, não erros: pedidos
em trânsito, cancelados ou indisponíveis ainda não têm data de entrega.

**Decisão:** não preencher nem excluir. Preencher criaria entregas fictícias,
e excluir perderia pedidos cancelados, que são informação de negócio.
Para as métricas de entrega, filtrei apenas pedidos `delivered` com data preenchida.

**Anomalia encontrada:** 8 pedidos com status `delivered` sem data de entrega,
uma inconsistência na fonte. Foram mantidos na base e excluídos só do cálculo.

## Principais resultados
- 96.470 pedidos entregues analisados
- Tempo médio de entrega: 12 dias (mediana: 10)
- 8,1% dos pedidos chegaram após a data estimada
- **SP** é o estado mais rápido (8,3 dias) e concentra ~42% dos pedidos
- **RR** é o mais lento (29 dias), 3,5x mais que SP
- **AM e AP** demoram ~26 dias, mas só ~4% atrasam: o prazo estimado já considera a distância
- **AL** tem 23,9% de atraso, quase 3x a média nacional: indica prazo estimado mal calculado, não só distância

> Obs: estados do Norte têm poucos pedidos (ex: RR com 41), então as médias são menos estáveis.

## Tecnologias
Python · Pandas · Jupyter

## Etapas
1. **Extração:** Dados vieram do seguinte link (https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?resource=download)
2. **Entendimento da Base:** Entendendo a base e o porque dos nulos.
3. **Transformação:** colunas novas, agrupamentos, filtros e a não preenchimento dos nulos.
4. **Saída:** Resultado das perguntas propostas

## Como executar
    1. Clone o repositório:
        git clone https://github.com/RaissaYrina/analise-ecommerce-olist.git
        cd analise-ecommerce-olist
    2. Instale as dependências:
        pip install -r requirements.txt
    3. Baixe os arquivos `olist_customers_dataset.csv` e `olist_orders_dataset.csv`
    no [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) e coloque na pasta `data/`.
    4. Abra o notebook:
        jupyter notebook ecommerce.ipynb

## O que aprendi
- Antes de tratar nulos, é preciso entender **por que** eles existem. Excluir ou
  preencher sem investigar pode distorcer a análise.
- Usei `.isna().sum()` para mapear nulos por coluna e `value_counts()` para
  cruzá-los com o status do pedido.
- Minha maior dificuldade foi separar nulos esperados (pedidos não entregues) de
  anomalias (8 pedidos "entregues" sem data). A solução foi manter tudo na base
  e filtrar só no cálculo, para não distorcer o % de atrasos.

## Próximos passos
- Adicionar gráficos com Matplotlib
- Transformar a análise em pipeline: Airflow orquestrando e dados salvos no S3/BigQuery

## Autora
Raissa · [LinkedIn](https://www.linkedin.com/in/raissamarusso/)