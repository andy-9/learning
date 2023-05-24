# Introduction to Apache Spark Architecture

## General concepts
* **Driver**: instructor, runs the Spark app, assigns tasks to slots in an executor, coordinates the work between tasks  
  receives the result if any  
  doesn't touch the data  
  runs in a Java VM  
  runs in a node

* **Executors**: provides an environment in which tasks can be run / operations are executed  
  run in Java VMs  
  Leverage the JVM to execute many jobs  
  Touch the data  
  Share machine level resources
* **Spark Cluster**: group of nodes  
  1 driver and many executors (nodes)  
  In Databricks: one executor on one node  
  (In YARN: sometimes multiple executors in a single node)
* **Nodes**: individual machines within a cluster (generally a VM)
* **Slots/Threads/Cores**: the lowest unit of parallelization  
  Execute a set of transformations against a partition as directed to by the driver  
  Driver has a slot on which it assigns a task to execute that one thread
  - Core refers to the hardware
  - Thread  refers to thread pooling
  - Slot is spot on which the driver can assign a task (most accurate term)
* **Shared Resources**: Threads are sharing resources (RAM, Disk, Network)  
  Thread can have side-effects on other threads within the same executor
* **Parallelization**:
  - scale horizontally by adding more executors
  - scale vertically by adding cores to each executor (has limits)
  - e.g. 12 Tasks (6 executors, 2 cores each)
* **Dataset**
* **Partitions**:
  - single slice (~128 MB chunk) of a larger dataset
  - each task processes one and only one partition
  - the size and record splits are decided by the driver
  - once partitions are set, they are never subdivided
  - the initial size is partially adjustable with various configuration options
* **Spark Job** (with only 1 stage): Instructions
* **Spark Application**: body of code in Dataframes API, SparkSQL or PySpark that is submitted to the driver via `spark-submit`

<br>

**Workflow**: Driver grabs single partition --> partition creates task --> task is assigned to a slot --> slot on the executor will execute whatever operations were dictated to it by the driver

<br>

Hierarchy into which work is subdivided:
* **Application**: Consists of zero or more jobs
* **Job**: Consists of one ore more stages, one Spark action results in one or more jobs
* **Stage**: Consists of one or more tasks, number of stages depends on the operations submitted with the application
* **Task**: A single unit of work against a single partition of data, shares executor (JVM) level resources

<br>

