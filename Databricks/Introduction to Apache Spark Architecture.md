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
  Thread refers to thread pooling  
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

<br>

## Transformations, Actions and Executions
### Lazy evaluation
* Spark waits until the last moment to execute a series of operations.
* Instead of modifying the data immediately when you express some operation, you build up a plan of transformations that you will apply to your source data. That plan is executed when you call an action.
* Lazy evaluation makes it easier to parallelize operations and allows Spark to apply various optimizations.

### Transformations
* are at the core of how you express your business logic in Spark. They are the instructions you use to modify a DataFrame to get the results that you want. We call them lazy because they will not be completed at the time you write and execute the code in a cell  - they will only get executed once you have called an action.
* 2 types of transformation:
  - Narrow: data required to compute the records in a single partition reside in at most one partition of the parent dataset; work can be computed and reported back to the executor without changing the way data is partitioned over the system
  - Wide: data required to compute the records in a single partition may reside in many partitions of the parent dataset; require that data be redistributed over the system - this is called a shuffle
* Shuffles are triggered when data needs to move between executors

### Actions
* Statements that are computed *and* executed when they are encountered in the developer's code. They are not postponed or wait for other code constructs. While transformations are lazy, actions are eager.

### Examples
* Transformations (lazy):
  - select
  - distinct
  - groupBy
  - sum
  - orderBy
  - filter
  - limit
* Actions
  - show
  - count
  - collect
  - save

<br>

## Performance
* parallelism (assign work to multiple VMs)
* lazy evaluation (build up plan for data optimization)
* optimization process (Catalyst Optimizer)

### Pipelining
Lazy evaluation allows Spark to optimize the entire pipeline of computations as opposed to the individual pieces. This makes it exceptionally fast for certain types of computation because it can perform all relevant computations at once, rather than having to do one operation for all pieces of data and then the following operation. Additionally, Apache Spark can keep results in-memory.

<br>

### Catalyst Optimizer
Automatically finds the most efficient plan for applying your transformations and actions.
1. User input: SQL Query or DataFrame
2. Unresolved Logical Plan: Logical plan for how to resolve data (yet unresolved)
   - Analysis: column and table names are validated against the catalog
   - Catalog: metadata
   - Logical plan gets resolved
3. Logical Plan
   - Logical Optimization: first set of optimizations - reordering the sequence of commands
4. Optimized Logical Plan
   - Physical Planning: represents what the query engine will actually do after all optimizations have been applied
5. Physical Plans
6. Cost Model: Each physical plan is evaluated according to its cost model. The best performing model is selected. This gives us the selected physical plan.
7. Selected Physical Plan
   - Code generation: compiled to Java bytecode and executed
8. RDDs (resilient distributed dataset: collection of elements partitioned across the nodes of the cluster that can be operated on in parallel)

<br>

### Adaptive Query Execution (AQE)
* allows Spark to re-optimize and adjust query plans based on runtime statistics collected during query execution
* *adaptive planning* from RDD (DAGs) back to Logical Plan: feedback statistics about the size of the data in the shuffle files so that for the next stage, when working out the logical plan, it can dynamically
  - switch join strategies (e.g. use the more performant broadcast-hash join over sort-merge join)
  - coalesce the number of shuffle partitions (coalesce several small partitions into one)
  - optimize skew joins (split skewed partitions into smaller subpartitions)
* AQE is turned off by default, set `spark.sql.adaptive.enabled` to `true`

<br>

### Dynamic Partition Pruning (DPP)
* avoid reading in unnecessary data, read only as much as needed
* *Simple static partition pruning* (filter push-down - filter before scan)
* *Star schema queries* (data is organized into large fact tables and smaller dimension tables)
* *Denormalizing tables* (pro-joining tables + writing to disk --> apply filter to denormalized table)
* DPP auto-optimizes queries and makes them more performant automatically

<br>

### Join Hints
* Can be used to overrid join strategies selected by the catalyst optimizer
* Join strategies:
  - *shuffle sort merge join*: most robust join strategy, requires data be shuffled and sorted (costly operation)
    ```
    SELECT /*+ MERGE(t1) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;
    SELECT /*+ SHUFFLE_MERGE(t1) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;
    SELECT /*+ MERGEJOIN(t2) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;
    ```
  - *broadcast hash join*: one side of the join needs to be small enough to be broadcast, no shuffle or sort --> fast method
    ```
    SELECT /*+ BROADCAST(t1) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;
    SELECT /*+ BROADCASTJOIN(t1) */ * FROM t1 left JOIN t2 ON t1.key = t2.key;
    SELECT /*+ MAPJOIN(t2) */ * FROM t1 right JOIN t2 ON t1.key = t2.key;
    ```
  - *shuffle hash join*: most basic type of join, no sort, can be used with large tables, must monitor for data skews
    ```
    SELECT /** SHUFFLE_HASH(t1) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;
    ```
  - *shuffle nested loop*: (also: Cartesian Product Join), no shuffle, does an all-pairs comparison between all join keys of each executor, useful for small workloads
    ```
    SELECT /*+ SHUFFLE_REPLICATE_NL(t1) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;
    ```
* Use with caution!

<br>

### Caching
* moving data around is expensive, both in time and money  
  --> avoid overhead by caching

* Caching will place a DataFrame or table into temporary storage  
  --> makes subsequent reads faster
* Caching itself is an expensive operation - use only if dataset is to be reused
* Once data is cached, the Catalyst Optimizer will only reach back to the location where the data was cached

<br>

## Usability Improvements in v3.0

### SQL Improvements
* 32 new built-in functions

* Formatted explain plan (call EXPLAIN FORMATTED on any DataFrame or Table object to see a formatted version of how Spark will execute any given query)
* Better ANSI SQL Compliance (critical for workload migration from other SQL engines to Spark SQL)

<br>

### Structured Streaming
* If you're using Structured Streaming, you will see a Structured Streaming Tab in the Databricks web UI. This can be useful for troubleshooting streaming applications
* Among others input rate and process rate can be seen

<br>

### Panda's UDFs
* UDF: User Defined Function

* 2 API categories:
  - Pandas UDFs
  - Pandas function APIs
* Pandas UDFs allow to define low-overhead, high-performance UDFs entirely in Python and leverage Python type hints
* Pandas function APIs
  - directly apply a Python native function which takes and outputs Pandas instances against a PySpark DataFrame
  - includes: Grouped Map, Map, Co-grouped Map

<br>

### Data Source V2
* Support for more external data stores + allows flexible integration with them

* Data Source V2 provides catalog functionalities

<br>

### Migration from v2 to v3
* Spark Core:
  - Event log files will be written as UTF-8 encoding, *not* the default charset of the driver JVM process
  - SparkConf will reject bogus entries (code will fail if developers set config keys to invalid values)
  - `spark.driver.allowMultipleContexts` is removed

* Spark SQL, Datasets, and DataFrames:
  - for many changes, yoption to restore behavior before Spark 3.0 using `spark.sql.legacy._______`
  - type coercions are performed per ANSI SQL standards when inserting new values into a column. Unreasonable conversions will fail and throw an exception
  - argument order of the trim function has been reversed: `TRIM(trimStr, str) → TRIM(str, trimStr)`
* SparkML
* PySpark: Library updates may be required (Pandas 0.23.2, PyArrow 0.12.1)
* SparR: Some SparkR methods have been modified to better match other APIs
