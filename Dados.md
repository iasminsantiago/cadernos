Olá! Esse caderno abordará temas iniciantes e gerais de dados.

# Dado, informação, Conhecimento, sabedoria....
*Dado*
Peso - 50 Kg 
- Observação simples e pura. O mínimo de unidade de uma informação, isolada. 
  - Quantificável.
> É um dado. Mas ele não está aplicado a contexto, nào informa uma situaçao nem me traz sabedoria a cerca dele.



  *Informação*
  - Dado foi inserido num contexto, agora tendo relevância e algum propósito.

---
### Fontes já usadas:
- [Fundamentos de Dados - Indicicum Academy](https://academy.indicium.ai/course/fundamentos-em-dados)
- [Databricks - Introduction tp Data Engineering](https://app.datacamp.com/learn/courses/introduction-to-data-engineering)



Para que profcessar dados?
- transformar dados nos traz:  remover dados não necessários 9duplicados, nulos etc), salva memoria, converter dados de um tipo pra outro mais nefessario (converetr musica de .flac para .ogg), ajustar para um schema/estrutura disponivel e limitante, reorganizar dados de data lake pra data warehouse, para scientistas de dados analisarem e resolverem problemas da sociedade. 

## Bases de dados / databases
*sistema de computador* que guarda quantidaed imensa de dados.

- Schema do banco de dados: define propriedades e relações do banco - relaçÕes, tabelas com entiedade e atributo, tipo de dado. Banco não estruturado não tem schema, é schema-less.

## Cloud
<img width="1170" height="600" alt="image" src="https://github.com/user-attachments/assets/f9c5a7db-ab45-4585-9c22-9631dcf19d49" />
Fonte: DataCamp

- As empresas de cloud oferecem 3 serviços: storage, computation and databases.
- Storage é muito barata pois ela só provê armazenamento, nenhuma outra função - Amazon S3, 
- Computation: usar computador virtual, com processamento deles e não seu; pra host web srevices - Amazon EC2
- Databases - Coleçào grande *organizada*, feita para rápida pesquisa e obtençao -  Amazon SDR, Azure SQL Database
- Em sistemas modernos (como aplicações Full Stack), eles quase sempre trabalham em dupla.
- 
- 
Num E-commerce:

A foto do produto (um arquivo pesado de 2MB) é enviada para o Storage. O Storage gera um link público para essa imagem (ex: https://storage.com/produto123.jpg).

O nome do produto ("Camiseta Preta"), o preço (R$ 59,90), o estoque (15 unidades) e a URL da imagem são salvos na Database.


- Ao invés de processar (transformar) dados num só pc, dta enginerrs usam clusters de máquinas (por abstração de arquitetura). A transformação pode ocorrer numa máquina local ou de terceiros (cloud etc).
- Mas o data engineer entende as abstrações e sabe as limitações da máquina que usará.
- Spark é processador: pode escalr número de nodes quando tem dados demais a processar;
- airflow gerencia ordem dos jobs e depedências resolvidas corretamente, além de horário


<img width="1029" height="524" alt="image" src="https://github.com/user-attachments/assets/5fb25b26-3939-4823-a6c5-98561e243f62" />


ELT faz transformação no data warehouse, só possível em dat awarehouses *de cloud*. Em outro local e forma, custo e infraestrutura seriam custosos e lentos.


---
## MEDALLION ARCHITECTURE
- Progressively melhora estrutura e qualidade dos dados
BRONZE - Raw data - armazenamento fundamental // adiciona-se colunas de metadados para 
SILVER - Cleaned - limpo, transformado e enriquecido - dataset refinado e pronto para análise
GOLD - Curated - curado, agregado, alta qualidade  - otimizado apra relatório e advanced analysis - a mais usada em apps de BI
---

## DBT
- sources (src) - raw data built on warehouse througyh loading
- staging (stg) - modelo feito de sources. Relacionamento 1-1 com as sources. Usados para transformaçoes leves, que desejavamos na source, como rename, cast data types, currency conversion. modelos stg são usados para limpar e padronizar dados antes de transformar nos processos downstream. Geralmente materializados em views.
- intermediate (int) - modelos existentes entre fct e dim finais, sem depender direto das sources mas sim dos stage models, pra nivelar limpeza de dados da stg. É onde ficam fct e dim, contem joins e aggregations. 
- fact (fct) - dados de algo que acontece ou esta acontecendo - sessão, transação, pedido/NF em restaurante, votos. Geralmente tem poucas coluans e muuitas linhas.
- dimension (dim) - dados que representam algo - pessoa, lugar ou coisa - cliente, produto, candidato, construção, item de cardapio
- Boa prática: fazer cada mudançá em commits diferentes, para se precisar fazer rollbacks em commits. Ex.: refatorar uma dim, criar uma stage.


### PASTAS
- marts folder -  todos os modelos int, fct e dim são guardados nela. Podemos usar subpastas para organiza-la por tipo de negócio - marketing, finance. Aplica nela logica de negocios e e referencia modelos upstream pela ref macro
- staging folder - todos os modelos stg e configuraçoes src. Podemos usar subpastas para separar por fonte de dados (segmento, salesforce)
<img width="326" height="382" alt="image" src="https://github.com/user-attachments/assets/536e62ec-ca09-4ea6-9d6a-d1e0f9621c6d" />

### COMANDOS
- dbt run - corre TODOS os modelos do diretório de modelos, mas pode especificar: dbt run -s staging corre todos os modelos de models/staging

### QUESTOES
1. Which command will only materialize dim_customers.sqland its downstream models?
   dbt run --select dim_customers+
   
3. You are working in the dbt Studio. How do you ensure the model you have created is built in your data platform?
    You must use the dbt run command. Quem realmente executa/constroi o modelo no banco de dados é o comando dbt run.
   Escrever nodelo - criar sql / escrever a lógica - select x,y from raw.customers, é um arquivo sql somente.
   Salvar - guardar o arquivo. Não materializou / construiu ele.
   dbt run - materializa o modelo no banco de dados // lê arquivo .sql, executa logica (select) no warehgouse (snowflake, sql server) e cria/atualiza a view ou table (depende da config do modelo) =materializa o modelo no banco de dados.
   
4. You just finished developing your first model that directly references and transforms customers source data. Where in your file tree should this model live?
    models/staging/stg_customers.sql
   
6. You want all models in a folder to be materialized as tables.
    in the dbt_project.yml file. O arquivo dbt_project.yml é onde você configura o comportamento dos modelos do projeto inteiro ou de pastas específicas. models my_project marts: materializaed: table
   Já O sources.yml serve para definir sources, por exemplo: de onde vem dados, nome das tabelas, testes, descricoes
   O models.yml é usado para documentar nmodelos, adicionar descricoes, configurar testes (not_null, unique etc)

7. You have just built a dim_customers.sql model that relies on data from the upstream model stg_customers. How would you reference this model in dim_customers?
    select * from {{ ref('stg_customers') }}. No dbt, quando você quer usar outro modelo dbt, você utiliza a macro ref(). Como stg_customers é um modelo upstream (criado no dbt), não colocamos sql, sql é arquivo e não modelo.
   Se fosse {{ source(stg_customers) }} está errado pois source não serve para modelos dbt, não tem modelos nela. Source é usado para acessar tabelas brutas, sua sintaxe seria {{ source('nome_da_fonte', 'nome_da_tabela') }} -> SELECT * FROM  FROM {{ source('jaffle_shop', 'customers') }}.
   Estou acessando dados brutos ou outro modelo do dbt?  Dados brutos (raw) → source()   {{ source('jaffle_shop', 'customers') }}
   Outro modelo do dbt → ref()  {{ ref('stg_customers') }}

8. Which command will only materialize dim_customers.sqland its downstream models?
     dbt run --select dim_customers+
     o operador + controla dependências.
    modelo+ → executa o modelo e todos os modelos downstream (os que dependem dele).<br>
    +modelo → executa o modelo e todos os upstream (os que ele depende).<br>
    +modelo+ → executa upstream + o modelo + downstream.<br>
    bt run --select dim_customers executa apenas dim_customers. Não inclui os modelos downstream.

9. Which of the following is a benefit of using subdirectories in your models directory?
    Subdirectories allow you to configure materializations at the folder level for a collection of models.
   📁 Subdirectories → organização + configurações em nível de pasta
   🔗 ref() → dependências entre modelos.
   ⚙️ dbt_project.yml → configurações aplicadas às pastas.
   Ao organizar os modelos em subpastas (staging/, intermediate/, marts/), posso aplicar configurações para todos os modelos daquela pasta no arquivo dbt_project.yml.
```
models:
  my_project:
    staging:
      +materialized: view

    marts:
      +materialized: table
```

Todos os modelos em models/staging/ serão views. Todos os modelos em models/marts/ serão tables. Você não precisa configurar cada modelo individualmente.



❌ b. Subdirectories allow you to include multiple dbt projects in a single project

As subpastas servem apenas para organizar os modelos dentro do mesmo projeto. Não permitem incluir vários projetos dbt em um único projeto.

❌ c. Subdirectories allow you to explicitly build dependencies between models based on the naming of the folders

As dependências entre modelos não são definidas pelas pastas, são definidas peli uso da macro {{ ref('stg_customers') }}. É o ref() que cria a dependência, não a estrutura de diretórios.

❌ d. Subdirectories will automatically change the schema where a model is built based on the name of the folder the model is located in

O dbt não muda automaticamente o schema apenas porque você colocou o modelo em uma determinada pasta. Você pode configurar schemas por pasta, mas isso precisa ser feito explicitamente no dbt_project.yml.
