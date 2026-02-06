# Exercise 3: Ingest data with a pipeline

### Estimated Duration: 90 Minutes

## Overview

In this exercise, you will learn how to ingest data into a Microsoft Fabric lakehouse using pipelines. You will implement ETL/ELT processes by building a pipeline that copies data from external sources into OneLake storage and leverages Apache Spark to transform and load it into structured tables for analysis—an essential skill for scalable cloud-based analytics solutions.

## Lab Objectives

In this exercise, you will complete the following tasks:

 - Task 1: Create a pipeline
 - Task 2: Create a notebook
 - Task 3: Modify the pipeline

## Task 1: Create a pipeline

In this task, you will create a pipeline in Microsoft Fabric to ingest data into your lakehouse. You will use the Copy Data activity to extract data from a source and copy it into a subfolder within the lakehouse, forming the foundation for an ETL or ELT process.

1. From the left pane, click on **My workspace (1)**, then select the **fabric-<inject key="DeploymentID" enableCopy="false"/> (2)** workspace.  

     ![](./Images2/lab4-11-1.png)

2. In the **fabric-<inject key="DeploymentID" enableCopy="false"/>** workspace, click on **+ New item (1)**, then in the right pane, search for **Pipeline (2)** in the search bar, and select **Pipeline (3)** under the **Get data** section.

     ![](./Images2/lab4-11-2n.png)

3. Create a new data pipeline by entering the name **Ingest Sales Data Pipeline (1)**, then click **Create (2)** to proceed.
   
    ![](./Images2/3/t1-2.png)
   
4. If the **Copy data** assistant doesn't open automatically, select **Copy data assistant** in the pipeline editor page.

    ![](./Images2/3/E3T1S4.png)

     >**Note:** If the **Use Copy job?** pop-up appears, select **No, use copy assistant** to continue with the Copy Data Assistant.

6. In the **Copy Data** wizard, search for **Http (1)** in the search bar and select the **Http (2)** connector from the results.

    ![](./Images2/3/t1-4.png)

7. In the **Connection settings** pane, enter the following for your data source connection:
    
    - URL: **`https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/sales.csv`**  **(1)**
    - Connection: **Create new connection (2)**
    - Connection name: **Connection<inject key="DeploymentID" enableCopy="false"/> (3)**
    - Authentication kind : **Anonymous (4)**
    - Privacy Level : **None (5)**
    - Then, click **Next (6)**.

      ![](./Images2/3/t1-5p.png)
    
8. Leave all fields on the **Choose data** page as default and click **Next**.
  
    ![](./Images2/3/E3T1S6.png)
   
9. After the data is sampled, ensure the following settings are selected and leave all other fileds as default:
    
    - File format: **DelimitedText (1)**
    - Column delimiter: **Comma (,) (2)**
    - Row delimiter: **Line feed (\n) (3)**
    - Click **Preview data (4)** to see a sample of the data.

      ![](./Images2/3/E3T1S8.png)

10. After reviewing the sample data, click **Next** to proceed to the next step.

    ![](./Images2/3/t1-8.png)

11. On the **Choose data destination** page, click on **OneLake catalog (1)** from the top menu bar, then select the lakehouse named **fabric_lakehouse_<inject key="DeploymentID" enableCopy="false"/> (2)**.

    ![](./Images2/3/t1-9.png)
     
1. On **Choose copy job mode** page, select **Full copy (1)** and click on **Next (2)**.

    ![](./Images2/3/E3T1S11.png)

12. On the **Map to destination** page, configure the following settings:

    - Select: **Files (1)**
    - Folder path: **new_data (2)**
    - File name: **sales.csv  (3)**
   
      ![](./Images2/3/E3T1S12.png)

13. Scroll down and expand **File format settings** and select following settings and then click on **Next (3)**:

    - Column delimiter: **Comma (,) (1)**
    - Row delimiter: **Line feed (\n) (2)**

      ![](./Images2/3/E3T1S14.png)

14. On the **Review + save** page, review the copy summary to verify all source and destination settings, and then click on **Save** to initiate the data copy process.

    ![](./Images2/3/E3T1S13.png)

15. After executing the copy operation, a new pipeline containing the **Copy job** activity is automatically created, as shown in the diagram.

    ![](./Images2/3/E3T1S15.png)

1. Click on the **Copy job**, go to **Setting** and in the **connection** drop down, select **Browse all**

    ![](./Images2/3/E3T1S16.png)

1. On the **Choose a data source to get started** page, select **Copy job**

    ![](./Images2/3/E3T1S17.png)

