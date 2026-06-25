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

---
- A tabela DEVE ter um identificador para cada registro dela, chamada ckey / chave.
- se juntasse patreons e checkouts numa tabela só, teremos a primeira coluna card_number e esta será repetida, pois Izzy fez 2 compras, então temos 2 registros.
- <img width="923" height="251" alt="image" src="https://github.com/user-attachments/assets/d60217b2-01a4-40be-a66c-1ed1c35d8338" />

- Mas se dividirmos em checkout e patrons, teremos o registro de izzy e seu card_number, sem repetir, e será a chave da tabela patrons. Separando tabelas, podemos responder questÕes mais facilmente do que com spreadsheets, que tudo fica junto e duplicado, como o caso mencionado.

- <img width="1007" height="480" alt="image" src="https://github.com/user-attachments/assets/7f74d373-fb2e-4252-835a-e0f28cb60aff" />


---
- Schema - design da database / blueprint / planta-baixa
  - relacoes entre as tabelas
  - tabelas que compoem
  - tipos de dados
  - se quero me familiarizar com os tipos de dados de uma database, acho essa info no schema, nao indo na tabela 
 
-  dados sào armazenados no HD de um servidor. Por isso,d atabses são armazenamento + computing:
  - server é computador que armazena informaçao e performa serviços via requests feitos à network deles -- por isso databses aguemta concorrencia, varios usuarios, ideal para collaboration, consegue manejar request de varios xomputadores ao mesmo tempo
    - no caso, o serviço performado é acesso a dados / data access 

---
QUERRY - keywords MAIUSCULOS, tabela e field minusculos
- keywords  - palavras reservadas para operação no código
- * = "widlcard"
- SELECT - field que será mostrado
- FROM - tabela que o field esta localizado
- ; indics querry esta completa
- DISTINCT - retorna valores sem repetir de um field
  - se coloco SELECT DISTINCT author, genre, ele aplica distinct a essa combinacao, nao so a author. Se tem machado - romance, machado=romance, clarice-romance, com distinct vira só machado-romance, clarice-romance. Se a querry fosse seelct distinct author, seria machado, clarice.
  - se quiser o distinct de cada, faço queries separadas. serao 2 select e from. select distinct author from books, select distinct genre from books.
  - se da 247 autores no distinct authors e 249 no distinct author, genre, entao pelo menos 1 autor esreveu pelo menos 2 livros com generos diferentes, multiplos generos.
- view - virtual table that stores querry code, not data, for later use - sql querry salva/ nao salva dados, salva a querry. Nao salva a tabela resultante, nao salva um result set, so salva a querry para reuso, salva o codigo. Se eu acessar a view e tiver atualizado a tabela depois de driar e salva-la, a view mostrará um resultado atualizado
  - CREATE VIEW nome_view as SELECT * FROM books
  - select id from nome_view // da para querry a view tambem
 
  ---
  POSTGRSQL - ... LIMIT 10;
  SQL SERVER - SELECT TOP(10) FROM...
