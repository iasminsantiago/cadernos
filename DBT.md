In dbt, models are your transformed tables created by SQL files inside your project, whereas sources are the raw, pre-existing tables.
```
sources:
  - name: salesforce
    tables:
      - name: accounts
        columns:
          - name: account_id
            data_tests:
              - unique
```
What data tests will be run in your project?

A unique test will be run on the accounts source table.
salesforce is declared as the source name (representing the raw database or schema where the data lands).

accounts is declared under tables:, meaning it represents a specific source table coming from Salesforce.

The unique test is nested directly under the account_id column of that specific source table.

When you run dbt test (or dbt source test), dbt will execute a query against the raw accounts source table in your data warehouse to verify that every value in the account_id column is unique.

---


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
- Teste sources for data integrity, teste models for transformation integrity
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

---
Assume your project only has models and sources and data tests configured on models and sources. (i.e. there are not snapshots or seeds -- these are beyond the scope of this quiz)

How does the dbt build command work?
dbt build will first test your sources, then materialize and test each model in DAG order.
How dbt build works under the hood
The dbt build command is designed to be resource-efficient and to catch data quality issues as early as possible. It executes your project in DAG (Directed Acyclic Graph) order, meaning it follows the strict parent-to-child lineage of your data pipeline from left to right.

The execution sequence follows this logic:

Source Testing: Sources are the starting point of your DAG. Before wasting compute resources transforming raw data, dbt build runs any data tests or freshness checks configured on your sources.

Step-by-Step Materialization & Testing: Once the sources pass, dbt moves down the graph. For each model, it will materialize the model (create the table/view) and then immediately run its tests before moving on to any downstream models that depend on it.

The "Fail-Fast" Benefit
The main advantage of dbt build over running dbt run && dbt test sequentially is its fail-fast nature. If a staging model fails its unique test, dbt build will automatically skip all downstream marts and reports that rely on that model. This prevents bad data from propagating through your warehouse and saves compute costs.

Why the other options are incorrect:
a and c are incorrect: These options describe a linear approach (doing all of one action, then all of another). If dbt built all models before testing them (as in option c), a failure in a raw source or early staging model wouldn't stop bad data from being processed by all subsequent downstream models.

---

Which command will run data tests only on sources?

a. dbt test --select source:*
Why this is correct:
In dbt, you use the --select (or -s) flag to filter which nodes you want to execute. The syntax source:* uses a selection method descriptor that tells dbt to select all configured sources in the project.

Running dbt test --select source:* instructs dbt to look at your entire Directed Acyclic Graph (DAG), isolate only the source nodes, and execute the data tests attached to them.

Alternative Shortcut:
You can also use the shorthand command:

Bash
dbt test --select path.to.source_file.yml
...but to select all sources across the entire project globally, source:* is the standard notation.

---

you are working in development and run dbt build. Your entire project materializes and tests successfully. You have only accepted values tests on your sources and models.

On Tuesday, you log back in and run dbt run and your models all run. You then run dbt test and find that 5 tests failed.

What is most likely the reason for the tests failing?
A new value was introduced on a column you were testing.
The scenario states that you are specifically using accepted values tests. An accepted values test ensures that all data within a specific column matches a predefined list of allowed values (e.g., if a status column should only contain 'ordered', 'shipped', or 'delivered').

Here is why c perfectly explains the timeline:

On Monday: Your source data only contained approved values, so dbt build passed.

On Tuesday: You ran dbt run, which successfully rebuilt your models. This means your SQL code had zero compilation or syntax errors.

The Failure: When you ran dbt test, it failed because the underlying raw data in your warehouse changed between Monday and Tuesday—a new, unexpected value (e.g., 'returned' or a typo like 'shpped') was introduced into the source data, causing the accepted values test to trigger a failure.

Why the other options are incorrect:
a is incorrect: If there was a compilation error in your SQL, your models would have failed during the dbt run step before you even got to dbt test.

b is incorrect: If a column name in a source had changed, the dbt run step would have failed with a "column not found" database error when trying to build the staging models.
