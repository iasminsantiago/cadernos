Todos os schemas dev sao separados, ja schema prod é a fonte unica de verdade em que o BI é construido. Quando mergeamos, ai sim meu trablho da branch sera incluido na run de producao.

Deployment in dbt (or running dbt in production) is the process of running dbt on a schedule in a deployment environment. The deployment environment will typically run from the default branch (i.e., main, master) and use a dedicated deployment schema (e.g., dbt_prod). The models built in deployment are then used to power dashboards, reporting, and other key business decision-making processes.

Development in dbt is the process of building, refactoring, and organizing different files in your dbt project. This is done in a development environment using a development schema (dbt_jsmith) and typically on a non-default branch (i.e. feature/customers-model, fix/date-spine-issue). After making the appropriate changes, the development branch is merged to main/master so that those changes can be used in deployment.

How do you indicate a deployment environment as a production environment?
Select production in the deployment environment settings.

How this works in dbt Cloud:
When you create or edit a deployment environment in dbt Cloud, there is a specific setting (often a dropdown or a radio button) for Environment Type. By setting this field to Production, you explicitly signal to dbt Cloud that this environment is the definitive "single source of truth" for your data platform.

Creating your Deployment Environment
A deployment environment can be configured in dbt on the Orchestration > Environments page.
General Settings: You can configure which dbt version you want to use and you have the option to specify a branch other than the default branch.
Data Warehouse Connection: You can set data warehouse specific configurations here. For example, you may choose to use a dedicated warehouse for your production runs in Snowflake.
Deployment Credentials:Here is where you enter the credentials dbt will use to access your data warehouse:
IMPORTANT: When deploying a real dbt Project, you should set up a separate data warehouse account for this run. This should not be the same account that you personally use in development.
IMPORTANT: The schema used in production should be different from anyone's development schema.

Which one of the following are true about running your dbt project in production?
Running dbt in production should use a different database schema than I used in development
One of the core design principles of dbt is isolation.

In Development: You work inside a personal sandbox schema (e.g., dbt_jsmith) so you can freely build, alter, and test tables without breaking downstream business intelligence tools or other analysts' work.

In Production: dbt targets a clean, shared, and stable environment (e.g., an analytics or prod schema) that serves as the single source of truth for your organization.

dbt handles this separation natively using the connection profile configurations (profiles.yml or dbt Cloud Environment settings), meaning your underlying SQL code remains exactly the same while targeting completely different database destinations.

Why the other options are incorrect: 
"Running dbt in production should use a different repository than I used in development" -> a is incorrect: You should always use the exact same Git repository for both development and production. Changes are developed on a feature branch, tested, and then merged into the main branch, which the production environment reads from.

"Running dbt in production only supports the `dbt run` command - it does not support `dbt test`" -> d is incorrect: Production environments absolutely support (and heavily rely on) commands like dbt test and dbt build to catch data quality anomalies before they reach end-user dashboards.


---
What is the function of a job in dbt?
the execution of dbt commands in deployment environments.
What is a dbt Job?
In dbt (specifically dbt Cloud), a Job is a scheduled or triggered set of dbt commands that executes automatically within a deployment (production or staging) environment.

While you manually type commands like dbt run or dbt test into a terminal while writing code in your development environment, jobs automate this workflow for production. A job bundles these commands together so they can run on a schedule (e.g., every morning at 6:00 AM) or via a webhook trigger (e.g., when a pull request is merged).

Why the other options are incorrect:
a is incorrect: Automated jobs do not run in development environments. Development environments are strictly interactive playgrounds for data analysts/engineers to build and test code manually.


Conversation with Gemini
Consider this yaml file.



models/staging/jaffle_shop/_stg_tpch.yml



sources:

- name: tpch

database: snowflake_sample_data

schema: tpch_prod

tables:

- name: orders

- name: customers

- name: order_items







How will you correctly select all from customers utilizing the source macro?

To correctly select all data from the customers table using the source macro in dbt, you would use the following SQL syntax in your model file:

SQL
select * from {{ source('tpch', 'customers') }}
How it breaks down:
The source macro takes two arguments: {{ source('source_name', 'table_name') }}.

Argument 1 ('tpch'): Corresponds to the name defined under the sources: block in your YAML file.

Argument 2 ('customers'): Corresponds to the specific table name listed under that source.

When dbt compiles this, it automatically resolves the macro into the proper fully qualified relation name based on your YAML configuration: snowflake_sample_data.tpch_prod.customers.

You have a table in your data platform called raw.jaffle_shop.orders.

You are attempting to run this select statement to preview the 'orders' source:

select * from {{ source( 'jaffle_shop', 'orders') }}



This statement is not running. Examine this YAML configuration :



sources:

- name: jaffle_shop

database: jaffle_shop

schema: raw

tables:

- name: orders

- name: customers

What is the problem with this YAML file?

a.

The source name is `jaffle_shop`, and should replace 'raw' in the source macro.

b.

The database and the schema field should be swapped.

c.

The schema name and database name should match.

d.

The source macro should also include the database name in addition to the schema name.

The correct answer is b. The database and the schema field should be swapped.

Why this is the issue:
According to the problem description, your table is located at raw.jaffle_shop.orders. In standard data warehouse hierarchy (and specifically how dbt interprets it):

raw is the Database (or project)

jaffle_shop is the Schema

However, looking at your current YAML configuration, you have them reversed:

YAML
database: jaffle_shop  # <--- Incorrectly set to the schema name
schema: raw            # <--- Incorrectly set to the database name
Because of this mismatch, dbt compiles the macro into jaffle_shop.raw.orders instead of raw.jaffle_shop.orders, causing the query to fail because the object cannot be found.

The Fix:
Swap the values in your YAML file like this:

YAML
sources:
  - name: jaffle_shop
    database: raw          # Changed to match your actual database
    schema: jaffle_shop    # Changed to match your actual schema
    tables:
      - name: orders
      - name: customers
You have written the following source file, but are encountering a syntax error:

models/staging/jaffle_shop/_stg_analytics.yml

sources:

- name: analytics

database: raw

schema: analytics

tables:

- name: agg_customer_orders__all_time

- name: agg_regions_segments

What went wrong?

a.

The names of tables should come before the name of the source.

b.

The line identifying `agg_customer_orders_all_time` is indented one tab too far.

c.

The database and schema names should always match.

d.

The "tables" key should be indented to match the indentation of `agg_customer_orders__all_time`.

The correct answer is b. The line identifying agg_customer_orders_all_time is indented one tab too far.

Why this causes a syntax error:
YAML is highly sensitive to indentation because it uses spacing to determine the hierarchy and structure of the data.

In a YAML list (denoted by the dash -), all items at the same level of a list must align perfectly. In your snippet, the two tables under the tables block are misaligned:

YAML
    tables:
        - name: agg_customer_orders__all_time  # Indented 8 spaces (Too far!)
      - name: agg_regions_segments             # Indented 6 spaces (Correct)
Because - name: agg_customer_orders__all_time is pushed out further than its sibling element, the YAML parser throws a syntax error because it can't resolve the structure.

The Fix:
Align both table dashes so they are at the exact same indentation level (typically 6 spaces in this dbt structure):

YAML
sources:
  - name: analytics
    database: raw
    schema: analytics
    tables:
      - name: agg_customer_orders__all_time   # Fixed indentation
      - name: agg_regions_segments
What is the purpose of declaring a source in dbt?

a.

Tells dbt the pre-existing data to query from the data platform.

b.

Tells dbt what data to copy from the data platform into dbt.

c.

Tells dbt what data should be stored in tables and not views.

d.

Tells dbt where to store your transformation results.

The correct answer is a. Tells dbt the pre-existing data to query from the data platform.

Why this is the purpose of sources:
In dbt, sources represent the raw data that has already been loaded into your data platform (like Snowflake, BigQuery, or Databricks) by an ingestion tool (like Fivetran, Stitch, or an airbyte pipeline).

