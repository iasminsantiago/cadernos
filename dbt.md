# DBT

--dbt project
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

# References
DATACAMP - DBT FUNDAMENTALS
