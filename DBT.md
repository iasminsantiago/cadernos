## YAML
models:
  name do modelo:
  columns do modelo
    - name da coluna: event_id
      description da coluna: This is a unique identifier por the event
      data_tests:
        - unique
        - not null

    - name de outra coluna: event_time
      description: "when the event ocurred in UTC (eg. 2018-01-02 12:00:00)"
      data_tests:
      - not null

    - name: user_id
      description:  The ID of the user  whor ecorded the event
      data_tests
        - not null
        - relationships:  (é um teste de relacionamento, testando se ha relacionamento)
          arguments: # avaliable in v1.10.5 and higher . older  versions
            to: ref('users')
            field: id


## sources
source: stage between dbt and the data platform
```
models/staging/jaffle_shop/_src_jaffle_shop.yml

sources:
  - name: jaffle_shop // name = schema by default
    database: raw
    schema: jaffle_shop
    tables:
      - name: customers
      - name: orders

  - name: stripe // so adicione schema se usar uma source name diferente do shcema ja existente
    tables:
      - name: payments
```

```
models/staging/jaffle_shop/stg_jaffle_shop__customers.sql

select 
    id as customer_id,
    first_name,
    last_name
from {{ source('jaffle_shop', 'customers') }}
```

```
models/staging/jaffle_shop/stg_jaffle_shop__orders.sql

select
    id as order_id,
    user_id as customer_id,
    order_date,
    status
from {{ source('jaffle_shop', 'orders') }}
```


- se digitar __ ele mostra lista de shortcuts, tem a __source e ele monta estrutura pra eu substituir palavras
