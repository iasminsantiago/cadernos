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
