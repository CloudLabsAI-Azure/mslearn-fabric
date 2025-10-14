#  Exercise 5: Analyze data with Apache Spark

### Estimated Duration: 75 Minutes

In this exercise, you'll explore big data analytics in Microsoft Fabric using Apache Spark and PySpark. You’ll begin by creating a Lakehouse and uploading sales data files. Then, you’ll build a Spark notebook to load and structure the data using defined schemas. Using Spark dataframe operations, you’ll filter, group, and transform data for deeper insights. You’ll save the processed data in Parquet and partitioned formats for optimized performance. Next, you’ll create Delta tables and use both PySpark and SQL to analyze the data. Finally, you’ll visualize insights using Spark charts, matplotlib, and seaborn, and conclude by saving the notebook and ending the Spark session.

## Objectives

In this exercise, you will be able to complete the following tasks:

- Task 1: Create a lakehouse and upload files
- Task 2: Create a notebook
- Task 3: Load data into a dataframe
- Task 4: Explore data in a dataframe
- Task 5: Aggregate and group data in a dataframe
- Task 6: Use Spark to transform data files

  - Task 6.1: Use dataframe methods and functions to transform data
  - Task 6.2: Save the transformed data
  - Task 6.3: Save data in partitioned files

- Task 7: Work with tables and SQL

  - Task 7.1: Create a table
  - Task 7.2: Run SQL code in a cell

- Task 8: Visualize data with Spark

  - Task 8.1: View results as a chart
  - Task 8.2: Get started with **matplotlib**
  - Task 8.3: Use the **seaborn** library

- Task 9: Save the notebook and end the Spark session

## Task 1: Create a lakehouse and upload files

In this task, you will create a lakehouse to organize and analyze your data files. After setting up your workspace, you'll switch to the Data Engineering experience in the portal to initiate the creation of the data lakehouse.

1. From the left navigation pane, select your **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** workspace, click on **+ New item (2)** to create a new Lakehouse.

    ![](./Images/p6t1p1.png)

1. In the search box, search **Lakehouse (1)** and select **Lakehouse (2)**.

    ![](./Images/E5T1S2-1208.png)
   
1. Enter the following details to create a Lakehouse. 
   
   - **Name:** Enter **fabric_lakehouse<inject key="DeploymentID" enableCopy="false"/> (1)**

   - Click on **Create (2)**

     ![](./Images/p6t1p3.png)

1. Once you're in the lakehouse, navigate to the **Files** folder in the **Explorer** pane, click on the **Ellipses ... (1)** menu for **Files**, and select **Upload (2)** followed by **Upload folder (3)**.

    ![](./Images/E5T1S4-1208.png)

1. On the Upload folder pane, click on the **Browse (1)** option. Go to the **C:\LabFiles\Files\ (2)** location, select the **orders (3)** folder, and click on **Upload (4)**. 

    ![](./Images/p6t1p5.png)

1. On the **Upload 3 files to this site?** pop-up, click on **Upload**.

    ![](./Images/p6t1p6.png)

1. Click on **Upload** after adding the `orders` folder.

    ![](./Images/p6t1p7.png)

1. After the files have been uploaded, expand **Files** and select the **orders** folder; and verify that the CSV files have been uploaded, as shown here:

    ![Screenshot of uploaded files in a lakehouse.](./Images/E5T1S6.png)

## Task 2: Create a notebook

In this task, you will create a notebook to work with data in Apache Spark. Notebooks provide an interactive environment where you can write and run code in multiple languages, while also allowing you to add notes for documentation.

1. From the left navigation pane, select your **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** workspace, click on **+ New item (2)** to create a new notebook.

    ![](./Images/E1T3S2-1108.png)

1. In the search box, search **Notebook (1)** and select **Notebook (2)**.

    ![](./Images/E4T2S2-1208.png)

    > **Note:** After a few seconds, a new notebook containing a single *cell* will open. Notebooks are made up of one or more cells that can contain *code* or *markdown* (formatted text).

1. In the **Explorer** tab, click on **Data items (1)**, then select **Add data items (2)** drop-down and choose **Existing data sources (3)**.

    ![.](./Images/p6t2p3.png)

1. Select **fabric_lakehouse<inject key="DeploymentID" enableCopy="false"/> (1)** from the options and click on **Connect (2)**.
    
    ![](./Images/E5T2S4-1208.png)

    > **Note:** If required, reload the page to get the files and folders.

