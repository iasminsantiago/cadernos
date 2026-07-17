# DBT

# dbt project  
.sql - model - casts, logica etc
.yml - schema - as tabelas e testes simples

# EXTENSõES / TIPOS DE ARQUIVO
## .sql  -  transforma algo
- Regras de Negócio: Filtrar clientes ativos, calcular faturamento, limpar textos.
- Usar funções do dbt dentro da query: Como as macros {{ ref('nome_do_modelo') }} ou {{ source(...) }}.
- Regra de ouro: Se você precisa rodar um SELECT, o arquivo TEM que ser .sql. Eles ficam sempre dentro da pasta models/. Ex. -  criar tabelas e views, usamos SELECT, FROM, JOIN, WHERE

## .yml  -  gerencia
- Configuração, Documentação e Testes.
- Arquivos YAML não processam dados, eles apenas guardam informações sobre a estrutura.
- Configuração de ambientes é feita através do arquivo profiles.yml (dbt-core)
    - Configuração de targets dev e prod
- Configurações de projeto são feitas através do dbt_project.yml
- Você usa o .yml para:
    - Configurações Globais: Como o dbt_project.yml (que define o nome do projeto) e o profiles.yml (que guarda a senha e o acesso ao banco).
    - Documentação: Escrever a descrição de uma tabela ou o que significa cada coluna para o usuário final.
    - Testes de Qualidade: Dizer ao dbt: "Olha, a coluna id_cliente não pode ter valores repetidos (unique) e não pode ser vazia (not_null)".
    - Mapear Fontes: Dizer ao dbt onde estão as tabelas brutas que vieram do Stitch, Fivetran, etc. (sources).


# PASTAS / 
## models / .sql e .yml 
- É aqui que o seu trabalho do dia a dia acontece. É a pasta onde fica toda a inteligência do seu pipeline de dados.
- O que tem dentro? Arquivos .sql (seus modelos/tabelas) e arquivos .yml (para testar e documentar esses modelos).
- Como ela se divide por dentro? Para não jogar centenas de arquivos lá dentro, nós criamos subpastas (as famosas camadas):
    -  sources: Dados brutos, São “chamadas” a partir da macro {{source(‘name’, ‘table’)}}. Todas as fontes têm que ser declaradas inicialmente em um arquivo sources.yml, onde é já é possível documentar e testar as tabelas
    -   models/staging/: Arquivos .sql que fazem a limpeza inicial dos dados brutos e arquivos .yml mapeando as fontes (sources). Transformações iniciais. Renomeia colunas, padroniza tipos de dados (cast) e trata nulos.
    -   models/intermediate/: Arquivos .sql com regras de negócio meio complexas que preparam os dados. Camada opcional. Junta tabelas do staging e aplica regras de negócio ou agrupamentos complexos.
    -   models/marts/: Arquivos .sql das tabelas finais que o time de negócio vai usar no PowerBI/Tableau (as dimensões e fatos). (fact e dim): Camada FINAL. Regras de negócio. Deve ser a única camada consultável a nível de BI/Relatório.

- Data Sandboxing"Cada usuário tem seu schema próprio, validações iniciais não tem impacto em produção

## tests / .sql
- Guarda os testes singulares (aqueles que você precisa escrever a query SQL na mão porque a regra de negócio é muito específica).
- Se a query desse arquivo retornar qualquer linha, o dbt entende que o teste falhou.

## macros / .sql
- Pense nas macros como as "funções" ou "fórmulas do Excel" que você mesmo cria para reaproveitar código. Se você repete muito um pedaço de SQL, você cria uma macro.
- O que tem dentro? Arquivos .sql (mas com um código especial do dbt chamado Jinja, cheio de chaves {% %}).

## seeds / .csv
- Sabe aquela tabela estática de "De-Para" de códigos de país, ou uma lista de feriados que nunca muda e que o pessoal do administrativo te passou em um Excel? Você joga ela aqui.
- O que tem dentro? Arquivos .csv. O dbt lê esse CSV e joga ele para dentro do seu banco de dados como se fosse uma tabela automaticamente.

