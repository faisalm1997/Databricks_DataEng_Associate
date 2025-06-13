Databricks is a 'multi cloud lakehouse platform' based on Apache spark 

A lakehouse is a unified analytics platform which combines a data lake + data warehouse for data engineering, analytics and AI workloads. 

A data lake is open and flexible for semi structured, unstructured data and suitable for ML support. 

A data warehouse is reliable and has strong governance, performance for structured data. 

The data lakehouse platform comrpises of: 
    - Workspaces: for each individual workload ie data engineering, data warehousing, ML 
    - Runtime: the compute is on apache spark, delta lake 
    - Compatible across all 3 cloud providers: Azure, AWS, GCP

The platform also has: 
    - Control plane: this is the databricks account which has the UI, cluster management, worklows and notebooks 
    - Data plane: the customers cloud account which has the VMs, cloud storage (DBFS)

Spark on databricks: 
    - Spark is 'in memory distributed data processing' 
    - All languages supported such as SQL, Scala, Java, Python and R 
    - Supports batch data processing and streaming processing (this is also due to delta lake)
    - Structured, semi structured and unstructured data can be processed

DBFS - Databricks file system 
    - Because spark processes data in distributed manner, we need a distributed file system to hold the data
    - Pre installed in databricks clusters
    - within the abstraction layer, the data can be persisted in the underlying cloud storage, e.g. if a user saves a file in the databricks cluster, the file will appear in cloud storage which is connected to the databricks cluster. 

Databricks can also be used alongside github by setting up github repos and authorizing access to the third party connection. 

Notebooks can also be used to setup adhoc analysis or to create ETL/ELT pipelines.