1. Select the first cell (which is currently a *code* cell), and then in the dynamic toolbar at its top-right, use the **M&#8595;** button to convert the cell to a **Markdown** cell.

    ![.](./Images/p6t2p5.png)

1. Use the **&#128393;** **(Edit)** button to switch the cell to editing mode, then modify the markdown as follows:

    ```
   # Sales order data exploration

   Use the code in this notebook to explore sales order data.
    ```

1. Click anywhere in the notebook outside of the cell to exit editing mode and view the rendered markdown.

## Task 3: Load data into a dataframe

In this task, you will load data into a dataframe to prepare it for analysis. Dataframes in Spark, similar to Pandas dataframes in Python, offer a structured way to manage and manipulate data organized in rows and columns.

> **Note:** Spark supports multiple coding languages, including Scala, Java, and others. In this exercise, we'll use *PySpark*, which is a Spark-optimized variant of Python. PySpark is one of the most commonly used languages on Spark and is the default language in Fabric notebooks.

1. With the notebook open, expand the **Files (1)** list and select the **orders (2)** folder to display the **CSV files (3)** alongside the notebook editor, as shown here:

    ![.](./Images/p6t3p1.png)

1. In the **orders** folder, right click on the **2019.csv (1)**, then select **Load data (2)** and then select **Spark (3)**.

   ![](./Images/E5T3S2-1208.png)

1. A new code cell containing the following code should be added to the notebook:

    ```python
   df = spark.read.format("csv").option("header","true").load("Files/orders/2019.csv")
   # df now is a Spark DataFrame containing CSV data from "Files/orders/2019.csv".
   display(df)
    ```

    > **Note:** You can hide the Lakehouse Explorer panes on the left by using their **<<** icons. Doing so will help you focus on the notebook.

1. Use the **&#9655; Run cell** button on the left of the cell to run it.

    > **Note:** Since this is the first time you've run any Spark code, a Spark session must be started. This means that the first run in the session can take a minute or so to complete. Subsequent runs will be quicker.

    > **Note:** The Spark pool limit has been reached because there are existing jobs running. Wait a few minutes for the current jobs to complete, and then you can proceed.

1. When the cell command has been completed, review the output below the cell, which should look similar to this:

    | Index | SO43701 | 11 | 2019-07-01 | Christy Zhu | christy12@adventure-works.com | Mountain-100 Silver, 44 | 16 | 3399.99 | 271.9992 |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | 1 | SO43704 | 1 | 2019-07-01 | Julio Ruiz | julio1@adventure-works.com | Mountain-100 Black, 48 | 1 | 3374.99 | 269.9992 |
    | 2 | SO43705 | 1 | 2019-07-01 | Curtis Lu | curtis9@adventure-works.com | Mountain-100 Silver, 38 | 1 | 3399.99 | 271.9992 |
    | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

1. The output shows the rows and columns of data from the 2019.csv file. However, note that the column headers don't look right. The default code used to load the data into a dataframe assumes that the CSV file includes the column names in the first row, but in this case, the CSV file just includes the data with no header information.

1. Modify the code to set the **header** option to **false** as follows:

    ```python
   df = spark.read.format("csv").option("header","false").load("Files/orders/2019.csv")
   # df now is a Spark DataFrame containing CSV data from "Files/orders/2019.csv".
   display(df)
    ```

1. Re-run the cell and review the output, which should look similar to this:

   | Index | _c0 | _c1 | _c2 | _c3 | _c4 | _c5 | _c6 | _c7 | _c8 |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | 1 | SO43701 | 11 | 2019-07-01 | Christy Zhu | christy12@adventure-works.com | Mountain-100 Silver, 44 | 16 | 3399.99 | 271.9992 |
    | 2 | SO43704 | 1 | 2019-07-01 | Julio Ruiz | julio1@adventure-works.com | Mountain-100 Black, 48 | 1 | 3374.99 | 269.9992 |
    | 3 | SO43705 | 1 | 2019-07-01 | Curtis Lu | curtis9@adventure-works.com | Mountain-100 Silver, 38 | 1 | 3399.99 | 271.9992 |
    | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

1. Now the dataframe correctly includes the first row as data values, but the column names are auto-generated and not very helpful. To make sense of the data, you need to explicitly define the correct schema and data type for the data values in the file.

