# SQL

Tem que separar uma querry de outra com ;

```sql
select * from raw.jaffle_shop.customers;

create table raw.jaffle_shop.orders
{ id integer,
  user_id integer,
  order_date date,
  status varchar,
  _etl_loaded_at timestamp edfault current_timestamp
};

// veja que são 2   uerries diferentes ;)
```
---

- O campo é uma coisa ou uma propriedade?
  - coisa - entidade - row - record/registro
  - propriedade - atributo - coluna - field/campo
 
- *Banco de dados é difernete de planilha excel*? Sim, pois tem>
 - escalabildiade - databases aguentam milhoes de linhas, spreadsheets aguentam só centenas
 - concorrencia - vários usuários podem usar e editar databases simultaneamente
 - automaçao - databses sáo feitas para serem acessadas com programaçáo -- por aplicaçoes e pipelines, como databricks e dbt
 - ja spreadsheets sáo projetadas para analise manual e rápida, permite poucas pessoas a usando/ feitas para serem acessadas manualmente, não por aplicaçoes
