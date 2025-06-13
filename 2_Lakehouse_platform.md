# Databricks Lakehouse Platform

## Delta Lake 

The delta lake is an open-source storage framework which brings reliability to data lakes.

Delta lake is a storage framework/layers which enables the building of a lakehouse. It is not a seperate technology which is a storage format/mediun or a database, data warehouse service. 

Delta lake is deployed on the cluster at run time. There is also an accompanying transaction log (delta log). 

Advantages: 
    - Can bring ACID transactions to object storage
    - Handle scalable metadata 
    - Full audit trail of all changes made to files 
    - Builds on standardised file formats of JSON & Parquet

### Delta (transaction log)

- Contains ordered records of each transaction which has been performed on the table (a transaction is a query or commmand)
- Single source of truth
- The JSON file contains commit info:
    - Operation performed + predicates used
    - Data files affected (added and removed)

Users can see the history of the table using ```DESCRIBE HISTORY <table_name>```

Users can see the metadata of the _delta_log using ```%fs ls 'dbfs:/user/hive/warehouse/table_name/_delta_log'```

### Writes/Reads 

1. Read/write Scenario: Writer process writes data to parquet and adds the metadata to the _delta_log, the reader process reads the transacton log from _delta_log and can then read the parquet files containing the data
2. Updating files scenario: When data is updated in a file, a new parquet file is created with the new updated data, there is an additional log in _delta_log which has been added by the writer, the reader can then read the transaction log and read the old and new updated parquet files. 
3. Simulateanous writes/reads scenario:  When there are continuous read/writes, the writer process will write data to a new parquet file, _delta_log will contain a new json file with the metadata after the reader process has the read the previous parquet files. 
4. Failed writes scenario: When there is a failure in the writer process, there are no data files added to storage and nothing added to the _delta_log. Thus the reader will read the transaction _delta_log and continue reading the parquet files which exist. 

## Advanced Delta Lake 

1. Time travel - users can time travel to query older versions of the data. Users can also ```SELECT * FROM table``` using an older timestamp or older version number. 
    - Users can also ROLLBACK and RESTORE TABLE using a timestamp/version.
2. Compaction - delta lake can improve the speed of reading by compacting small files. ```OPTIMIZE my_table ZORDER BY column_name``` can sort and order by id or other foreign/primary keys.
3. Vaccuum - Cleaning up unused data files which are uncommitted and files which are no longer in the latest state. ```VACCUUM table_name``` has a default retention of 7 days but is not equivalent to time travel. 

## Relational Entities

1. Databases = Schemas in the hive metastore. Users can create databases by using both ```CREATE DATABASE db_name``` ```CREATE SCHEMA db_name```. 
2. Hive metastore = Each workspace has a central hive metastore, each db is created in the metastore as a folder and the tables created in the db are stored as sub folders. Users can create dbs, tables outside the metastore by adding ```LOCATION custom_path``` 
3. Tables: 
    - Managed tableS: created under the database directory and dropping the table will delete the underlying data files
    - External tables: created outside the database directory using LOCATION command, dropping the table WILL NOT delete the underlying data files

## Setting up delta tables 

1. CTAS: CREATE TABLE AS SELECT statement creates a table from another table, auto infers the schema information from query results. Users are able to filer and rename columns in CTAS. They can also add partitions, comments and specify the location of the table. 

2. Table constraints: NOT NULL, CHECK constraints can be added by ```ALTER TABLE table_name ADD CONSTRAINT...``` - this is to enhance data quality and also add checks. 

3. Cloning delta tables: 
    - Deep cloning: Full copies of data + metadata from a source table to target, can sync changes and can take a while for larger datasets ```CREATE TABLE table_1 DEEP CLONE source_table``` 
    - Shallow cloning: Just creating a copy of a table and copying delta transaction logs. ```CREATE TABLE table_1 SHALLOW CLONE source_table```

## Views 

A view is a logical query against a source table, ```CREATE VIEW view_x from SELECT ..... FROM source_table```

1. Stored views: A persisted object 
2. Temp view: session scoped view, will be deleted after spark session ends. A spark session can be restarted when opening a new notebook, detaching and attaching a cluster, installing a py package, restarting a cluster. 
3. Global temporary views: cluster scoped view, tied to the cluster and anyone who has access to the cluster can use it. 