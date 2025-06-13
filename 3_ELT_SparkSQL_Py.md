# ETL Process

## Step 1: Querying Files 

Users are able to query files directly by ```SELECT * FROM file_format.'path_to_file'```, this query is useful with self describing formats such as json, parquet. Not useful with non self describing fornats such as csv, tsv. 

Users can use the text format to extract text from files as strings 
```SELECT * FROM text.'path_to_file'```

## Step 2: CTAS 

Creating tables from files which have been ingested using CTAs is only useful for parquet and self describing formats.

```CREATE TABLE table_name AS SELECT * FROM file_fomat.'path_to_file'```

If a table needs to be created using external data sources you will create an external table which just points to the data stored in an external location and is not a delta table. 

```CREATE TABLE table_name USING CSV OPTIONS (header = "TRUE", delimiter=";") LOCATION'path'```

```CREATE TABLE table_name USING JDBC OPTIONS (url="url_database", dbtable="db_table", user="username", password="pwd")```

We cannot guarantee that the external tables will behave in the same manner as DLT, having a huge database to read from can cause performance issues. 

Solution: When performance matters on external tables, CREATE a TEMP VIEW USING data_source

## Step 3: Transformations 

After tables have been ingested into a landing_zone, users can then transform and clean the data ready to be transported to a reporting layer. 

Users can build temp views using CSV format and specify options to make sure the data is well parsed - we can then create a DLT based on the temp view. 

## Writing to Tables 

1. Overwrite a table: Sometimes it is easier to overwrite a table using CRAS statements, ```CREATE OR REPLACE TABLE AS...``` as databricks does not then need to list all items in a table, delete and then re write which is compute intensive. (DDL operation)

2. Update a table: Users can also use ```INSERT OVERWRITE``` to replace data in a table without altering the schema (DML operation) - useful when the user wants to refresh the data in a table. 

3. Append records to table: ```INSERT INTO table SELECT * FROM parquet.'path_to_file'``` - this query does not save the user from inserting duplicates 

4. Merge records to table: ```MERGE INTO table USING temp_view ON id = id WHEN MATCHED AND ... UPDATE WHEN NOT MATCHED THEN INSERT *``` - the merge statement saves the table from having duplicates. 

5. Struct type: Represents values with the structure described by a sequence of fields. Users can create struct data types from JSON objects and then use it. The star operation (*) can be used in the struct to flatten fields which have been nested into seperate columns (exploded nests). 

## Higher order functions, UDFs

1. TRANSFORM: the transform function transforms elements in an array into an expression.
2. FILTER: the filter function filter an array into an expression.
3. UDF: Allows you to reuse and share code with built in functionality. 