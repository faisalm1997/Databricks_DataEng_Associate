## Delta Live Tables DLT 

When creating a production pipeline in databricks, we need to create DLTs. We also need to conform to the bronze, silver and gold layers (medallion architecture). 

Users can apply CONSTRAINTS to their queries in the notebooks and also DROP ROWS ON VIOLATION. 

e.g. ```CONSTRAINT valid_id EXPECT (id IS NOT NULL) ON VIOLATION FAIL UPDATE/DROP ROW```

Users would need to use the ```LIVE.``` prefix to refer to other DLT tables which will be used in the query. 

Users can deploy notebooks with DLTs to a workflow and configure a pipleine to run. For testing, it is advised to use the development mode. When ready, users can switch to the production mode and also see the data lineage graph (DAG).

Note: the system directory captures all points of the pipeline. The events which have occurred, checkpoints, tables and how the system is behaving. 

## Change Data Capture CDC

CDC refers to the process of identifying changes made to the data in source and delivering those changes to the target. Such as inserts, updates and deletes at source need to be delivered and shown in target. 

1. Row level changes: inserting new records, updating and deleting existing records. The CDC feed shows the row data and metadata in databricks with the operation which has been carried out. 

2. The APPLY CHANGES INTO command orders late arriving records using sequencing keys. The default assumption from this command is that the rows will contain only INSERTS and UPDATES. 
    - Can APPLY AS DELETE WHEN to take into account application of deletions 
    - EXCEPT can be used to ignore columns 

The disadvantage: Since data is being deleted and updated in the target table, this breaks the append requirement. This means users will no longer be able to use the updated table as a streaming source later in the other layers of the medallion architecture. 

## Jobs 

Databricks will allow you to create JOBS with TASKS which can run a series of tables or processes which would originally be run in a notebook. 

We can add notebooks which have individual commands, queries and then run them as tasks in a job. Users can also set email notifcations and add concurrent runs, set a schedule on the jobs. 

Users can also repair the job run by re running only the failed tasks - we dont have to run the full end to end job again. 

## Databricks SQL 

Also known as dbSQL, users can query the data lake, visualise the results and easily share insights with their teams. Users can create warehouses, notebooks and dashboard using SQL (similar to snowflake) to do analytics and showcase insights. 

Users can also share dashboards and the underlying datasets to others and also configure permissions to the datasets. We can also set alerts when a query exceeds or goes under a certain threshold. 