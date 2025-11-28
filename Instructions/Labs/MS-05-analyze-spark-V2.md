#  Exercise 6: Analyze data with Apache Spark

### Estimated Duration: 90 Minutes

Apache Spark is an open-source engine for distributed data processing and is widely used to explore, process, and analyze huge volumes of data in data lake storage. Spark is available as a processing option in many data platform products, including Azure HDInsight, Azure Databricks, Azure Synapse Analytics, and Microsoft Fabric. One of the benefits of Spark is support for a wide range of programming languages, including Java, Scala, Python, and SQL; making Spark a very flexible solution for data processing workloads including data cleansing and manipulation, statistical analysis and machine learning, and data analytics and visualization.

## Lab objectives

You will be able to complete the following tasks:

- Task 1: Create a lakehouse and upload files
- Task 2: Create a notebook
- Task 3: Load data into a dataframe
- Task 4: Explore data in a dataframe
- Task 5: Aggregate and group data in a dataframe
- Task 6: Use Spark to transform data files
- Task 7: Work with tables and SQL
- Task 8: Visualize data with Spark
- Task 9: Save the notebook and end the Spark session

### Task 1: Create a lakehouse and upload files

In this task, you will create a lakehouse to organize and analyze your data files. After setting up your workspace, you'll switch to the *Data Engineering* experience in the portal to initiate the creation of the data lakehouse.

1. Select your workspace, then select **+ New item** and **Notebook**. After a few seconds, a new notebook containing a single cell will open. Notebooks are made up of one or more cells that can contain code or markdown (formatted text).

2. On the All items page, scroll down and select Lakehouse from Store data.

     ![Screenshot of uploaded files in a lakehouse.](./Images/E1T1S3.png)
   
3. Enter the below-mentioned details to create a Lakehouse. 

   - **Name:** Enter **fabric_lakehouse<inject key="DeploymentID" enableCopy="false"/>**

   - Click on **Create**

4. Once you're in the lakehouse, navigate to the **Files** folder in the **Explorer** pane, click on the **ellipses** menu, and select **Upload** followed by **Upload folder**. Then, upload the **orders** folder located at **C:\LabFiles\Files\orders** to the lakehouse.

5. After the files have been uploaded, expand **Files** and select the **orders** folder; and verify that the CSV files have been uploaded, as shown here:

    ![Screenshot of uploaded files in a lakehouse.](./Images/uploaded-files.png)

### Task 2: Create a notebook

In this task, you will create a notebook to work with data in Apache Spark. Notebooks provide an interactive environment where you can write and run code in multiple languages, while also allowing you to add notes for documentation.

1. Navigate back to the workspace and click on **Notebook** from All Items to create a new notebook.
   
    > **Note**: After a few seconds, a new notebook containing a single *cell* will open. Notebooks are made up of one or more cells that can contain *code* or *markdown* (formatted text).

2. In the **Explorer** tab, click on **Data items (1)**, then select **Add data items (2)** and choose **Existing data sources (3)**.

    ![.](./Images/notebooka.png)

3. Select **fabric_lakehouse<inject key="DeploymentID" enableCopy="false"/>** from the options and click on **Connect**.
   
    > Note: Reload the page to get the files and folders.

4. Select the first cell (currently a *code* cell), and then click the **M&#8595;** button in the dynamic toolbar at the top-right to convert it to a **markdown** cell.

5. Use the **&#128393;** **(Edit)** button to switch the cell to editing mode, then modify the markdown as follows:

    ```
   # Sales order data exploration

   Use the code in this notebook to explore sales order data.
    ```

6. Click anywhere in the notebook outside of the cell to exit editing mode and view the rendered markdown.

### Task 3: Load data into a dataframe

In this task, you will load data into a dataframe to prepare it for analysis. Dataframes in Spark, similar to Pandas dataframes in Python, offer a structured way to manage and manipulate data organized in rows and columns.

> **Note**: Spark supports multiple coding languages, including Scala, Java, and others. In this exercise, we'll use *PySpark*, which is a Spark-optimized variant of Python. PySpark is one of the most commonly used languages on Spark and is the default language in Fabric notebooks.

1. With the notebook open, expand the **Files** list and select the **orders** folder to display the CSV files alongside the notebook editor, as shown here:

    ![Screenshot of a notebook with a Files pane.](./Images/notebook-files.png)

2. In the **orders (1)** folder, click the **ellipses(2)** menu for **2019.csv**, then select **Load data (3)** **->** **Spark (4)**.

   ![](./Images/Pg7-LoadData-S2.png)