Declaring them in a .yml file provides several key benefits:

Abstraction: It allows you to name the raw tables in one central place. If a schema name changes, you only update the YAML file once, rather than updating dozens of SQL models.

Lineage Tracking: It allows dbt to build a visual lineage graph, showing exactly how data flows from your raw source tables all the way to your final analytics tables.

Freshness Testing: It enables you to use the dbt source freshness command to monitor if your raw data pipelines are lagging or broken.

Why the other options are incorrect:
b is incorrect: dbt does not copy, move, or store data inside itself. It executes transformations directly inside your data warehouse.

c is incorrect: Materializations (whether a model is a table or a view) are controlled in the dbt_project.yml file or in a model's {{ config() }} block, not in source declarations.

d is incorrect: Where transformation results are stored is controlled by your target connection profiles and custom schema configurations.

A new model that you are creating needs to reference your `jaffle_orders_information_table`.

Examine this source configuration:



sources:

- name: jaffle_shop

database: raw

schema: jaffle_shop_dataset

tables:

- name: orders

identifier: jaffle_orders_information_table

- name: customers

identifier: jaffle_customers_information_table

How would you reference this information using the source macro?

a.

{{ source('jaffle_shop', 'jaffle_orders_information_table') }}

b.

{{ source('jaffle_shop_dataset', 'orders') }}

c.

{{ source('jaffle_shop', 'orders') }}

d.

{{ source('jaffle_shop_dataset', 'jaffle_orders_information_table') }}

The correct answer is c. {{ source('jaffle_shop', 'orders') }}

Why this is correct:
The source macro in dbt is designed to abstract away the physical names of your database objects so your SQL code remains clean and decoupled from warehouse-specific naming quirks.

The macro strictly expects the following arguments:
{{ source( 'source_name', 'table_name' ) }}

source_name: References the - name: property of the source block (jaffle_shop).

table_name: References the - name: property of the table block (orders).

The role of the identifier:
The identifier property tells dbt: "Hey, in the actual data warehouse, this table is physically called jaffle_orders_information_table, but in my dbt code, I want to call it orders." When dbt compiles {{ source('jaffle_shop', 'orders') }}, it will automatically look at your configuration, see the identifier, and resolve the query to the correct physical name: raw.jaffle_shop_dataset.jaffle_orders_information_table.

nesse exmplo, me mostre de onde ele pega as partes do ref

sources:

- name: jaffle_shop

database: raw

schema: jaffle_shop_dataset

tables:

- name: orders

identifier: jaffle_orders_information_table

- name: customers

identifier: jaffle_customers_information_table

Para esclarecer uma confusão muito comum no dbt: para referenciar arquivos de configuração de fontes (como o seu exemplo), nós usamos a macro source e não a macro ref (que serve para apontar para outros modelos/arquivos SQL que você mesmo criou).

Olhando para o seu YAML, a macro {{ source() }} vai buscar os valores exatamente destas duas propriedades - name:

YAML
sources:
  - name: jaffle_shop                         # <-- 1º Argumento do source
    database: raw
    schema: jaffle_shop_dataset
    tables:
      - name: orders                          # <-- 2º Argumento do source
        identifier: jaffle_orders_information_table
      - name: customers
        identifier: jaffle_customers_information_table
Portanto, no seu arquivo SQL, você vai escrever:

SQL
select * from {{ source('jaffle_shop', 'orders') }}
O que o dbt faz por debaixo dos panos?
Quando o dbt compila o seu código para enviar ao banco de dados, ele faz um "mapeamento". Ele ignora o nome interno que você usa no código (orders) e monta o caminho final usando o database, o schema e o identifier físico do banco.

O mapeamento visual de onde o dbt puxa cada parte funciona assim:

jaffle_shop (1º argumento) → Aponta para o - name: da fonte.

orders (2º argumento) → Aponta para o - name: da tabela.

Ao compilar, o dbt substitui a macro por:
raw.jaffle_shop_dataset.jaffle_orders_information_table