## analyses /
- Espaço para queries SQL analíticas que você quer rodar usando os recursos do dbt (como o {{ ref() }}), mas que você não quer que virem tabelas no banco de dados. É como um bloco de notas de consultas.




# OUTROS TOPICOS
## 🔌 Targets (Ambientes de Execução) -  Dev e Prod
Configurados no arquivo profiles.yml. Dizem para ONDE o dbt vai apontar quando você rodar os comandos.
- Dev (Desenvolvimento): Aponta para as credenciais e schemas de teste do analista
- - Prod (Produção): Aponta para as credenciais oficiais que rodam os dados da empresa de forma automatizada.

## 🗄️ Schemas (Onde as tabelas nascem no Banco de Dados)
O local físico (no Snowflake/BigQuery) criado pelo dbt dependendo do seu Target.  
- Schema Pessoal (Sandbox): Usado quando você roda o dbt em Dev (ex: dbt_joao). TODOS os seus modelos (staging, intermediate e marts) rodam isolados aqui dentro para você testar sem quebrar nada.
- - Schemas de Produção: Usados quando o projeto roda em Prod. O dbt separa os modelos finais automaticamente em schemas organizados para a empresa (ex: analytics_staging e analytics_marts).

## codestyle - Padrões de código
 - Sempre priorizar Common Table Expression (CTEs) ao invés de subqueries
 - Possível forçar a adoção de um codestyle através de ferramentas como o sqlfluff

## vresionamento
Comandos principais

<img width="336" height="194" alt="image" src="https://github.com/user-attachments/assets/894e31ab-652c-4a09-8734-3ce9cf757ddb" />

- Git add: Adiciona as mudanças na área pré-publicação do código
- Git commit: Publica as mudanças na história do git, porém apenas na sua *máquina*. Já criei uma versão do projeto, ams só no meu pc, no .git.
- Git push: Publica as mudanças da sua máquina no repositório remoto

Conflitos:
- Ocorrem quando dois usuários tentam editar o mesmo arquivo a partir de um mesmo ponto na história
- Por não poder inferir qual é a mudança correta, o git aponta um conflito entre os dois arquivos, mostrando onde as histórias divergiram

Como resolver:
- Em casos mais simples, a própria ferramenta (editor de texto ou dbt cloud) irá apontar o conteúdo das mudanças divergentes e o usuário consegue resolver com um clique.
- Em casos mais complexos, pode ser necessário escolher a versão mais correta e substituir completamente a versão com conflito

Como evitar:
- Separação dos ambientes para que 2 desenvolvedores não trabalhem na mesma coisa
- Manter a sua branch local sempre atualizada com as mudanças mais recentes publicadas no repositório

 
 ## macros -  ref e source
 reduzem digitação
 - sources: fotnes raw
 - refs: todo modelo criado DENTRO DO DBT é uma ref. Pode referenciar outra ref, uma source, ambas num mesmo modelo.


## testes
### Generic Tests
-   Generic tests ou schema tests são testes que podem ser aplicados da mesma maneira em qualquer modelo
-   Podem ser escritos no formato de macros, mas o dbt disponibiliza quatro testes prontos

Unique
Not_null
Relationships
Accepted_values

### Testes singulares
 - Tem o objetivo de testar expressões mais complexas
- É construído com base em um model sql que será salvo na pasta testes
- A ideia é testar uma expressão que não deveria retornar nenhum resultado, se retornar dá um erro ou warning
<img width="664" height="193" alt="image" src="https://github.com/user-attachments/assets/996d6c1f-61a6-49d5-9ba8-f3c2d7647558" />


 
### Severidade
 - Nem sempre os testes serão preto no branco, na prática algumas colunas aceitam uma margem de erro
 - Para isso o dbt permite a configuração da severidade do teste, onde é possível definir um valor limite para que levante apenas um aviso e a partir disso um erro.


### Testando expressões
 - É possível também incluir comandos simples de SQL em testes singulares, isso é chamado de testar expressões
 - Muito útil quando você não quer criar uma coluna na tabela só pra testar ou quando é necessário testar uma expressão simples como coluna > X

 