1. On the **Connect data source** page, select **Create new connection (1)** and in the **Authentication kind**, select **Organizational account (2)** then click on **Sign in (3)**

    ![](./Images2/3/E3T1S20.png)

1. In the pop-up select the provided **Email: <inject key="AzureAdUserEmail"></inject>** then click on **Connect**

    ![](./Images2/3/E3T1S18.png)

1. Click on the **Home (1)** tab, save the pipeline using the **Save (2)** icon, then execute it by clicking **Run (3)** and wait for all activities to complete.

    ![](./Images2/3/E3T1S19.png)

16. Once the pipeline is running, monitor its execution status by selecting the **Output (1)** tab below the pipeline designer. Click the refresh **↻ (2)** icon to update the status, and wait for the pipeline to show as Succeeded.

    ![](./Images2/3/E3T1S21.png)

17. From the left pane, click on **My workspace (1)**, then select **fabric_lakehouse_<inject key="DeploymentID" enableCopy="false"/> (2)** to open.

    ![](./Images2/lab4-11-4.png)

18. In the **Explorer** pane, click the **ellipsis (...) (1)** next to the **Files** folder, and  select **Refresh (2)** to verify that the folder **new_data (3)** contains the copied file **sales.csv (4)**.

     ![](./Images2/lab4-11-5.png)

     ![](./Images2/lab4-11-5.1.png)

## Task 2: Create a notebook

In this task, you will create a notebook in Microsoft Fabric to begin processing your ingested data using PySpark. You will write code to load sales data, apply transformations, and save the results as a table in the lakehouse—enabling further analysis or reporting through SQL or visualization tools.

1. From the lakehouse Home page, open the **Open notebook (1)** menu and select **New notebook (2)** to create a new notebook.

    ![](./Images2/3/t2-1.png)

1. Select the existing cell in the notebook, replace the default code with the following **code (1)** and click on **&#9655; Run (2)**.

    ```python
   table_name = "sales"
    ```

    ![](./Images2/3/t2-2.png)

1. In the notebook cell, click the **ellipsis (...) (1)** menu from the top-right corner of the cell, then select **Toggle parameter cell (2)** to set the cell’s variables as parameters for pipeline runs.

    ![](./Images2/3/t2-3.png)

1. Under the parameters cell, hover the mouse and use the **+ Code** button to add a new code cell. Then add the following code to it:

    ![](./Images2/3/t2-4.png)

    ```python
    from pyspark.sql.functions import *
    
    # Read the new sales data
    df = spark.read.format("csv").option("header","true").option("inferSchema","true").load("Files/new_data/*.csv")

    ## Add month and year columns
    df = df.withColumn("Year", year(col("OrderDate"))).withColumn("Month", month(col("OrderDate")))

    # Derive FirstName and LastName columns
    df = df.withColumn("FirstName", split(col("CustomerName"), " ").getItem(0)).withColumn("LastName", split(col("CustomerName"), " ").getItem(1))

    # Filter and reorder columns
    df = df["SalesOrderNumber", "SalesOrderLineNumber", "OrderDate", "Year", "Month", "FirstName", "LastName", "EmailAddress", "Item", "Quantity", "UnitPrice", "TaxAmount"]

    # Load the data into a managed table
    #Managed tables are tables for which both the schema metadata and the data files are managed by Fabric. The data files for the table are created in the Tables folder.
    df.write.format("delta").mode("append").saveAsTable(table_name)
    ```

    This code loads data from the ingested **sales.csv** file, applies transformations, and saves it as a **managed table**, appending if the table already exists.

1. Verify your notebook matches the example and click **&#9655; Run all** on the toolbar to execute all cells.

    ![](./Images2/3/t2-5.png)

    > **Note:** Since this is the first time you've run any Spark code in this session, the Spark pool must be started. This means that the first cell can take a minute or so to complete.

1. After the notebook run completes, from the left pane, click on **My workspace (1)**, then select **fabric_lakehouse_<inject key="DeploymentID" enableCopy="false"/> (2)** to open.

     ![](./Images2/lab4-11-4.png)

1. In the **Explorer** pane, expand **Tables**, click the **ellipsis (...) (1)** next to it, and select **Refresh (2)** to verify that the **sales** table has been created successfully.

     ![](./Images2/lab4-11-6.png)

     ![](./Images2/lab4-11-7.png)

1. From the left pane, click on **My workspace (1)**, then select **Notebook 1 (2)**.  

    ![](./Images2/lab4-11-8.png)

1. In the notebook window, click the **Settings (1)** icon from the menu bar, update the **Name (2)** field to **Load Sales Notebook**, and then close the settings pane by clicking the **X (3)**.

     ![](./Images2/lab4-11-9.png)