Nota mental: O identifier serve justamente para isso: ele permite que você dê um nome amigável e curto no dbt (orders), enquanto o banco de dados continua usando o nome original e comprido (jaffle_orders_information_table).

Consider the YAML configuration below. Assume this is the only YAML file in the models directory.

sources:

- name: salesforce

tables:

- name: accounts

columns:

- name: account_id

data_tests:

- unique

What data tests will be run in your project?

a.

A unique test will be run on the accounts source table.

b.

A unique test will be run on the salesforce source table.

c.

A unique test will be run on the salesforce model

d.

A unique test will be run on the accounts model 

The correct answer is a. A unique test will be run on the accounts source table.

Why this is correct:
In your YAML configuration, the test is defined within a sources: block, not a models: block.

salesforce is declared as the source name (representing the raw database or schema where the data lands).

accounts is declared under tables:, meaning it represents a specific source table coming from Salesforce.

The unique test is nested directly under the account_id column of that specific source table.

When you run dbt test (or dbt source test), dbt will execute a query against the raw accounts source table in your data warehouse to verify that every value in the account_id column is unique.

Why the other options are incorrect:
b is incorrect: salesforce is the name of the source grouping, not the name of a specific table or column.

c and d are incorrect: This configuration defines a source, not a model. In dbt, models are your transformed tables created by SQL files inside your project, whereas sources are the raw, pre-existing tables.

When a data test is run, what is happening under the hood in dbt?

a.

dbt will compile python to run against models or sources

b.

dbt will compile SQL to run against models or sources

c.

dbt will enforce a constraint on the column of the models or sources

d.

dbt will scan the SQL of your data models to ensure the data materializes correctly

The correct answer is b. dbt will compile SQL to run against models or sources

What happens under the hood?
When you execute dbt test, dbt doesn't actually pull your raw data into its own system, nor does it enforce database-level constraints (like a primary key constraint that blocks inserts).

Instead, it follows these exact steps:

Compiles SQL: dbt translates your YAML test declarations (like unique or not_null) into specific SQL SELECT queries.

Queries for Failures: The compiled SQL query is designed to look specifically for records that violate the test condition (i.e., it looks for bad data). For example, a unique test compiles into a query that counts rows and looks for duplicates.

Evaluates the Result: * If the query returns 0 rows, the test passes.