3. A new code cell containing the following code should be added to the notebook:

    ```python
   df = spark.read.format("csv").option("header","true").load("Files/orders/2019.csv")
   # df now is a Spark DataFrame containing CSV data from "Files/orders/2019.csv".
   display(df)
    ```

    > **Tip**: You can hide the Lakehouse Explorer panes on the left by using their **<<** icons. Doing so will help you focus on the notebook.

4. Use the **&#9655; Run cell** button on the left of the cell to run it.

    > **Note**: Since this is the first time you've run any Spark code, a Spark session must be started. This means that the first run in the session can take a minute or so to complete. Subsequent runs will be quicker.

5. When the cell command has been completed, review the output below the cell, which should look similar to this:

    | Index | SO43701 | 11 | 2019-07-01 | Christy Zhu | christy12@adventure-works.com | Mountain-100 Silver, 44 | 16 | 3399.99 | 271.9992 |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | 1 | SO43704 | 1 | 2019-07-01 | Julio Ruiz | julio1@adventure-works.com | Mountain-100 Black, 48 | 1 | 3374.99 | 269.9992 |
    | 2 | SO43705 | 1 | 2019-07-01 | Curtis Lu | curtis9@adventure-works.com | Mountain-100 Silver, 38 | 1 | 3399.99 | 271.9992 |
    | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

6. The output shows the rows and columns of data from the 2019.csv file. However, note that the column headers don't look right. The default code used to load the data into a dataframe assumes that the CSV file includes the column names in the first row, but in this case, the CSV file just includes the data with no header information.

7. Modify the code to set the **header** option to **false** as follows:

    ```python
   df = spark.read.format("csv").option("header","false").load("Files/orders/2019.csv")
   # df now is a Spark DataFrame containing CSV data from "Files/orders/2019.csv".
   display(df)
    ```

8. Re-run the cell and review the output, which should look similar to this:

   | Index | _c0 | _c1 | _c2 | _c3 | _c4 | _c5 | _c6 | _c7 | _c8 |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | 1 | SO43701 | 11 | 2019-07-01 | Christy Zhu | christy12@adventure-works.com | Mountain-100 Silver, 44 | 16 | 3399.99 | 271.9992 |
    | 2 | SO43704 | 1 | 2019-07-01 | Julio Ruiz | julio1@adventure-works.com | Mountain-100 Black, 48 | 1 | 3374.99 | 269.9992 |
    | 3 | SO43705 | 1 | 2019-07-01 | Curtis Lu | curtis9@adventure-works.com | Mountain-100 Silver, 38 | 1 | 3399.99 | 271.9992 |
    | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

9. Now the dataframe correctly includes the first row as data values, but the column names are auto-generated and not very helpful. To make sense of the data, you need to explicitly define the correct schema and data type for the data values in the file.

10. Modify the code as follows to define a schema and apply it when loading the data:

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

11. Run the modified cell and review the output, which should look similar to this:

    | Index | SalesOrderNumber | SalesOrderLineNumber | OrderDate | CustomerName | Email | Item | Quantity | UnitPrice | Tax |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | 1 | SO43701 | 11 | 2019-07-01 | Christy Zhu | christy12@adventure-works.com | Mountain-100 Silver, 44 | 16 | 3399.99 | 271.9992 |
    | 2 | SO43704 | 1 | 2019-07-01 | Julio Ruiz | julio1@adventure-works.com | Mountain-100 Black, 48 | 1 | 3374.99 | 269.9992 |
    | 3 | SO43705 | 1 | 2019-07-01 | Curtis Lu | curtis9@adventure-works.com | Mountain-100 Silver, 38 | 1 | 3399.99 | 271.9992 |
    | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

12. Now the dataframe includes the correct column names (in addition to the **Index**, which is a built-in column in all dataframes based on the ordinal position of each row). The data types of the columns are specified using a standard set of types defined in the Spark SQL library, which were imported at the beginning of the cell.

13. Confirm that your changes have been applied to the data by viewing the dataframe. Run the following cell:

    ```python
    display(df)
    ```

14. The dataframe includes only the data from the **2019.csv** file. Modify the code so that the file path uses a \* wildcard to read the sales order data from all of the files in the **orders** folder:

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

15. Run the modified code cell and review the output, which should now include sales for 2019, 2020, and 2021.

    >**Note**: Only a subset of the rows is displayed, so you may not be able to see examples from all years.

