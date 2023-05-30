# Apache Spark Programming with Databricks

## Dataframes
Tables are equivalent to Apache Spark DataFrames.  
A DataFrame is an immutable, distributed collection of data organized into named columns.

<br>

### Databricks ecosystem

* Data lakes: support ML, open formats, big ecosystem
* Data warehouses: good for BI apps, limited support for ML, proprietary systems, SQL interface
* Key enabler: Delta Lake - an open approach to bringing data management and gonvernance to data lakes (reliable, fast processing, data governance at scale)
* Databricks Lakehouse Platform
  - Simple: unify your data, analytics, and AI on one common platform for all data use cases
  - Open: open source standards and formats
  - Collaborative: Unify your data teams (data engineers/scientists/analysts) to collaborate across the entire data and AI workflow

<br>

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

<br>

### Databricks overview
- <a href="https://docs.databricks.com/notebooks/notebooks-use.html#language-magic" target="_blank">Magic commands</a>: `%python`, `%scala`, `%sql`, `%r`, `%sh`, `%md`
- <a href="https://docs.databricks.com/dev-tools/databricks-utils.html" target="_blank">DBUtils</a>: `dbutils.fs` (`%fs`), `dbutils.notebooks` (`%run`), `dbutils.widgets`
- <a href="https://docs.databricks.com/notebooks/visualizations/index.html" target="_blank">Visualization</a>: `display`, `displayHTML`
- Use the `%run` magic command to run another notebook within a notebook
- Run shell commands on the driver using the magic command: `%sh`. E.g. `%sh ps | grep 'java'`
- ```
  html = """<h1 style="color:orange;text-align:center;font-family:Courier">Render HTML</h1>"""
  displayHTML(html)
  ```
- Render cell as <a href="https://www.markdownguide.org/cheat-sheet/" target="_blank">Markdown</a> using the magic command: `%md`  

<br>

### Access DBFS (Databricks File System)
The <a href="https://docs.databricks.com/data/databricks-file-system.html" target="_blank">Databricks File System</a> (DBFS) is a virtual file system that allows you to treat cloud object storage as though it were local files and directories on the cluster.

* Run file system commands on DBFS using the magic command: `%fs` (short for `dbutils.fs`)
* output everything in DBFS: `%fs ls`  
  output specific dir, e.g.: `%fs ls /databricks-dataset/`  
  output head of file, e.g.: `%fs head /databricks-datasets/README.md`
* show mount points: `%fs mounts`
* show help: `%fs help`
* %md Visualize results in a table using the Databricks <a href="https://docs.databricks.com/notebooks/visualizations/index.html#display-function-1" target="_blank">display</a> function:
  ```
  files = dbutils.fs.ls("/databricks-datasets")
  display(files)
  ```

<br>

### Create table
* Run <a href="https://docs.databricks.com/spark/latest/spark-sql/language-manual/index.html#sql-reference" target="_blank">Databricks SQL Commands</a> to create a table named `events` using BedBricks event files on DBFS.
  ```
  %sql
  CREATE TABLE IF NOT EXISTS events USING parquet OPTIONS (path "/mnt/training/ecommerce/events/events.parquet");
  ```
* View db name: `print(databaseName)`

<br>

### Query table and plot results
* ```
  %sql
  SELECT * FROM events
  ```
* ```
  %sql
  SELECT traffic_source, SUM(ecommerce.purchase_revenue_in_usd) AS total_revenue
  FROM events
  GROUP BY traffic_source
  ```

<br>

### Add notebook parameters with widgets
* Widgets are ways we can add parameters
* Create a text input widget using SQL:
  ```
  %sql
  CREATE WIDGET TEXT state DEFAULT "CA"
  ```
* %md Access the current value of the widget using the function `getArgument`:
  ```
  %sql
  SELECT *
  FROM events
  WHERE geo.state = getArgument("state")
  ```
* Remove widget:
  ```
  %sql
  REMOVE WIDGET state
  ```
* To create widgets in Python, Scala, and R, use the DBUtils module: `dbutils.widgets`:
  ```
  dbutils.widgets.text("name", "Brickster", "Name")
  dbutils.widgets.multiselect("colors", "orange", ["red", "orange", "black", "blue"], "Traffic Sources")
  ```
* Access the current value of the widget using the `dbutils.widgets` function `get`:
  ```
  name = dbutils.widgets.get("name")
  colors = dbutils.widgets.get("colors").split(",")

  html = "<div>Hi {}! Select your color preference.</div>".format(name)
  for c in colors:
    html += """<label for="{}" style="color:{}"><input type="radio"> {}</label><br>""".format(c, c, c)

  displayHTML(html)
  ```
* Remove all widgets: `dbutils.widgets.removeAll()`