1. Modify the code as follows to define a schema and apply it when loading the data:

    ```python
    from pyspark.sql.types import *

    orderSchema = StructType([
        StructField("SalesOrderNumber", StringType()),
        StructField("SalesOrderLineNumber", IntegerType()),
        StructField("OrderDate", DateType()),
        StructField("CustomerName", StringType()),
        StructField("Email", StringType()),
        StructField("Item", StringType()),
        StructField("Quantity", IntegerType()),
        StructField("UnitPrice", FloatType()),
        StructField("Tax", FloatType())
        ])
   
    df = spark.read.format("csv").schema(orderSchema).load("Files/orders/2019.csv")
    display(df)
    ```

1. Run the modified cell and review the output, which should look similar to this:

    | Index | SalesOrderNumber | SalesOrderLineNumber | OrderDate | CustomerName | Email | Item | Quantity | UnitPrice | Tax |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | 1 | SO43701 | 11 | 2019-07-01 | Christy Zhu | christy12@adventure-works.com | Mountain-100 Silver, 44 | 16 | 3399.99 | 271.9992 |
    | 2 | SO43704 | 1 | 2019-07-01 | Julio Ruiz | julio1@adventure-works.com | Mountain-100 Black, 48 | 1 | 3374.99 | 269.9992 |
    | 3 | SO43705 | 1 | 2019-07-01 | Curtis Lu | curtis9@adventure-works.com | Mountain-100 Silver, 38 | 1 | 3399.99 | 271.9992 |
    | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

1. Now the dataframe includes the correct column names (in addition to the **Index**, which is a built-in column in all dataframes based on the ordinal position of each row). The data types of the columns are specified using a standard set of types defined in the Spark SQL library, which were imported at the beginning of the cell.

1. Confirm that your changes have been applied to the data by viewing the dataframe. Run the following cell by adding a new code cell from **+ Code** button

    ```python
    display(df)
    ```

1. The dataframe includes only the data from the **2019.csv** file. Modify the code so that the file path uses a \* wildcard to read the sales order data from all of the files in the **orders** folder:

    ```python
    from pyspark.sql.types import *

    orderSchema = StructType([
        StructField("SalesOrderNumber", StringType()),
        StructField("SalesOrderLineNumber", IntegerType()),
        StructField("OrderDate", DateType()),
        StructField("CustomerName", StringType()),
        StructField("Email", StringType()),
        StructField("Item", StringType()),
        StructField("Quantity", IntegerType()),
        StructField("UnitPrice", FloatType()),
        StructField("Tax", FloatType())
    ])

    df = spark.read.format("csv").schema(orderSchema).load("Files/orders/*.csv")
    display(df)
    ```

1. Run the modified code cell and review the output, which should now include sales for 2019, 2020, and 2021.

    >**Note:** Only a subset of the rows is displayed, so you may not be able to see examples from all years.

## Task 4: Explore data in a dataframe

In this task, you will explore data within a dataframe to gain insights and understand its structure. The dataframe object offers various functions for filtering, grouping, and manipulating the data it contains, facilitating effective analysis.

### Filter a dataframe

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code:

    ```Python
   customers = df['CustomerName', 'Email']
   print(customers.count())
   print(customers.distinct().count())
   display(customers.distinct())
    ```

1. Run the new code cell, and review the results. Observe the following details:

    - When you operate on a dataframe, the result is a new dataframe (in this case, a new **customers** dataframe is created by selecting a specific subset of columns from the **df** dataframe)
    - Dataframes provide functions such as **count** and **distinct** that can be used to summarize and filter the data they contain.
    - The `dataframe['Field1', 'Field2', ...]` syntax is a shorthand way of defining a subset of columns. You can also use the **select** method, so the first line of the code above could be written as `customers = df.select("CustomerName", "Email")`

1. Modify the code as follows:

    ```Python
   customers = df.select("CustomerName", "Email").where(df['Item']=='Road-250 Red, 52')
   print(customers.count())
   print(customers.distinct().count())
   display(customers.distinct())
    ```

1. Run the modified code to view the customers who have purchased the *Road-250 Red, 52* product. Note that you can "chain" multiple functions together so that the output of one function becomes the input for the next - in this case, the dataframe created by the **select** method is the source dataframe for the **where** method that is used to apply filtering criteria.

## Task 5: Aggregate and group data in a dataframe

