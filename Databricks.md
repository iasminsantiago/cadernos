## Databricks
---

Nivel + alto do databricks - account administrator, ele pode criar e adc gente ao workspace, governar acessos e monitorara a conta toda e coisas/workspaces dela e billings.
No account console, accounts.cloud.databricks.com
- workspaces - da pra ver todos os woekspaces, onde os datasets estao armazenados no lakehouse
- data  - gerir data catalogs usando o unity catalog
- users e groups - local unificado pra gerenciar acesso sdos users adicionados a minha conta
- acc admin tem permissoes pra conta inteira e configuracpes back-end como revursos etc; workspace admin, mesmo sendo user externo, tem permissao pras operacoes do workspace especifico

- 
* *Dashboard*: = um painel com informações e indicadores
* UC Unity Catalog: gerencia governançá e os dados/objetos de dado do meu workspace
*   notebook - querry + markdown
*   SQL Editor - desenvolvo, testo e salvo querries

---

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
       
--- 

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

---

Delta - formato de dado. Traz performance de datawarehouse ao lakehouse. 
- coleção de:  arquivo parquet + metadados + log de transação JSON -> dados se comportarão como uma tabela mas formato de arquivo aberto no lakehouse, ACID, permite datasets  batch e streaming, tudo num so lugar.
- após dados serem stored nesse lakehouse, precisaremos gerenciar seu acesso e governança, no unity catalog.
- <img width="1002" height="468" alt="image" src="https://github.com/user-attachments/assets/368aafb5-cd2d-4430-9f0c-b791e3d2f7c2" />


* Delta lake - API/protocolo pra ler e escrever files na nuvem
  * ACID - ou 100% certo, ou faz nada; evita arquivos corrompidos se gravação falhar - o ACID dá suporte a insert, update, delete 
  * Time Travel - histórico de todas as alteraçoes // log de transações, que posso consultar ex.: como estavam os dados na sexta-feira passada -- bom para auditoria e corrigir erros
  * Schema enforcement - dados novos só entram no scehmaa tual se eu permitir a mudança
  * Suporta streaming (dados que chegam em tempo real) e batch (dados que chegam em lote) AO mesmo tempo, na mesma tabela
  * carrega os arquivos externos ocmo DELTA TABLES, suportando arquitetura lake house.
    * DELTA TABELES armazenam os dados externos num diretorio de pastas como arquivo parquet,e  cria delta logs deles (JSON) também.
    * DELTA LOGS mantém registro de toda transaçao/mudança (todo INSERT, DELETE E UPDATE) nos arquivos parquet e versões das tabelas externas que ingerimos pro DTBKS 

---

- The Control Plane "controls" what happens in the platform, and the Compute Plane "computes" the tasks.
  - The Control Plane is the "brains" of the platform, controlling all processing and creating clusters.
  - The Compute Plane is the "muscles" of the platform, and houses all of the physical entities of the platform.

---

- Camadas de dados no Databricks
  - Antes de entrar na bronze, posso remover PIIs - personally identifiable information ao ingerir pro delta lake
  - Bronze / raw: Dados não são exlcuidos da raw para serem usados em futuros projetos; tem colunas de metadata com load time, hora, ID do processo etc.
      - ingerir dados brutos / aw reduz exposição a bugs de sistema ou logica de processos. Com raw data, posso sempre ter acesso ao estado inicial.
  - Silver: filtra, limpa, join e enriquece, define estrutura de dados e reenforça schema 
      - dados ja viram unica fonte de verdade aqui (single source of truth) -- erros corrigidos, dados para business, aplica regras de negocio
      - ex.: tabelas com dados de de entidades unicas como clientes, transacoes etc
  - Gold: dados limpos e de alta qualidade, para consumo, agrega ao negocio dos dados silver, entregue pros processos mais downstream da empresa;
      - tem agregacoes na camada silver, para negocios usar
      - as tabelas podem ser guardadas em formato delta, para usarmos spark jobs ou consultas sql
      - Como uNITIY catalog UC tem suporte para delta parquet etc, as tabelas podem ser lidas por outros app
   
  ---
  
Data importing - metodos
- File Upload UI
  FROM rad_files()
  COPY INTO

  ---

Pipelines de ingestão de dados / DATA INGESTION PIPELINES
- Connect - conecta fontes de dados
- Declarative pipelines - facilita pipelines de dados 
- Jobs - orquestraçào unificada para IA e analytics

- ---
VIEWS
**<img width="566" height="192" alt="image" src="https://github.com/user-attachments/assets/12305e56-d41f-4427-93d1-4e59a9afa6ba" />
**
---

DATABRICKS SQL
- Otimizações preditivas do DBCKS SQL:
  - Inteliigente workload management: ML aplica, direciona querries pras machines corretas,escala up ou down o cluster preditivamente - antes de precisar, prioriza certas querries pra aumentar eficiencia e minimiza latencia
  - dautomatic data layout - dtcs aprende do meu workload pra escolher automaticamente o tamanho de arquivo correto  pra meu banco de dados
  - indezless indexing - 
  -   
QUERRY TOOLS
- Photon - maquina vetorizada , deixa chamadas SQL mais rápidas e reduz meu custo por workload; é o padrão, então está sempre ativo
- Predictive IO- acelera scans seletos em querries SQL que eu fizer. Acelera processos ligados a querreis SQL.
- IWM - Inteligent workload management - melhora o serverless no seu processamento de grandes querries ou quantidade de querries, ajuda o workload a ter as resources necessarias mais rapido. 
