# Introduction to Apache Spark Architecture

## General concepts
* **Driver**: instructor, doesn't touch the data

* **Executors**: space in which work can be done / operations are executed, a Java VM, touch the data
* **Spark Cluster**: 1 driver and many executors (nodes)  
  In Databricks: one executor on one node  
  (In YARN: sometimes multiple executors in a single node)
* **Slots/Threads/Cores**: driver has a slot on which it assigns a task to execute that one thread
  - Core refers to the hardware
  - Thread  refers to thread pooling
  - Slot is spot on which the driver can assign a task
* **Shared Resources**: Threads are sharing resources (RAM, Disk, Network)  
  Thread can have side-effects on other threads within the same executor
* **Parallelization**:  
  e.g. 12 Tasks (6 executors, 2 cores each)
* **Dataset**
* **Partitions**: Driver decides on how to partition the data.   
  Once partitioned --> create task for each partition  
  Once partitions are set, they are never subdivided
* **Spark Job** (with only 1 stage): Instructions
* **Spark Application**: body of code in Dataframes API, SparkSQL or PySpark that is submitted to the driver via `spark-submit`

<br>

* **Application**: consists of one ore more stages
* **Stage**: consists of zero or more jobs
* **Job**: Consists of one or more tasks
* **Task**: A single unit of work against a single partition of data

<br>