If the query returns 1 or more rows, the test fails (because it found data that shouldn't be there).

Why the other options are incorrect:
a is incorrect: While dbt supports Python for writing models, standard data tests are natively compiled into and executed as SQL queries.

c is incorrect: dbt does not alter your warehouse tables to enforce constraints (like ALTER TABLE... ADD CONSTRAINT). It only queries the data to see if it meets your criteria.

d is incorrect: Checking the SQL syntax of your models is part of the compilation and parsing phase (or formatting/linting), not what happens when a data test is executing against your warehouse data.

On Monday, you are working in development and run dbt build. Your entire project materializes and tests successfully. You have only accepted values tests on your sources and models.

On Tuesday, you log back in and run dbt run and your models all run. You then run dbt test and find that 5 tests failed.

What is most likely the reason for the tests failing?

a.

The SQL in your models resulted in a compilation error.

b.

The column name in one of your sources that changed.

c.

A new value was introduced on a column you were testing.

d.

A column you were testing now has a duplicate.

The correct answer is c. A new value was introduced on a column you were testing.

Why this is the most likely reason:
The scenario states that you are specifically using accepted values tests. An accepted values test ensures that all data within a specific column matches a predefined list of allowed values (e.g., if a status column should only contain 'ordered', 'shipped', or 'delivered').

Here is why c perfectly explains the timeline:

On Monday: Your source data only contained approved values, so dbt build passed.

On Tuesday: You ran dbt run, which successfully rebuilt your models. This means your SQL code had zero compilation or syntax errors.

The Failure: When you ran dbt test, it failed because the underlying raw data in your warehouse changed between Monday and Tuesday—a new, unexpected value (e.g., 'returned' or a typo like 'shpped') was introduced into the source data, causing the accepted values test to trigger a failure.

Why the other options are incorrect:
a is incorrect: If there was a compilation error in your SQL, your models would have failed during the dbt run step before you even got to dbt test.

b is incorrect: If a column name in a source had changed, the dbt run step would have failed with a "column not found" database error when trying to build the staging models.

d is incorrect: A duplicate value would cause a unique test to fail, but the prompt specifies that you only have accepted values tests configured in your project. An accepted values test doesn't care about duplicates; it only cares if the values themselves are on the approved list.

Consider the YAML block below

sources:

- name: salesforce

tables:

- name: accounts

loaded_at_field: _updated_at

freshness:

warn_after: {count: 1, period: day}

error_after: {count: 7, period: day}

columns:

- name: account_id

tests:

- unique

- not_null

When running `dbt source freshness` what will cause a warning to be flagged?

a.

A duplicate in the account_id column will cause a warning.

b.

A null value in the account_id column will cause a warning.

c.

The greatest value of the _updated_at column is more than 7 days before the current time

d.

The greatest value of the _updated_at column is between 1 and 7 days before the current time.

The correct answer is d. The greatest value of the _updated_at column is between 1 and 7 days before the current time.

How dbt Source Freshness Works
When you execute the command dbt source freshness, dbt does not run your data tests (like unique or not_null). Instead, it calculates the age of your raw data by running a query equivalent to:

SQL
select max(_updated_at) from salesforce.accounts
It then compares that maximum timestamp (the most recent record received) against the current system time.

Based on your YAML configuration:

warn_after: {count: 1, period: day}: If the freshest record is older than 1 day, dbt will flag a Warning.

error_after: {count: 7, period: day}: If the freshest record passes 7 days, the status escalates from a warning to an Error.

Therefore, if the most recent update happened between 1 and 7 days ago, it triggers a warning.

Why the other options are incorrect:
a and b are incorrect: dbt source freshness completely ignores the columns: and tests: blocks. The unique and not_null tests are only evaluated when you run dbt test or dbt build.

c is incorrect: If the greatest value of _updated_at is more than 7 days old, it surpasses the error_after threshold and will flag an Error, not a warning.

What is the dbt best practice for testing your primary keys?

a.

Apply a unique test to your primary keys.

b.

Apply a not_null test to your primary keys.

c.

Apply a unique and not_null test to your primary keys.

d.

Apply a relationships test to your primary keys to ensure referential integrity to a foreign key.

The correct answer is c. Apply a unique and not_null test to your primary keys.

Why this is the best practice:
By definition in relational databases, a primary key must fulfill two strict conditions to reliably identify a unique row in a table:

It cannot contain duplicate values (Unique).

It cannot contain missing or blank values (Not Null).

Because many modern cloud data warehouses (like Snowflake, BigQuery, and Redshift) do not natively enforce primary key constraints when data is loaded, dbt requires you to test and validate these constraints yourself. Applying both tests ensures total data integrity for that entity.

What this looks like in practice:
In your model's .yml file, you should always configure your primary keys with both tests under the column name:

YAML
version: 2

models:
  - name: stg_customers
    columns:
      - name: customer_id
        description: "The primary key for this table"
        data_tests:
          - unique
          - not_null
Why the other options are insufficient:
a is incorrect: A column can have completely unique values but still contain a NULL value, which violates primary key rules.

b is incorrect: A column can have no NULL values but contain thousands of duplicate IDs, which breaks its ability to serve as a primary key.

d is incorrect: A relationships test is used to check foreign keys (ensuring a child table's ID points to an existing ID in a parent table), not to validate the primary key of the table itself.

Assume your project only has models and sources and data tests configured on models and sources. (i.e. there are not snapshots or seeds -- these are beyond the scope of this quiz)

How does the dbt build command work?

a.

dbt build will first test your sources, then materialize all your models, and then test all your models.

b.

dbt build will first test your sources, then materialize and test each model in DAG order.

c.

dbt build will first materialize your models, then test your sources, and then test your models.

The correct answer is b. dbt build will first test your sources, then materialize and test each model in DAG order.

How dbt build works under the hood
The dbt build command is designed to be resource-efficient and to catch data quality issues as early as possible. It executes your project in DAG (Directed Acyclic Graph) order, meaning it follows the strict parent-to-child lineage of your data pipeline from left to right.

The execution sequence follows this logic:

Source Testing: Sources are the starting point of your DAG. Before wasting compute resources transforming raw data, dbt build runs any data tests or freshness checks configured on your sources.

Step-by-Step Materialization & Testing: Once the sources pass, dbt moves down the graph. For each model, it will materialize the model (create the table/view) and then immediately run its tests before moving on to any downstream models that depend on it.

The "Fail-Fast" Benefit
The main advantage of dbt build over running dbt run && dbt test sequentially is its fail-fast nature. If a staging model fails its unique test, dbt build will automatically skip all downstream marts and reports that rely on that model. This prevents bad data from propagating through your warehouse and saves compute costs.

Why the other options are incorrect:
a and c are incorrect: These options describe a linear approach (doing all of one action, then all of another). If dbt built all models before testing them (as in option c), a failure in a raw source or early staging model wouldn't stop bad data from being processed by all subsequent downstream models.

Which command will run data tests only on sources?

a.

dbt test --select source:*

b.

dbt test --select _sources.yml

c.

dbt test --select staging/

d.

dbt source test 

The correct answer is a. dbt test --select source:*

Why this is correct:
In dbt, you use the --select (or -s) flag to filter which nodes you want to execute. The syntax source:* uses a selection method descriptor that tells dbt to select all configured sources in the project.

Running dbt test --select source:* instructs dbt to look at your entire Directed Acyclic Graph (DAG), isolate only the source nodes, and execute the data tests attached to them.

Alternative Shortcut:
You can also use the shorthand command:

Bash
dbt test --select path.to.source_file.yml
...but to select all sources across the entire project globally, source:* is the standard notation.

Why the other options are incorrect:
b is incorrect: You cannot simply target a generic file name like _sources.yml unless you pass the exact file path (e.g., models/staging/_sources.yml), and even then, it would only run tests for the sources defined inside that specific file, not all sources in the project.

c is incorrect: This selects all tests inside a specific directory folder (staging/), which likely includes staging models in addition to sources.

d is incorrect: dbt source test is not a valid dbt command. The command to check sources is dbt source freshness, whereas tests are always initiated using the dbt test or dbt build commands.

You are documenting the columns in your models. Several models have the same column. What documentation type guarantees these columns have a shared description?
The correct answer is c. Doc block

Why this is correct:
A doc block (using Markdown files coupled with the {{ doc('...') }} function) is dbt's built-in solution for DRY (Don't Repeat Yourself) documentation.

If you have a column like customer_id or organization_id that appears across 20 different models, writing a standard string description would force you to copy and paste it 20 times. If the definition changes, you would have to update it in 20 places.

With a doc block, you define the description once in a .md file:

Markdown
{% docs shared_customer_id %}
The unique primary key for a customer, generated sequentially upon account creation.
{% enddocs %}
Then, in your various .yml files, you simply reference that single block:

YAML
models:
  - name: stg_customers
    columns:
      - name: customer_id
        description: "{{ doc('shared_customer_id') }}"

  - name: fct_orders
    columns:
      - name: customer_id
        description: "{{ doc('shared_customer_id') }}"
If the definition ever changes, you only update the Markdown file, and dbt automatically propagates the update to every model documentation page.

Why the other options are incorrect:
a and b are incorrect: Single-line or multi-line string descriptions are hardcoded directly into the YAML file for a single column. They cannot be shared natively across different models without manual copying and pasting.

d is incorrect: While a README.md file is great for providing a high-level overview of a project or folder, dbt cannot parse it to automatically populate column-level descriptions in the generated dbt Docs site.

Which file type are single-line descriptions written in?
The correct answer is b. .yml

Why this is correct:
In dbt, model schemas, configurations, data tests, and standard single-line (or inline multi-line) descriptions are written inside YAML (.yml) files.

These files are typically named schema.yml or _stg_models.yml and sit inside your models/ directory. A standard single-line description looks like this:

YAML
version: 2

models:
  - name: stg_customers
    description: "A staging model for raw customer data."  # <-- Single-line description
    columns:
      - name: customer_id
        description: "The unique primary key for a customer."  # <-- Single-line description
Why the other options are incorrect:
a and d are incorrect: .sql and .py files are used strictly to write the functional transformation logic for your models (SQL queries or Python DataFrames). They do not store metadata properties like dbt descriptions.

c is incorrect: .md (Markdown) files are used for writing doc blocks, which are reserved for complex, long-form, or reusable documentation rather than basic single-line strings.

How do you indicate a deployment environment as a production environment?
The correct answer is b. Select production in the deployment environment settings.

How this works in dbt Cloud:
When you create or edit a deployment environment in dbt Cloud, there is a specific setting (often a dropdown or a radio button) for Environment Type. By setting this field to Production, you explicitly signal to dbt Cloud that this environment is the definitive "single source of truth" for your data platform.

Why this classification matters:
Designating an environment as production unlocks several critical dbt features:

dbt Docs Production Link: dbt Cloud uses this environment to host and serve your official, public-facing project documentation site.

State Comparison (Slim CI): It allows your pull request (CI) jobs to compare deferred state against this specific environment using the --defer --state prod flags. This ensures your CI jobs only test modified models, saving massive compute costs.

Dashboard Integrations: Semantic layer and discovery APIs look at the production environment to fetch metadata.

Why the other options are incorrect:
a is incorrect: You flag the environment as production, not the individual jobs. Multiple jobs (e.g., daily runs, hourly runs, freshness checks) can run inside a single production environment.

c is incorrect: Development environments are meant for individual sandbox work and cannot be marked as the production environment.

d is incorrect: Adding a service account's credentials connects dbt to your data warehouse, but it does not inherently classify the environment's role as "production".

What is the function of a job in dbt?
The correct answer is b. the execution of dbt commands in deployment environments

What is a dbt Job?
In dbt (specifically dbt Cloud), a Job is a scheduled or triggered set of dbt commands that executes automatically within a deployment (production or staging) environment.

While you manually type commands like dbt run or dbt test into a terminal while writing code in your development environment, jobs automate this workflow for production. A job bundles these commands together so they can run on a schedule (e.g., every morning at 6:00 AM) or via a webhook trigger (e.g., when a pull request is merged).

Why the other options are incorrect:
a is incorrect: Automated jobs do not run in development environments. Development environments are strictly interactive playgrounds for data analysts/engineers to build and test code manually.

c is incorrect: This describes the Environment settings, not the Job itself. The environment specifies where the data should go (database, schema, credentials), while the job specifies what commands to run and when to run them.

d is incorrect: Investigating data assets is the function of dbt Explorer or the dbt Docs site, which serve as discovery and data cataloging tools.

Which one of the following are true about running your dbt project in production?
The correct answer is c. Running dbt in production should use a different database schema than I used in development

Why this is essential for production:
One of the core design principles of dbt is isolation.

In Development: You work inside a personal sandbox schema (e.g., dbt_jsmith) so you can freely build, drop, and test tables without breaking anyone else's dashboards or queries.

In Production: dbt targets a clean, shared environment (e.g., a analytics, marts, or prod schema) that serves as the single source of truth for your organization's business intelligence tools.

dbt handles this separation natively using the target.schema variable specified in your connection profiles, meaning you never have to change your underlying SQL code when moving between development and production.

Why the other options are incorrect:
a is incorrect: You should always use the exact same Git repository for both development and production. Changes are developed on a feature branch and merged into the main branch, which production then pulls from.

b is incorrect: You do not need to reconfigure your sources. By utilizing environment variables or configurations based on the deployment target, dbt can automatically redirect your sources to point to production data warehouses.

d is incorrect: Production environments absolutely support (and heavily rely on) dbt test and dbt build to ensure data quality and integrity before or during the materialization of models.



Which one of the following are true about running your dbt project in production?

The correct answer is c. Running dbt in production should use a different database schema than I used in development

Why this is essential for production:
One of the core design principles of dbt is isolation.

In Development: You work inside a personal sandbox schema (e.g., dbt_jsmith) so you can freely build, alter, and test tables without breaking downstream business intelligence tools or other analysts' work.

In Production: dbt targets a clean, shared, and stable environment (e.g., an analytics or prod schema) that serves as the single source of truth for your organization.

dbt handles this separation natively using the connection profile configurations (profiles.yml or dbt Cloud Environment settings), meaning your underlying SQL code remains exactly the same while targeting completely different database destinations.

Why the other options are incorrect:
a is incorrect: You should always use the exact same Git repository for both development and production. Changes are developed on a feature branch, tested, and then merged into the main branch, which the production environment reads from.

b is incorrect: You do not need to reconfigure your source files manually. Using target profiles or environment variables allows dbt to automatically route source paths based on whether you are running in dev or prod.

d is incorrect: Production environments absolutely support (and heavily rely on) commands like dbt test and dbt build to catch data quality anomalies before they reach end-user dashboards.


When does a dbt job run?
Flexibility of dbt Jobs
A dbt job is highly customizable. It does not have a single fixed schedule; instead, you configure exactly when and how often it should execute based on your organization's data needs.

You can set up a job to run using several different triggers:

Schedule-based (Cron): You can set it to run hourly, daily (e.g., every morning at 9 AM), weekly, or on custom cron schedules.

Event-driven (CI/CD): You can trigger a job to run automatically whenever a pull request is opened or updated to test changes before they are merged.

API-driven: You can trigger a job via an API call from an external orchestrator (like Airflow, Prefect, or Dagster) immediately after your upstream data ingestion pipeline completes.




---

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

---

You are documenting the columns in your models. Several models have the same column. What documentation type guarantees these columns have a shared description?
Doc block

Why this is correct:
A doc block (using Markdown files coupled with the {{ doc('...') }} function) is dbt's built-in solution for DRY (Don't Repeat Yourself) documentation.

