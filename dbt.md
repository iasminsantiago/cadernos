# DBT

--dbt project
.sql - model - casts, logica etc
.yml - schema - as tabelas e testes simples
- deve ter arquivo .yml
- é uma pasta, facil de copiar  e mover:
- has all componentes needed to work with data within dbt
- structured COLECTION  of files 
-  files define how to transform and organize the data
-   configuraçoes -  nome projeto, nome pastas, fonte e destino de dados (fuutro DW)
-   querries SQL e templates contem acesso e transformacao dos dados, relacoes entre dados
-   COMO CRIAR PROJETO DBT: dbt init - informo nome do projeto e tipo de databse
-   PROFILE ~deployment -> tem ambientes dev, prod, stg, testing; projeto pode ter variso profiles, assim podemos ter diferentes tipos de DW por cenario (dev prod tes t etc)
-     profiles soa definidos no yml
exemplo de profile (profiles.yml) com 2 tiupos de deployment:
<img width="532" height="352" alt="image" src="https://github.com/user-attachments/assets/e67aeb3b-e7c2-44fd-b1d7-9c914fac88e7" />
- target: default deploy = dev

## YML
- yet another markup language
- text-based  arquivo
- muito usado em desenvolvimento para configurar, pois é human-readable

- Exercicio: temos arquivo Projectinfo.txt informando nome do projeto nyc_yellow_taxi e databse type: duckdb -> dbt init nyc_yellow_taxi, confirma duckdb, cd nyc_yellow_taxi,
- ls mostra os diretorios criados (analyses, dbt_project.yml macros, models, README.md, seeds, snapshots, testes)

## PROJECT PROFILE
-  defines where your database / data warehouse information is defined.

## MATERIALIZATION
"A process is materialized" = transform src data and place the result in table or view // Colocar em uma tabela

## TABLE VS VIEW
- TABLE - OBject of database or warehouse - HOLDS data, takes memopry space - its content is updated ONLY if a specifi commands changes its data
- VIEW - DO NOT HOLD data  - holds little space - select querry on a table/tables - responde: not saved, generated when querried

## footsteps
- DBT INIT - CREATE/COPY PROJECT
- CREATE DATA DESTINATION IN profiles.yml - define/update configuration
- DEFINE THE MODELS
- DBT RUN model (transformed raw data) - dows data transofrmation and pushes it to warehouse -- use when model changes or process should be MATERIALIZED
- VERIFY THE CONTENTS ON DATAWAREHOUSE

# References
DATACAMP - DBT FUNDAMENTALS
