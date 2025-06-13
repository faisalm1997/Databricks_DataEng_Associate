## Data Stream 

A data stream is a data source which grows over time. To process a data stream: 
    - We can reprocess the entire source dataset each time 
    - Only process new data which has been added since the last update (structured streaming)

### Spark structure streaming 

A scalable streaming processing engine, allows you to query an infinite data source which auto detects new data and persists the result into a data sink. A sink is a system which contains files/tables. 

The input data stream can be treated as a table which is unbounded. An input data stream could be a message system like kafka. We can use ```spark.readStream()``` to query the delta table as a stream source. This can create a streaming data frame which we can apply any kind of transformation to. 

We can then ouput the stream data frame to a target table and append with a trigger processing time. 

1. Trigger intervals: This is the specified interval to check for new updated data coming through the data stream. Can be left as unspecified, as a fixed interval, a triggered batch or triggered micro batches. If no trigger interval is specified then the default is 0.5 seconds. ```trigger(processingTime=”500ms")```
2. Output modes: Can append records or 'complete' which is a complete overwrite
3. Checkpointing: Storing the stream state and tracking progress of the stream processing, this log can be output to a path. 

For optimal fault tolerance: add checkpoints and write ahead logs which can record the range of data being processed during each trigger interval. 

Deduping and sorting operations are not supported bu the streaming data frame. Windows and watermarking can be used to support deduping/sorting. 

### Incremental Data Ingestion 

The ability to load new data files which have not been processed since the last ingestion. It reduces the need to re process redundant data. We can use two mechanisms for this: 

    1. COPY INTO - A SQL command which allows for idempotent and incremental loading of new data files, files which have been loaded are skipped. 
    2. Autoloader - Structured streaming, can process billions of files and supports near real time ingestion of millions of files per hour. Auto loader uses checkpointing to store metadata of loaded files and is fault tolerant. Can also be used with Pyspark API. 

### Multi-Hop Architecture 

Also known as medallion architecture, can organise data into a multi-layered approach. The data quality is improved as the data flows through each layer. 

Bronze > Silver > Gold 

Benefits: enables users to build a simple data model, ETL can be incremental at the bronze layer, streaming and batch workloads can operate in a unified pipeline. Can recreate tables from the raw/bronze layer at any time. 