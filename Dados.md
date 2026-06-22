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

