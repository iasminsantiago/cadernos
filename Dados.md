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
## Databricks
* *Dashboard*: = um painel com informações e indicadores
* UC Unity Catalog: gerencia governançá e os dados/objetos de dado do meu workspace
*   notebook - querry + markdown
*   SQL Editor - desenvolvo, testo e salvo querries
  
*   Workspace: onde eu faço meu trabalho. Tudo está dentro do workspace.
  * unity metastore: contém os metadados dos objetos e acessos // pode conter varios catalogs 
    * *Catalog*: schemas + objetos de dados 
        * container lógico // pode conter multiplos schemas // lhnautical, no nosso projeto
      * *Schema* = database base de dados / dbo, no nosso projeto
        *   OBJETOS DE DADOS DO SCHEMA:
        * table - objeto de dados tabulares
        * view - resultado de uma querry de tabelas, só read-only // db_lh_vw
        * volume - objeto de armazenar dados NÃO-tabulares para os governar/organizar
          * semi e não estruturados -- Ex.: parquet, xlsx, CSV (que é na verdade arquivo de texto)
          * só usados como path-based dataa ccess; não pode ser usado como table location
          * permite acessar, governar, armaenar  e organizar arquivos  que não são manejaveis em em um banco de dados
          * mais usado para guardar os dados raw, que podem depois ser convertidos para tabela ou suados em ML e IA
        * função - salvo funçào que retornará valor ou set de linhas // criar funções SQL e salvar num catalog ou shcema e compartilhar com pessoas da minha equipe
        * models
       
          
* computers
  * serverless - qualquer tipo de task, on-demand, mais r[apido que clusters
  * all-purpose - geral, usado pros notebooks
  * job compute - workflows automatizados
  * SQL warehouse -  querries SQL sobre os data objects e exploraçao de dados
 
    
* Principals
  * user - humano que acessa, representado pelo seu e-mail
  * service principals - atores não humanos, como serviço de automaçao para schedule e conector para 3rd parties, ex. conectores para S3 e airflow
  * account groups -  grupo das entidades user e ser principals. Os administradores podem manejar os grupos com eles, workspaces, dados.

* address a data object - catalog.schema.dataobjectname para maior granularidade -- select from lnnautical.dbo.sales
* TODO objeto de dados no UC tem dono, que pode ser user, serv princ, acc group podem ser donos
* o principal que criou o objeto tem privilégios maiores como select e modify. Mas o dono de um catalog não tem maior ou igual hierarquia que o dono de seu schema, mesmo que este esteja dentro daquele.
* os NON-DATA objetos, como workbook e querry, podem ter dono remanejado pelo administrador do workspace
  * CAN READ/VIEW - view e create copy
  * CAN RUN - READ + run para obter resultados -- ex. querry num notebook
  * CAN EDIT - run + editar objeto
  * CAN MANAGE - edit + modify permissoes e acessos + deletar o objeto
* objetos que eu não tenho acesso NÃO aparecem para mim. Ex. não tenho acesso ao bdo, então clico em lhnautical e a pasta tá vazia

* Delta lake - API/protocolo pra ler e escrever files na nuvem


- The Control Plane "controls" what happens in the platform, and the Compute Plane "computes" the tasks.
  - The Control Plane is the "brains" of the platform, controlling all processing and creating clusters.
  - The Compute Plane is the "muscles" of the platform, and houses all of the physical entities of the platform.