### Task 4: Explore data in a dataframe

In this task, you will explore data within a dataframe to gain insights and understand its structure. The dataframe object offers various functions for filtering, grouping, and manipulating the data it contains, facilitating effective analysis.

### Filter a dataframe

1. Use the **+ Code** icon below the cell output to add a new code cell to the notebook, and enter the following code in it.

    ```Python
   customers = df['CustomerName', 'Email']
   print(customers.count())
   print(customers.distinct().count())
   display(customers.distinct())
    ```

   >**Note:** You might have to hover your mouse below the output to view the + Code option.

2. Run the new code cell and review the results. Observe the following details:
    
    - When you operate on a dataframe, the result is a new dataframe (in this case, a new **customers** dataframe is created by selecting a specific subset of columns from the **df** dataframe)
    
    - Dataframes provide functions such as **count** and **distinct** that can be used to summarize and filter the data they contain.
    
    - The `dataframe['Field1', 'Field2', ...]` syntax is a shorthand way of defining a subset of columns. You can also use the **select** method, so the first line of the code above could be written as `customers = df.select("CustomerName", "Email")`

3. Modify the code as follows:

    ```Python
   customers = df.select("CustomerName", "Email").where(df['Item']=='Road-250 Red, 52')
   print(customers.count())
   print(customers.distinct().count())
   display(customers.distinct())
    ```

4. Run the modified code to view the customers who have purchased the *Road-250 Red, 52* product. Note that you can "chain" multiple functions together so that the output of one function becomes the input for the next - in this case, the dataframe created by the **select** method is the source dataframe for the **where** method that is used to apply filtering criteria.

### Task 5: Aggregate and group data in a dataframe

In this task, you will aggregate and group data within a dataframe to summarize information and extract meaningful insights. This process involves applying functions to organize the data based on specific criteria, allowing for easier analysis and reporting.

1. Add a new code cell to the notebook, and enter the following code in it:

    ```Python
   productSales = df.select("Item", "Quantity").groupBy("Item").sum()
   display(productSales)
    ```

2. Run the code cell you added, and note that the results show the sum of order quantities grouped by product. The **groupBy** method groups the rows by *Item*, and the subsequent **sum** aggregate function is applied to all of the remaining numeric columns (in this case, *Quantity*)

3. Add a new code cell to the notebook, and enter the following code in it:

    ```Python
   from pyspark.sql.functions import *

   yearlySales = df.select(year(col("OrderDate")).alias("Year")).groupBy("Year").count().orderBy("Year")
   display(yearlySales)
    ```

4. Run the code cell you added, and note that the results show the number of sales orders per year. Note that the **select** method includes an SQL **year** function to extract the year component of the *OrderDate* field (which is why the code includes an **import** statement to import functions from the Spark SQL library). It then uses an **alias** method to assign a column name to the extracted year value. The data is then grouped by the derived *Year* column, and the count of rows in each group is calculated before finally the **orderBy** method is used to sort the resulting dataframe.

### Task 6: Use Spark to transform data files

In this task, you will use Spark to transform data files into a desired format for analysis and processing. This involves ingesting data in specific structures and applying transformations, a common responsibility for data engineers, to prepare the data for downstream applications.

### Use dataframe methods and functions to transform data

1. Add a new code cell to the notebook, and enter the following code in it:

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

1. Review the output and verify that the transformations have been made to the data.

1. You can use the full power of the Spark SQL library to transform the data by filtering rows, deriving, removing, renaming columns, and applying any other required data modifications.

    > **Tip**: See the [Spark dataframe documentation](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html) to learn more about the methods of the Dataframe object.

### Save the transformed data

1. Add a new cell with the following code to save the transformed dataframe in Parquet format (Overwriting the data if it already exists):

    ```python
   transformed_df.write.mode("overwrite").parquet('Files/transformed_data/orders')
   print ("Transformed data saved!")
    ```

   >**Note**: If the node fails, rerun it.

2. Run the cell and wait for the message that the data has been saved. Then, in the **Explorer** pane on the left, in the **...** menu for the **Files** node, select **Refresh**; and select the **transformed_data** folder to verify that it contains a new folder named **orders**, which in turn contains one or more Parquet files.

    ![Screenshot of a folder containing parquet files.](./Images/saved-parquet.png)

3. Add a new cell with the following code to load a new dataframe from the parquet files in the **transformed_orders/orders** folder:

    ```python
   orders_df = spark.read.format("parquet").load("Files/transformed_data/orders")
   display(orders_df)
    ```