If you have a column like customer_id or organization_id that appears across 20 different models, writing a standard string description would force you to copy and paste it 20 times. If the definition changes, you would have to update it in 20 places.

With a doc block, you define the description once in a .md file:
```
Markdown
{% docs shared_customer_id %}
The unique primary key for a customer, generated sequentially upon account creation.
{% enddocs %}
```
Then, in your various .yml files, you simply reference that single block:
```
YAML
models:
  - name: stg_customers
    columns:
      - name: customer_id
        description: "{{ doc('shared_customer_id') }}"

  - name: fct_orders
    columns:
      - name: customer_id
        description: "{{ doc('shared_customer_id') }}"
```
If the definition ever changes, you only update the Markdown file, and dbt automatically propagates the update to every model documentation page.

---

Which file type are single-line descriptions written in?
.yml

Why this is correct:
In dbt, model schemas, configurations, data tests, and standard single-line (or inline multi-line) descriptions are written inside YAML (.yml) files.

These files are typically named schema.yml or _stg_models.yml and sit inside your models/ directory. A standard single-line description looks like this:

```
YAML
version: 2

models:
  - name: stg_customers
    description: "A staging model for raw customer data."  # <-- Single-line description
    columns:
      - name: customer_id
        description: "The unique primary key for a customer."  # <-- Single-line description
```
Why the other options are incorrect:
a and d are incorrect: .sql and .py files are used strictly to write the functional transformation logic for your models (SQL queries or Python DataFrames). They do not store metadata properties like dbt descriptions.

c is incorrect: .md (Markdown) files are used for writing doc blocks, which are reserved for complex, long-form, or reusable documentation rather than basic single-line strings.


