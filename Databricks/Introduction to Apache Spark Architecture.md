# Introduction to Apache Spark Architecture
Spark uses clusters of machines to process big data by breaking a large task into smaller ones and distributing the work among several machines. 

<br>

## High-level components
### Driver
- 3 main tasks:
    - maintaining information about the Spark app
    - responds to the user's program  
    - analyzes + distributes + schedules work across the executors, instructor, assigns tasks to slots in an executor, coordinates the work between tasks
- receives the result if any  
- doesn't touch the data  
- runs in a Java VM  
- runs in a node

### Executors
- provides an environment in which tasks can be run / operations are executed  
- Each executor will hold a chunk of the data to be processed --> Spark partition  
- Responsible for 2 things:
    * execute code assigned by the driver
    * report the state of the computation back to the driver    
- have a number of slots  
- run in Java VMs  
- Leverage the JVM to execute many jobs  
- Touch the data  
- Share machine level resources

### Spark Cluster
* group of nodes  
* 1 driver and many executors (nodes)  
  In Databricks: one executor on one node  
  (In YARN: sometimes multiple executors in a single node)

### Nodes
* individual machines within a cluster (generally a VM)

### Shared Resources
* Threads are sharing resources (RAM, Disk, Network)  
* Thread can have side-effects on other threads within the same executor

### Parallelization
* Two levels of parallelization:
  - scale horizontally by adding more executors
  - scale vertically by adding cores to each executor (has limits)  
  - e.g. 12 Tasks (6 executors, 2 cores each)

### Workflow 
* Driver grabs single partition  
  --> partition creates task  
  --> task is assigned to a slot  
  --> slot on the executor will execute whatever operations were dictated to it by the driver

### Partitions
- single slice (~128 MB chunk) of a larger dataset
- collection of rows that sit on one physical machine in cluster
- each task processes one and only one partition
- the size and record splits are decided by the driver
- once partitions are set, they are never subdivided
- the initial size is partially adjustable with various configuration options

### Task
* created by the driver
* assigned a partition of data to process
* are assigned to slots for parallel execution
* single unit of work
* shares executor (JVM) level resources

### Slots/Threads/Cores
* The lowest unit of parallelization  
* Execute a set of transformations against a partition as directed to by the driver  
* Driver has a slot on which it assigns a task to execute that one thread
* Core refers to the hardware  
  Thread  refers to thread pooling  
  Slot is spot on which the driver can assign a task (most accurate term). Some slots may have tasks, others my be open/waiting

### Spark Job (with only 1 stage)
* One Spark action results in one or more jobs
* each parallelized action is referred to as a job
* each job is consists of one ore more stages

### Stage
* Set of ordered steps that together accomplish a job
* consists of one or more tasks
* number of stages depends on the operations submitted with the application

### Spark Application
* Consists of zero or more jobs
* Body of code in Dataframes API, SparkSQL or PySpark that is submitted to the driver via `spark-submit`

<br>

## Stages
* Stage Boundry: Every task in current stage needs to be completed before moving on to the next stage
* One long-running task can delay an entire stage from completing
* Spark 3.x can run some stages in parallel (e.g. inputs to a join)
* The shuffle between two stages is one of the most expensive operations in Apache Spark

<br>

## Shuffling
* Shuffling is the process of rearranging data withing a cluster between stages
* Triggered by wide operations (comparing against entire data set):
  - Re-partitioning
  - ByKey operations (except counting)
  - Joins, the worse being cross joins
  - Sorting
  - Distinct (e.g. reduce duplicates)
  - GroupBy
* Narrow operations (can execute without any regard to the rest of the data, e.g. transforming `red` to `FF0000`):
  - Filter
  - Count
  - Coalesce
  - Union
  - Sample
  - Map
* Try to group wide operations together for the best automatic optimization
  - Aim for: `narrow`, `narrow`, `wide`, `wide`, `wide`, `narrow`
  - Avoid:  `narrow`, `wide`, `narrow`, `wide`, `narrow`, `wide`  
  This is one of the most significant, yet often unavoidable, cause of performance degradation  
  Just because it's slow, it doesn't mean that it's bad