4. Run the cell and verify that the results show the order data that has been loaded from the parquet files.

### Save data in partitioned files

1. Add a new cell with the following code; which saves the dataframe, partitioning the data by **Year** and **Month**:

    ```python
   orders_df.write.partitionBy("Year","Month").mode("overwrite").parquet("Files/partitioned_data")
   print ("Transformed data saved!")
    ```

1. Run the cell and wait for the message that the data has been saved. Then, in the **Explorer** pane on the left, in the **...** menu for the **Files** node, select **Refresh**; and expand the **partitioned_data** folder to verify that it contains a hierarchy of folders named **Year=*xxxx***, each containing folders named **Month=*xxxx***. Each month's folder contains a parquet file with the orders for that month.

    ![Screenshot of a hierarchy of partitioned data files.](./Images/partitioned-files.png)

1. Partitioning data files is a common way to optimize performance when dealing with large volumes of data. This technique can significantly improve performance and make it easier to filter data.

1. Add a new cell with the following code to load a new dataframe from the **orders.parquet** file:

    ```python
   orders_2021_df = spark.read.format("parquet").load("Files/partitioned_data/Year=2021/Month=*")
   display(orders_2021_df)
    ```

1. Run the cell and verify that the results show the order data for sales in 2021. Note that the partitioning columns specified in the path (**Year** and **Month**) are not included in the dataframe.

### Task 7: Work with tables and SQL

As you've seen, the native methods of the dataframe object enable you to query and analyze data from a file quite effectively. However, many data analysts are more comfortable working with tables that they can query using SQL syntax. Spark provides a *metastore* in which you can define relational tables. The Spark SQL library that provides the dataframe object also supports the use of SQL statements to query tables in the metastore. By using these capabilities of Spark, you can combine the flexibility of a data lake with the structured data schema and SQL-based queries of a relational data warehouse - hence the term "data lakehouse".

### Create a table

Tables in a Spark metastore are relational abstractions over files in the data lake. tables can be *managed* (in which case the files are managed by the metastore) or *external* (in which case the table references a file location in the data lake that you manage independently of the metastore).

1. Add a new code cell to the notebook, and enter the following code, which saves the dataframe of sales order data as a table named **salesorders**:

    ```Python
   # Create a new table
   df.write.format("delta").saveAsTable("salesorders")

   # Get the table description
   spark.sql("DESCRIBE EXTENDED salesorders").show(truncate=False)
    ```

    > **Note:** It's worth noting a couple of things about this example. Firstly, no explicit path is provided, so the files for the table will be managed by the metastore. Secondly, the table is saved in **delta** format. You can create tables based on multiple file formats (including CSV, Parquet, Avro, and others) but *delta lake* is a Spark technology that adds relational database capabilities to tables; including support for transactions, row versioning, and other useful features. Creating tables in delta format is preferred for data lakehouses in Fabric.

2. Run the code cell and review the output, which describes the definition of the new table.

3. In the **Explorer** pane, in the **...** menu for the **Tables** folder, select **Refresh**. Then expand the **Tables** node and verify that the **salesorders** table has been created.

    ![Screenshot of the salesorder table in Explorer.](./Images/table-view.png)

4. In the **...** menu for the **salesorders** **(1)** table, select **Load data** **(2)** > **Spark** **(2)**. 

   ![01](./Images/02/pg04-loadspark.png)

5. A new code cell containing code similar to the following example is added to the notebook:

    ```Python
   df = spark.sql("SELECT * FROM [your_lakehouse].salesorders LIMIT 1000")
   display(df)
    ```
     > **Note**: Ensure to replace the [your_lakehouse] with **fabric_lakehouse<inject key="DeploymentID" enableCopy="false"/>**
     
6. Run the new code, which uses the Spark SQL library to embed a SQL query against the **salesorder** table in PySpark code and load the results of the query into a dataframe.

### Run SQL code in a cell

While it's useful to be able to embed SQL statements into a cell containing PySpark code, data analysts often just want to work directly in SQL.

1. Add a new code cell to the notebook, and enter the following code in it:

    ```SQL
   %%sql
   SELECT YEAR(OrderDate) AS OrderYear,
          SUM((UnitPrice * Quantity) + Tax) AS GrossRevenue
   FROM salesorders
   GROUP BY YEAR(OrderDate)
   ORDER BY OrderYear;
    ```

