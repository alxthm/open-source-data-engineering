# Open-Source Data Engineering
A list of (mostly) open-source tools and resources for data engineering.

## Tools

**Data ingestion**

Collect data from various sources (e.g. databases, APIs of 3rd party applications, etc) and store it. For each external interface/internal data storage solution, *connectors* can be developed to move the data.

- [Meltano](https://meltano.com/): open-source Extract and Load (EL) tool, by gitlab. Low-code solution, but there is also a UI. Uses [Singer](https://www.singer.io/) specification (an open standard with many connectors already available)
- [Airbyte](https://airbyte.io/): similar to Meltano, open-source EL tool with high traction, but no code solution. Also based on Singer, but uses a modified version -> custom connectors can be made (quickly?) with their [Connector Development Kit](https://airbyte.io/connector-development-kit) (CDK)
- [Talend Open Studio](https://www.talend.com/fr/products/talend-open-studio/): French ETL tool, nice graphical user interface, but open-source version  only runs on a local installation (Windows 10/MacOS) ?
- cloud-based, not open-source: Fivetran, Stitch (a product of Talend), Segment

**Data storage** (warehouse/lakehouse)

All the relevant business data can be stored in a data warehouse, data lake, or data lakehouse (difference [here](https://databricks.com/fr/glossary/data-lakehouse)).

- Simple databases: PostgreSQL, MongoDB

- Scaling-up

  - [Greenplum](https://greenplum.org/): open-source massively parallel Postgres for Analytics
  - [Apache Druid](https://druid.apache.org/docs/latest/design/index.html): real-time analytics database designed for fast [OLAP](https://en.wikipedia.org/wiki/Online_analytical_processing) queries, instead of a simple PostgreSQL database
  - [Apache Pinot](https://pinot.apache.org/): "Realtime distributed OLAP datastore, designed to answer OLAP queries with low latency"
  - [ClickHouse](https://clickhouse.com/): "open-source, high performance columnar OLAP database management system for real-time analytics using SQL"
  - Apache HBase: NoSQL distributed database for real-time access to petabytes of data

- **Query Engines** (to keep performant SQL even with massive data lakes)

  - [Delta Lake](https://delta.io/): Query and acceleration engine, based on Apache Arrow. Open source, can be installed on premise
  - [Dremio](https://docs.dremio.com/): "IT-governed self-service semantic layer". Tool on top of Delta Lake to add SQL capabilities. Open source, can be installed on premise

  - [Apache Hive](https://hive.apache.org/): data lake query engine, perform SQL-like queries on massive datasets (petabytes)
    - distributed, fault-tolerant datawarehouse for analytics at massive scale
    - Built on top of Hadoop (YARN, HDFS)
    - HiveQL: leverage Apache Tez or MapReduce for SQL-like queries (?)
    - data warehouse software facilitates reading, writing, and managing large datasets residing in distributed storage using SQL. Structure can be projected onto data already in storage. A command line tool and JDBC driver are provided to connect users to Hive
  - [Trino](https://trino.io/) (ex-PrestoSQL, also a fork of PrestoDB), developed as a replacement to Hive (simpler to deploy and get working). "Can even be seen as an alternative to Snowflake in terms of performance and usability" ([source](https://www.datafold.com/blog/the-modern-data-stack-open-source-edition))

- Cloud data lakes/warehouses (big players, but not open-source, cannot be run on-premise)

  - [Snowflake](https://docs.snowflake.com/en/user-guide/intro-key-concepts.html): initially a warehouse tool, now store all data (including raw data) to allow for ML/data science workflows. loud only, no open source on-premise version
  - [Databricks](https://databricks.com/): initially a data lake tool, now also adds warehouse capabilities to the lake (structured table, reliability, quality, performance) thanks to their open source Delta Lake tool. But requires their Delta Engine (not open source) to do SQL queries on top of the Delta Lake
  - Amazon Resdhift
  - Google BigQuery
  - Panoply: all in one tool, data warehouse + ETL

  > Cloud brings "cost-effective" scaling, flexibility and cheaper storage 
  >
  > /!\ pushing for storing large raw data (ELT) instead of traditional datawarehouses (ETL), makes sense for them, to increase compute costs?
  >
  > Though they let you not worry about db administration, scaling up if needed, setting indexes for performance, worrying about data types, etc

**Data transformation and modeling**

Clean, standardize, and transform data for analysis (e.g. remove empty rows, correct data types, join tables).

- [dbt](https://github.com/dbt-labs/dbt) (data build tool): transform data (T part of ETL) with a project of sql files (-> tests, version control, etc). dbt cloud has a price, but dbt Core is the free, open source, on premise version
- [dataform](https://github.com/dataform-co/dataform): modern data modelling and transformation as well, but open source version is CLI only (web interface is their paid product)
- Other data processing tools (hybrid or streaming, not just batch processing)

**Data analytics** (data-viz, BI)

Aka data visualisation or business intelligence (BI), explore and find insights in the data.

- [Apache Superset](https://superset.apache.org/docs/intro): 40k stars [repo](https://github.com/apache/superset), fully open source, no-code viz builder, looks a bit less polish/user friendly, but many options and visualisations possible. [Video demo](https://www.youtube.com/watch?v=hktHz89Zco4)
- [metabase](https://github.com/metabase/metabase): 26k stars, open-source version, can be self-hosted, easy to use even without knowing SQL, ~15 visualisations
- [redash](https://github.com/getredash/redash): 20k stars, fully open source (no enterprise version), but requires SQL and less options
- [Pentaho Community Edition](https://sourceforge.net/projects/pentaho/): end-to-end platform with both ETL (Pentaho Data Integration) and OLAP queries/dashboards (Pentaho Business Analytics). Looks old-school, and beware of the paid [Enterprise version](https://www.hitachivantara.com/en-us/pdfd/brochure/leverage-open-source-benefits-with-assurance-of-hitachi-overview.pdf) they seem to push hard
- Not open-source: Tableau, [Mode](https://mode.com/) (a bit targeted for data scientists: R/python notebooks), [Looker](https://looker.com/) (by Google)

**Data orchestration tools**

Can orchestrate the whole data stack. They can even serve as ETL tools if writing custom tasks.

- Airflow
- Luigi
- [Prefect](https://www.prefect.io/core/): Python workflow engine, orchestrate external systems (e.g. dbt, dremio, postgresql) or process data directly with *task* functions
- [Dagster](https://dagster.io/): similar to Prefect, but with data aware tasks?

**Other tools**

- Data testing, documentation and profiling
  - [Great Expectations](https://github.com/great-expectations/great_expectations)
- Data Processing - Fundamentals
  - Apache Arrow: a shared foundation for SQL execution engines (Drill, Impala), data analysis systems (Pandas, Spark), streaming and queuing systems (Kafka, Storm), storage systems (Parquet, Kudu, Cassandra, Hbase). Provides a unified data representation, structures, libraries, processing algorithms, for performance
  - Apache Hadoop
    - behind most big data processing tools
    - MapReduce: programming model for processing big data sets with a parallel, distributed algorithm
      - Developers can write massively parallelized operators, without having to worry about work distribution, and fault tolerance
      - However, multi-step process with disk read/write from and to HDFS between each step, which slow down the process
    - HDFS: distributed file system, used for scalable storage
    - YARN (Yet Another Resource Negotiator): a way of managing computing resources used by different applications
- Data Processing - Streaming
  - Apache Kafka
  - Apache Storm
  - Apache Samza
- Data Processing - Hybrid
  - [Apache Spark](https://spark.apache.org/), execution engine
    - [blog post](https://aws.amazon.com/fr/big-data/what-is-spark/)
    - Spark Core: process data in-memory (multiple steps), using DataFrames (an abstraction over the cached data in memory). Greatly lowers latency of Hadoop MapReduce, especially when doing ML and interactive analytics
      - either R, Python, Scala or Java
    - Spark SQL, for interactive queries
      - optimised engine for structured data
      - Integrated with Hive with HiveQL syntax?
      - Connect to existing BI tools through JDBC or ODBC
    - Spark Streaming, for real-time analytics
      - scalable fault-tolerant streaming applications, also support batch queries?
      - Can read data from HDFS, Flume, Kafka, Twitter or other custom data sources
    - Spark MLlib, for ML



## Blog posts

Data Analytics/BI-oriented

- [Traditional data warehouse systems](https://www.ibm.com/cloud/learn/data-warehouse): OLAP vs OLTP, ETL tools (Extract Transform Load), dimensional modelling (star schema, snowflake schema), data mart, data lake
- The "Modern Data Stack": shift to the cloud (cost effective and easily scalable), more flexibility with ELT instead of ETL, modularity (avoid vendor lock-in), more accessible data exploration
  - [The Modern Data Stack](https://www.metabase.com/blog/The-Modern-Data-Stack/index.html) by metabase
  - [ELT and the modern data stack](https://docs.dataform.co/introduction/modern-data-stack), ELT (store raw data in the warehouse, and transform into prepared data after) rather than ETL, by dataform
  - [Open source data stack](https://www.opensourcedatastack.com/), an example of a fully open-source stack, with demos of some tools (meltano, dbt, superset, dagster)
- [ETL history and benchmark](https://www.castordoc.com/blog/etl-benchmark-for-mid-market-companies) by castor
  - ETL 1st gen (1990s): transform data according to business needs before storing in the central data warehouse, due to expensive storage/computation. Requires constant maintenance by data engineers when source formats or business needs change
  - ELT: cloud computing area, cheap and scalable data warehouse (Redshift, BigQuery). Data is loaded into the warehouse without transformations using connectors, and transformations required by business are done after, e.g. using dbt. Data analysts (instead of data engineers) can now make the transformations themselves. Pb: ppl often have to build additional custom connectors, e.g. using Airflow or Python
  - today's ETL: standardize and open-source the development of connectors (otherwise hard to build and to maintain by a single person/company)
- [white paper by the databricks team](http://cidrdb.org/cidr2021/papers/cidr2021_paper17.pdf) (on DeltaLake)