In this task, you will aggregate and group data within a dataframe to summarize information and extract meaningful insights. This process involves applying functions to organize the data based on specific criteria, allowing for easier analysis and reporting.

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code:

    ```Python
   productSales = df.select("Item", "Quantity").groupBy("Item").sum()
   display(productSales)
    ```

1. Run the code cell you added, and note that the results show the sum of order quantities grouped by product. The **groupBy** method groups the rows by *Item*, and the subsequent **sum** aggregate function is applied to all of the remaining numeric columns (in this case, *Quantity*)

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code:

    ```Python
   from pyspark.sql.functions import *

   yearlySales = df.select(year(col("OrderDate")).alias("Year")).groupBy("Year").count().orderBy("Year")
   display(yearlySales)
    ```

1. Run the code cell you added, and note that the results show the number of sales orders per year. Note that the **select** method includes an SQL **year** function to extract the year component of the *OrderDate* field (which is why the code includes an **import** statement to import functions from the Spark SQL library). It then uses an **alias** method to assign a column name to the extracted year value. The data is then grouped by the derived *Year* column, and the count of rows in each group is calculated before finally the **orderBy** method is used to sort the resulting dataframe.

## Task 6: Use Spark to transform data files

In this task, you will use Spark to transform data files into a desired format for analysis and processing. This involves ingesting data in specific structures and applying transformations, a common responsibility for data engineers, to prepare the data for downstream applications.

#### Task 6.1: Use dataframe methods and functions to transform data

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code:

    ```Python
   from pyspark.sql.functions import *

   ## Create Year and Month columns
   transformed_df = df.withColumn("Year", year(col("OrderDate"))).withColumn("Month", month(col("OrderDate")))

   # Create the new FirstName and LastName fields
   transformed_df = transformed_df.withColumn("FirstName", split(col("CustomerName"), " ").getItem(0)).withColumn("LastName", split(col("CustomerName"), " ").getItem(1))

   # Filter and reorder columns
   transformed_df = transformed_df["SalesOrderNumber", "SalesOrderLineNumber", "OrderDate", "Year", "Month", "FirstName", "LastName", "Email", "Item", "Quantity", "UnitPrice", "Tax"]

   # Display the first five orders
   display(transformed_df.limit(5))
    ```

1. Run the code to create a new dataframe from the original order data with the following transformations:
    
    - Add **Year** and **Month** columns based on the **OrderDate** column.
    
    - Add **FirstName** and **LastName** columns based on the **CustomerName** column.
    
    - Filter and reorder the columns, removing the **CustomerName** column.

      ![](./Images/E5T6S4-1208.png)

1. Review the output and verify that the transformations have been made to the data.

1. You can use the full power of the Spark SQL library to transform the data by filtering rows, deriving, removing, renaming columns, and applying any other required data modifications.

    > **Tip:** See the [Spark dataframe documentation](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html) to learn more about the methods of the Dataframe object.

### Task 6.2: Save the transformed data

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code. This code will save the transformed dataframe in Parquet format (Overwriting the data if it already exists):

    ```python
   transformed_df.write.mode("overwrite").parquet('Files/transformed_data/orders')
   print ("Transformed data saved!")
    ```

   >**Note:** If the node fails, rerun it.

1. Run the cell and wait for the message that the data has been saved. Then, in the **Explorer** pane on the left, in the **... (1)** menu for the **Files** node, select **Refresh (2)**; and select the **transformed_data (3)** folder to verify that it contains a new folder named **orders (4)**, which in turn contains one or more Parquet files **(5)**.

    ![Screenshot of a folder containing parquet files.](./Images/p6t6.2p2.png)

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code. This code will load a new dataframe from the parquet files in the **transformed_orders/orders** folder:

    ```python
   orders_df = spark.read.format("parquet").load("Files/transformed_data/orders")
   display(orders_df)
    ```

1. Run the cell and verify that the results show the order data that has been loaded from the parquet files.

### Task 6.3: Save data in partitioned files

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code. This code will save the dataframe, partitioning the data by **Year** and **Month:**

    ```python
   orders_df.write.partitionBy("Year","Month").mode("overwrite").parquet("Files/partitioned_data")
   print ("Transformed data saved!")
    ```

1. Run the cell and wait for the message that the data has been saved. Then, in the **Explorer** pane on the left, in the **... (1)** menu for the **Files** node, select **Refresh (2)**; and expand the **partitioned_data (3)** folder to verify that it contains a hierarchy of folders named **Year=*xxxx***, each containing folders named **Month=*xxxx***. Each month's folder contains a parquet file with the orders for that month.

    ![Screenshot of a hierarchy of partitioned data files.](./Images/p6t6.3p2.png)