## Task 3: Modify the pipeline

In this task, you will modify your existing pipeline to include the notebook you created for data transformation. By integrating the notebook into the pipeline, you will build a reusable and automated ETL process that extracts data, runs Spark-based transformations, and loads the results into a lakehouse table.

1. From the left pane, click on **My workspace (1)**, then select the previously created **Ingest Sales Data pipeline (2)** to proceed.

     ![](./Images2/lab4-11-10.png)

2. From the **Activities (1)** tab, click the **ellipsis (...) (1)** in the toolbar, select **Delete data (3)** from the list, then position the **Delete data** activity to the left of the **Copy job** activity and connect the **On completion** (blue arrow) output from **Delete data** to **Copy job**, as shown below:

    ![](./Images2/3/E3T3S2.png)

    ![](./Images2/3/E3T3S2-1.png)

3. Select the **Delete data (1)** activity. In the pane below the design canvas, set the following properties:

    - **General (2)**:
        - **Name**: Delete old files **(3)**

          ![](./Images2/3/t3-3a.png)

    - In the **Source (1)** tab, open the **Connection (2)** dropdown and select **Browse all (3)**.

         ![](./Images2/lab4-11-11.png)
    
        - From the **Choose a data source to get started** window, select **fabric_lakehouse_<inject key="DeploymentID" enableCopy="false"/>**.

          ![](./Images2/lab4-11-12.png)

        - Once connected, configure the following:
            - **File path type**: Wildcard file path **(1)**
            - **Folder path**: new_data **(2)**
            - **Wildcard file name**: *.csv **(3)**    
            - **Recursively**: Selected **(4)**

                ![](./Images2/lab4-11-13.png)

    - **Logging settings (1)**:
        - **Enable logging**: *Unselected* **(2)**

          ![](./Images2/3/t3-3c.png)

    These settings will ensure that any existing .csv files are deleted before copying the **sales.csv** file.

4. In the pipeline designer, navigate to the **Activities (1)** tab and select the **Notebook (2)** to add it to the pipeline.

    ![](./Images2/3/t3-4.png)

5. Select the **Copy job** activity, then link its **On completion** (blue arrow) output to the **Notebook** activity as illustrated below:

    ![](./Images2/3/E3T3S5.png)

6. Select the **Notebook (1)** activity. In the pane below the design canvas, set the following properties:

    - **General (2)**:

        - **Name (3)**: Load Sales notebook

             ![](./Images2/3/t3-6a.png) 

    - **Settings (1)**:
        - **Notebook**: Load Sales Notebook  **(2)**
        - **Base parameters (3)**: Click on **New (4)** to add a new parameter with the following properties:
            
            | Name | Type | Value |
            | -- | -- | -- |
            | table_name **(5)** | String **(6)** | new_sales **(7)** |

             ![](./Images2/3/t3-6b.png) 

    The **table_name** parameter will be passed to the notebook and override the default value assigned to the **table_name** variable in the parameters cell.

7. Click on the **Home (1)** tab, save the pipeline using the **Save (2)** icon, then execute it by clicking **Run (3)** and wait for all activities to complete.

    ![](./Images2/3/t3-7.png)

    ![](./Images2/3/E3T3S7.png)

     >**Note :** It may take a few minutes for all activities to complete successfully

8. From the left pane, click on **My workspace (1)**, then select **fabric_lakehouse_<inject key="DeploymentID" enableCopy="false"/> (2)** to open.

    ![](./Images2/lab4-11-15.png)

9. In the **Explorer** pane, click the **ellipsis (...) (1)** next to the **Tables** folder, and  select **Refresh (2)**.

    ![](./Images2/lab4-11-16.png)

10. In the Explorer pane, expand **Tables (1)** and select the **new_sales (2)** table to view a **preview (3)** of its data, which was created by the notebook during the pipeline execution.

    ![](./Images2/lab4-11-17.png)

## Summary

In this exercise, you have completed the following:

- Created a pipeline to automate data processing.
- Developed a notebook to write and test pipeline logic.
- Modified the pipeline to refine and optimize its functionality.

## You have successfully completed the Hands-on lab.

By completing the **How to use Apache Spark in Microsoft Fabric** hands-on lab, you have developed a comprehensive understanding of data engineering workflows within the Fabric environment. You created and managed lakehouses, performed data exploration and transformation using Spark notebooks, ingested data through Dataflows (Gen2), and orchestrated processes using pipelines. This end-to-end experience has equipped you with the practical skills required to build, automate, and optimize scalable data solutions in Microsoft Fabric.