2. Run the cell and review the results. Observe that:
    
    - The `%%sql` line at the beginning of the cell (called a *magic*) indicates that the Spark SQL language runtime should be used to run the code in this cell instead of PySpark.
    
    - The SQL code references the **salesorders** table that you created previously.
    
    - The output from the SQL query is automatically displayed as the result under the cell.

        > **Note**: For more information about Spark SQL and dataframes, see the [Spark SQL documentation](https://spark.apache.org/docs/2.2.0/sql-programming-guide.html).

### Task 8: Visualize data with Spark

In this task, you will visualize data using Spark to enhance understanding and insights through graphical representation. While Fabric notebooks offer a basic chart view for data from dataframes or Spark SQL queries, you can utilize Python graphics libraries like **matplotlib** and **seaborn** for more comprehensive and customized charting.

### View results as a chart

1. Add a new code cell to the notebook, and enter the following code in it:

    ```SQL
   %%sql
   SELECT * FROM salesorders
    ```

2. Run the code and observe that it returns the data from the **salesorders** view you created previously.

3. We need to change the view from table to chart in the results section beneath the cell. To do so click on **+ New chart** next to your result table tab.

   ![01](./Images/02/pg07-addchart.png)

4. Click on **Start editing** under **Suggested charts** to customize chart visulaization. Then set the options as follows under **Basic** of **chat settings**.
    - **Title**: Sales Order Visualization
    - **Subtitle**: Overview of Item-wise Sales Order Quantities
    - **Chart type**: Bar chart
    - **X-axis**: Item
    - **Y-axis**: Quantity
    - **Show Legend**: *Unslected*
    - **Series Group**: *leave blank*
    - **Aggregation**: Sum
    - **Stacked**: *Unselected*

       ![01](./Images/02/pg07-basic01.png)

       ![01](./Images/02/pg07-basic02.png)

5. Verify that the chart looks similar to this:

   ![01](./Images/02/pg07-finalchart.png)

### Get started with **matplotlib**

1. Add a new code cell to the notebook, and enter the following code in it:

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

1. To visualize the data as a chart, we'll start by using the **matplotlib** Python library. This library is the core plotting library on which many others are based, and provides a great deal of flexibility in creating charts.

1. Add a new code cell to the notebook, and add the following code to it:

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

   ![01](./Images/02/pg07-Matplotresult01.png)

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

   ![01](./Images/02/pg07-Matplotresult02.png)

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

   ![01](./Images/02/pg07-Matplotresult03.png)

    > **Note:** To learn more about plotting with matplotlib, see the [matplotlib documentation](https://matplotlib.org/).

### Use the **seaborn** library

While **matplotlib** enables you to create complex charts of multiple types, it can require some complex code to achieve the best results. For this reason, over the years, many new libraries have been built on the base of matplotlib to abstract its complexity and enhance its capabilities. One such library is **seaborn**.

1. Add a new code cell to the notebook, and enter the following code in it:

    ```Python
   import seaborn as sns

   # Clear the plot area
   plt.clf()

   # Create a bar chart
   ax = sns.barplot(x="OrderYear", y="GrossRevenue", data=df_sales)
   plt.show()
    ```

2. Run the code and observe that it displays a bar chart using the seaborn library.

3. Modify the code as follows:

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

4. Run the modified code and note that seaborn enables you to set a consistent color theme for your plots.

5. Modify the code again as follows:

    ```Python
   import seaborn as sns

   # Clear the plot area
   plt.clf()

   # Create a bar chart
   ax = sns.lineplot(x="OrderYear", y="GrossRevenue", data=df_sales)
   plt.show()
    ```

6. Run the modified code to view the yearly revenue as a line chart.

   ![01](./Images/02/pg07-Seaborn01.png)

    > **Note**: To learn more about plotting with seaborn, see the [seaborn documentation](https://seaborn.pydata.org/index.html).

### Task 9: Save the notebook and end the Spark session

In this task, you will save your notebook with a meaningful name to preserve your work after processing the data. Additionally, you will end the Spark session to free up resources and complete your data engineering tasks.

1. In the top left corner, set the **Name** of the notebook from Notebook 1 to **Explore Sales Orders Notebook**.

2. On the notebook menu, select **Stop session** to end the Spark session.

    > **Note:** The stop session icon is present next to the **Start Session** option.

### Summary

In this exercise, you've learned how to use Spark to work with data in Microsoft Fabric.

### You have successfully completed the exercise. Click on Next >> to proceed with the next exercise.