1. Partitioning data files is a common way to optimize performance when dealing with large volumes of data. This technique can significantly improve performance and make it easier to filter data.

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code. This code will load a new dataframe from the **orders.parquet** file.

    ```python
   orders_2021_df = spark.read.format("parquet").load("Files/partitioned_data/Year=2021/Month=*")
   display(orders_2021_df)
    ```

1. Run the cell and verify that the results show the order data for sales in 2021. Note that the partitioning columns specified in the path (**Year** and **Month**) are not included in the dataframe.

## Task 7: Work with tables and SQL

As you've seen, the native methods of the dataframe object enable you to query and analyze data from a file quite effectively. However, many data analysts are more comfortable working with tables that they can query using SQL syntax. Spark provides a *metastore* in which you can define relational tables. The Spark SQL library that provides the dataframe object also supports the use of SQL statements to query tables in the metastore. By using these capabilities of Spark, you can combine the flexibility of a data lake with the structured data schema and SQL-based queries of a relational data warehouse - hence the term "data lakehouse".

### Task 7.1: Create a table

Tables in a Spark metastore are relational abstractions over files in the data lake. Tables can be *managed* (in which case the files are managed by the metastore) or *external* (in which case the table references a file location in the data lake that you manage independently of the metastore).

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code. This code will save the dataframe of sales order data as a table named **salesorders:**

    ```Python
   # Create a new table
   df.write.format("delta").saveAsTable("salesorders")

   # Get the table description
   spark.sql("DESCRIBE EXTENDED salesorders").show(truncate=False)
    ```

    > **Note:** It's worth noting a couple of things about this example. Firstly, no explicit path is provided, so the files for the table will be managed by the metastore. Secondly, the table is saved in **delta** format. You can create tables based on multiple file formats (including CSV, Parquet, Avro, and others), but *delta lake* is a Spark technology that adds relational database capabilities to tables, including support for transactions, row versioning, and other useful features. Creating tables in delta format is preferred for data lakehouses in Fabric.

1. Run the code cell and review the output, which describes the definition of the new table.

1. In the **Explorer** pane, in the **... (1)** menu for the **Tables** folder, select **Refresh (2)**. Then expand the **Tables** node and verify that the **salesorders (3)** table has been created.

   ![Screenshot of the salesorder table in Explorer.](./Images/E5T7.1S3-1208new.png)

   ![Screenshot of the salesorder table in Explorer.](./Images/E5T7.1S3-1208new2.png)

1. Right click on the **salesorders (1)** table, select **Load data (2)** and click on **Spark (3)**. A new code cell containing code similar to the following example is added to the notebook:

    ```Python
   df = spark.sql("SELECT * FROM [your_lakehouse].salesorders LIMIT 1000")
   display(df)
    ```

   ![](./Images/E5T7.1S4-1208.png)

     > **Note:** Ensure to replace the [your_lakehouse] with **fabric_lakehouse<inject key="DeploymentID" enableCopy="false"/>** if you are copying the code from above step.
    
1. Run the new code, which uses the Spark SQL library to embed a SQL query against the **salesorder** table in PySpark code and load the results of the query into a dataframe.

   ![](./Images/E5T7.1S5-1208.png)

### Task 7.2: Run SQL code in a cell

While it's useful to be able to embed SQL statements into a cell containing PySpark code, data analysts often just want to work directly in SQL.

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code:

    ```SQL
   %%sql
   SELECT YEAR(OrderDate) AS OrderYear,
          SUM((UnitPrice * Quantity) + Tax) AS GrossRevenue
   FROM salesorders
   GROUP BY YEAR(OrderDate)
   ORDER BY OrderYear;
    ```