### Source Freshness
- Verifica o quão fresh estão as fontes ou seja, se as extrações atualizaram os dados como esperado
- Baseado em uma coluna de data e um parâmetro definido pelo usuário de quão defasado essa data pode estar até os dados serem considerados desatualizados
<img width="385" height="307" alt="image" src="https://github.com/user-attachments/assets/58a5cd28-0a89-4f43-8619-b0da5accc162" />

 

## MATERIALIZAÇõES DE DADOS
### TABELA 
 - a mais comum
 - Ideal para relatórios/dashboards consultados com frequência
 - OBject of database or warehouse - HOLDS data, takes memopry space - its content is updated ONLY if a specifi commands changes its data
 - <img width="266" height="108" alt="image" src="https://github.com/user-attachments/assets/32bb7a8c-be88-4cb3-a715-07fc9cdd9bae" />

 ### VIEW
 - Só materializa a tabela quando consultada
- “Receita para a materialização da tabela”
- Ideal para consultas pouco custosas que não são realizadas com frequência
- DO NOT HOLD data  - holds little space - select querry on a table/tables - responde: not saved, generated when querried
<img width="304" height="107" alt="image" src="https://github.com/user-attachments/assets/8059ffc3-5ac0-42d0-a298-e7efaf20aaca" />
<img width="714" height="402" alt="image" src="https://github.com/user-attachments/assets/c11ceab8-9f6c-4649-ab82-b1debf63a320" />

## SNAPSHOT
- Utilizado para criação de SCDs tipo 2
- Realiza uma comparação entre a tabela fonte e os novos dados carregados e mantém as alterações, definindo o intervalo em que as mudanças passaram a valer
<img width="449" height="308" alt="image" src="https://github.com/user-attachments/assets/35aae723-73a4-42c9-8852-d409a6f304c9" />
<img width="481" height="161" alt="image" src="https://github.com/user-attachments/assets/27ca77b2-61e7-49dd-af2c-7d04691eed6a" />

## INCREMENTAL
- Tipo de materialização mais complexa
- Processa apenas os dados que sofreram alterações, baseado em uma coluna de chave primária e uma coluna de data de atualização do registro
INCREMENTAL: CUIDADOS
 - Dados podem ficar “velhos”
 - Janela de atualização deve ser muito bem definida, ou através de uma coluna na tabela ou utilizando um período fixo (atualizar somente os últimos 60 dias por exemplo)
 - Recomendável rodar um --full-refresh periodicamente para atualizar a tabela completamente

 - 
### EPHEMERAL
- Dropa todas as tabelas intermediárias, mantendo apenas o resultado final
- Ideal quando se tem o ambiente dev e prod bem separado, evitando consumo desnecessário de disco em produção





## YML
- yet another markup language
- text-based  arquivo
- muito usado em desenvolvimento para configurar, pois é human-readable

## dbt_project
- deve ter arquivo .yml -> dbt_project.yml, alem do profiles.yml que [e pessoal meu
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

- Exercicio: temos arquivo Projectinfo.txt informando nome do projeto nyc_yellow_taxi e databse type: duckdb -> dbt init nyc_yellow_taxi, confirma duckdb, cd nyc_yellow_taxi,
- ls mostra os diretorios criados (analyses, dbt_project.yml macros, models, README.md, seeds, snapshots, testes)

## PROJECT PROFILE
-  defines where your database / data warehouse information is defined.

## MATERIALIZATION
"A process is materialized" = transform src data and place the result in table or view // Colocar em uma tabela



## footsteps
- DBT INIT - CREATE/COPY PROJECT
- CREATE DATA DESTINATION IN profiles.yml - define/update configuration
- DEFINE THE MODELS
- DBT RUN model (transformed raw data) - dows data transofrmation and pushes it to warehouse -- use when model changes or process should be MATERIALIZED
- VERIFY THE CONTENTS ON DATAWAREHOUSE

# References
DATACAMP - DBT FUNDAMENTALS
