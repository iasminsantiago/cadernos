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

- Schema é o conjunto de tabelas

- 
```
sources:
  - name: tpch
    database: snowflake_sample_data
    schema: tpch_prod
    tables:
      - name: orders
      - name: customers
      - name: order_items
```
Como selecionar todos os consumidores usando a source macro?
The source macro takes two arguments: {{ source('source_name', 'table_name') }}.
\
```
sources:
  - name: jaffle_shop
    database: raw
    schema: jaffle_shop  
    tables:
      - name: orders
      - name: customers
```
select * from {{ source( 'jaffle_shop', 'orders') }}   -> raw.jaffle_shop.orders, por isso a macro é jaffle_shop e orders


```

sources:
  - name: jaffle_shop
    database: raw  
    schema: jaffle_shop_dataset 
    tables:
      - name: orders
        identifier: jaffle_orders_information_table
      - name: customers
        identifier: jaffle_customers_information_table
```

a referencia seria {{ source('jaffle_shop', 'orders') }}, não 
{{ source('jaffle_shop_dataset', 'jaffle_orders_information_table') }}
```
sources:
  - name: jaffle_shop                         # <-- 1º Argumento do source
    database: raw
    schema: jaffle_shop_dataset
    tables:
      - name: orders                          # <-- 2º Argumento do source
        identifier: jaffle_orders_information_table
      - name: customers
        identifier: jaffle_customers_information_table
```
        
mesmo se fosse jaffle_shop_prod, algo asism, a macro _source_, no dbt, é designed para abstrair os noems fisicos dos objetos da database, assim o codigo fica clean e desvinculado de nomenclaturas especificas pra warehouse como stg, prod etc
{{ source( 'source_name', 'table_name' ) }}  

- source_name: References the - name: property of the source block (jaffle_shop).

- table_name: References the - name: property of the table block (orders).

## Testes - ajuda pessoas a confiarem in my data
dbt test aplica testes que eu faria manualmente, programaticamente, aí da pra escalar pra  projetos grandes
- se os testes da source gfalham, é nbom ppois indicam um problema na ingestao, nao no dbt. Se nao testar a source, terá qie analisar que etapa tem rpoblema depois
### generic test
- escalavel, posso aplicar em muitos diferentes modelos e colunas
- pre-built ja - unique, notnull, accepted_values (valores devem ser um dos de uma lista especificada, como cash, credit, debit, gift-card), relationships (todos os valores de uma coluna tem valores correspondentes na tabela de Pk de uma tabela pai, assegura integridade inferencial)
- sugre aplciar unique e not-null em todas as Pk, isso evita quebra de joins e outros no downstream / as Fk tem relacao com a Pk?  os loaded_at timestamps sendo carregados corretamente?
- reusable, general test - colunas tem valores unicos, nao nulos?
- definido no yaml, ja o singuklar nao
- <img width="746" height="668" alt="image" src="https://github.com/user-attachments/assets/8d81dda9-926f-477c-aab4-6d58976dd163" />

  
## singular test
- personalizavel, mais usado pra BO
- definidos num arquivo de querry sql, num arquivo sql separado, colcoado na pasta tests
- ex.- teste pra assegurar que total revenue, do modelo, nunca é numero negativo
- indicado pra validar regras de negocio complexas, que nao podem er validadas por teste general