1. Run the cell and review the results. Observe that:
    
    - The `%%sql` line at the beginning of the cell (called a *magic*) indicates that the Spark SQL language runtime should be used to run the code in this cell instead of PySpark.
    - The SQL code references the **salesorders** table that you created previously.
    - The output from the SQL query is automatically displayed as the result under the cell.

       ![](./Images/p6t7.2p2.png)

        > **Note:** For more information about Spark SQL and dataframes, see the [Spark SQL documentation](https://spark.apache.org/docs/2.2.0/sql-programming-guide.html).

## Task 8: Visualize data with Spark

In this task, you will visualize data using Spark to enhance understanding and insights through graphical representation. While Fabric notebooks offer a basic chart view for data from dataframes or Spark SQL queries, you can utilize Python graphics libraries like **matplotlib** and **seaborn** for more comprehensive and customized charting.

### Task 8.1: View results as a chart

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code:

    ```SQL
   %%sql
   SELECT * FROM salesorders
    ```

1. Run the code and observe that it returns the data from the **salesorders** view you created previously.

   ![](./Images/E5T8.1S2-1208.png) 

1. We need to change the view from table to chart in the results section beneath the cell.  Click on **+ New chart**.

    ![](./Images/p6t8.1p3.png) 

1. Click on the **Build my own** button at the bottom right of the suggested charts.

    ![](./Images/p6t8.1p4.png)

1. In the **Chart settings** pane, set the options as follows.

    - **Chart type (1):** Bar chart
    - **X-axis (2):** Item
    - **Y-axis (3):** Quantity
    - **Series Group:** *leave blank*
    - **Aggregation (4):** Sum
    - **Stacked:** *Unselected*

   Verify that the chart looks similar to this:

      ![](./Images/p6t8.1p5.png)
    
      ![](./Images/E5T8.1S5.2-1208.png)

### Task 8.2: Get started with **matplotlib**

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code:

    ```Python
   sqlQuery = "SELECT CAST(YEAR(OrderDate) AS CHAR(4)) AS OrderYear, \
                   SUM((UnitPrice * Quantity) + Tax) AS GrossRevenue \
               FROM salesorders \
               GROUP BY CAST(YEAR(OrderDate) AS CHAR(4)) \
               ORDER BY OrderYear"
   df_spark = spark.sql(sqlQuery)
   df_spark.show()
    ```

1. Run the code and observe that it returns a Spark dataframe containing the yearly revenue.

    ![](./Images/p6t8.2p2.png)

1. To visualize the data as a chart, we'll start by using the **matplotlib** Python library. This library is the core plotting library on which many others are based, and provides a great deal of flexibility in creating charts.

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code:

    ```Python
   from matplotlib import pyplot as plt

   # matplotlib requires a Pandas dataframe, not a Spark one
   df_sales = df_spark.toPandas()

   # Create a bar plot of revenue by year
   plt.bar(x=df_sales['OrderYear'], height=df_sales['GrossRevenue'])

   # Display the plot
   plt.show()
    ```

1. Run the cell and review the results, which consist of a column chart with the total gross revenue for each year. Note the following features of the code used to produce this chart:

    - The **matplotlib** library requires a *Pandas* dataframe, so you need to convert the *Spark* dataframe returned by the Spark SQL query to this format.
    - At the core of the **matplotlib** library is the **pyplot** object. This is the foundation for most plotting functionality.
    - The default settings result in a usable chart, but there's considerable scope to customize it

      ![](./Images/E5T8-2S5.png)

1. Modify the code to plot the chart as follows:

    ```Python
   from matplotlib import pyplot as plt

   # Clear the plot area
   plt.clf()

   # Create a bar plot of revenue by year
   plt.bar(x=df_sales['OrderYear'], height=df_sales['GrossRevenue'], color='orange')

   # Customize the chart
   plt.title('Revenue by Year')
   plt.xlabel('Year')
   plt.ylabel('Revenue')
   plt.grid(color='#95a5a6', linestyle='--', linewidth=2, axis='y', alpha=0.7)
   plt.xticks(rotation=45)

   # Show the figure
   plt.show()
    ```

1. Re-run the code cell and view the results. The chart now includes a little more information.

    ![](./Images/E5T8-2S7.png)

1. A plot is technically contained within a **Figure**. In the previous examples, the figure was created implicitly for you, but you can create it explicitly.

1. Modify the code to plot the chart as follows:

    ```Python
   from matplotlib import pyplot as plt

   # Clear the plot area
   plt.clf()

   # Create a Figure
   fig = plt.figure(figsize=(8,3))

   # Create a bar plot of revenue by year
   plt.bar(x=df_sales['OrderYear'], height=df_sales['GrossRevenue'], color='orange')

   # Customize the chart
   plt.title('Revenue by Year')
   plt.xlabel('Year')
   plt.ylabel('Revenue')
   plt.grid(color='#95a5a6', linestyle='--', linewidth=2, axis='y', alpha=0.7)
   plt.xticks(rotation=45)

   # Show the figure
   plt.show()
    ```

1. Re-run the code cell and view the results. The figure determines the shape and size of the plot. A figure can contain multiple subplots, each on its own *axis*.

    ![](./Images/E5T8-2S10.png)

1. Modify the code to plot the chart as follows:

    ```Python
   from matplotlib import pyplot as plt

   # Clear the plot area
   plt.clf()

   # Create a figure for 2 subplots (1 row, 2 columns)
   fig, ax = plt.subplots(1, 2, figsize = (10,4))

   # Create a bar plot of revenue by year on the first axis
   ax[0].bar(x=df_sales['OrderYear'], height=df_sales['GrossRevenue'], color='orange')
   ax[0].set_title('Revenue by Year')

   # Create a pie chart of yearly order counts on the second axis
   yearly_counts = df_sales['OrderYear'].value_counts()
   ax[1].pie(yearly_counts)
   ax[1].set_title('Orders per Year')
   ax[1].legend(yearly_counts.keys().tolist())

   # Add a title to the Figure
   fig.suptitle('Sales Data')

   # Show the figure
   plt.show()
    ```

1. Re-run the code cell and view the results. The figure contains the subplots specified in the code.

    ![](./Images/E5T8-2S12.png)

    > **Note:** To learn more about plotting with matplotlib, see the [matplotlib documentation](https://matplotlib.org/).

### Task 8.3: Use the **seaborn** library

While **matplotlib** enables you to create complex charts of multiple types, it can require some complex code to achieve the best results. For this reason, over the years, many new libraries have been built based on matplotlib to abstract its complexity and enhance its capabilities. One such library is **seaborn**.

1. Add a new code cell in the notebook using **+ Code**. Then, in the new cell, add the following code:

    ```Python
   import seaborn as sns

   # Clear the plot area
   plt.clf()

   # Create a bar chart
   ax = sns.barplot(x="OrderYear", y="GrossRevenue", data=df_sales)
   plt.show()
    ```

1. Run the code and observe that it displays a bar chart using the seaborn library.

    ![](./Images/E5T8-3S2.png)

1. Modify the code as follows:

    ```Python
   import seaborn as sns

   # Clear the plot area
   plt.clf()

   # Set the visual theme for seaborn
   sns.set_theme(style="whitegrid")

   # Create a bar chart
   ax = sns.barplot(x="OrderYear", y="GrossRevenue", data=df_sales)
   plt.show()
    ```

1. Run the modified code and note that seaborn enables you to set a consistent color theme for your plots.

1. Modify the code again as follows:

    ```Python
   import seaborn as sns

   # Clear the plot area
   plt.clf()

   # Create a bar chart
   ax = sns.lineplot(x="OrderYear", y="GrossRevenue", data=df_sales)
   plt.show()
    ```

1. Run the modified code to view the yearly revenue as a line chart.

    ![](./Images/p6t8.3p6.png)

    > **Note:** To learn more about plotting with seaborn, see the [seaborn documentation](https://seaborn.pydata.org/index.html).

## Task 9: Save the notebook and end the Spark session

In this task, you will save your notebook with a meaningful name to preserve your work after processing the data. Additionally, you will end the Spark session to free up resources and complete your data engineering tasks.

1. In the top left corner, set the **Name** of the notebook from **Notebook 1 (1)** to **Explore Sales Orders Notebook (2)**.

    ![](./Images/Spark8.png)

1. On the notebook menu, select **Stop session** to end the Spark session.

    ![](./Images/Spark9.png)

    > **Note:** The stop session icon is present next to the **Standard Session** option.

## Summary

In this exercise, you:

- Created a **Lakehouse** and uploaded data files.
- Built a **Spark notebook** to explore and process data using **PySpark**.
- Loaded and structured data with a defined schema.
- Performed **data exploration**, **filtering**, and **aggregation** using dataframe operations.
- Transformed and saved data in **Parquet** and **partitioned** formats.
- Created and queried **Spark SQL tables** using both PySpark and SQL.
- Visualized data using **Spark charts**, **matplotlib**, and **seaborn**.
- Saved the notebook and ended the Spark session.

### You have successfully completed the exercise. Click on Next >> to proceed with the next exercise.

   ![05](./Images/next-page-1208.png)
