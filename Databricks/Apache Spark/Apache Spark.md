# Apache Spark Programming with Databricks

## Dataframes

### Databricks ecosystem

* Data lakes: support ML, open formats, big ecosystem
* Data warehouses: good for BI apps, limited support for ML, proprietary systems, SQL interface
* Key enabler: Delta Lake - an open approach to bringing data management and gonvernance to data lakes (reliable, fast processing, data governance at scale)
* Databricks Lakehouse Platform
  - Simple: unify your data, analytics, and AI on one common platform for all data use cases
  - Open: open source standards and formats
  - Collaborative: Unify your data teams (data engineers/scientists/analysts) to collaborate across the entire data and AI workflow

### Spark overview
* De-facto standard unified analytics enginge for big data processing with built-in modules for streaming, SQL, ML, graph processing
* Largest open-source project in data processing
* Fast, easy to use, unified
* Core modules of Spark API:
  - Spark SQL + DataFrames
  - Streaming
  - MLlib (scalable for ML algorithms)
  - Spark Core API: underlying general execution engine
* Spark uses clusters of machines to process big data, distributing the work across several machines
* Spark application  
  --> Jobs (parallelism)  
    --> Stages (set of ordered steps)  
        --> Tasks (created by the driver, assigned a partition of data to process)
* Driver  
  --> Workers  
    --> Executors  
        --> Core (Task)
* Driver: machine in which the app runs. Responsibe for:
  - maintaining information about the Spark app
  - responding to the user's program
  - analyzing, distributing and scheduling work across the executors
* Worker node:
  - hosts the ececutor process
  - has a fixed number of executors allocated at any point in time
* Executor:
  - holds a chunk of the data to be processed
  - chunk is called a Spark partition
  - is a collection of rows that sits on one physical machine on the cluster
  - responsible for carrying out work assigned by the driver
  - responsible for two things:
    - ececute the code assigned by the driver
    - report the state of the computation back to the driver
* Cores:
  - are also known as slots of threads
  - Spark parallelizes at two levels:
    - splitting the work among the executors
    - slot
  - Each executor has a number of slots. Each slot can be assigned a task.
