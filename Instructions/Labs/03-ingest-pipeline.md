# Exercise 3: Ingest data with a pipeline

### Estimated Duration: 90 minutes

## Overview

Data pipelines orchestrate data movement by extracting, transforming, and loading (ETL) or extracting, loading, and transforming (ELT) data into analytical stores like lakehouses or data warehouses. In Microsoft Fabric, the graphical pipeline canvas simplifies pipeline creation with minimal coding, enabling automation of data ingestion from operational sources. Fabric also integrates Apache Spark, allowing scalable data processing and custom transformations within OneLake storage. By combining pipelines and Spark, data engineers can efficiently implement complex ingestion workflows, ensuring transformed data is optimized for analysis.

## Lab Objectives

In this lab, you will complete the following tasks:

 - Task 1 : Create a pipeline
 - Task 2 : Create a notebook
 - Task 3 : Modify the pipeline

## Task 1 : Create a pipeline

A simple way to ingest data is to use a **Copy Data** activity in a pipeline to extract the data from a source and copy it to a file in the lakehouse.

1. Navigate back to your workspace **fabri-<inject key="DeploymentID" enableCopy="false"/>** **(1)** from the left navigation pane. Click on **+ New Item (2)** and select **Data Pipeline (3)**.

    ![](./Images/E1T3S1.png)

2. Create a new data pipeline named **Ingest Sales Data Pipeline (1)** and click on **Create (2)**. 
   
   ![03](./Images/01/Pg3-TCreatePipeline-S1.1.png)
   
3. If the **Copy data** wizard doesn't open automatically, select **Copy data assistant (1)** in the pipeline editor page.

   ![03](./Images/E2-T3-S3.png)

4. In the **Copy Data** wizard, on the **Choose a data source** page, search for **Http (1)** and select it.

   ![Screenshot of the Choose data source page.](./Images/E1T1S4.png)

5. In the **Connection settings** pane, enter the following settings for the connection to your data source:
    
    - URL: **`https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/sales.csv`**  **(1)**
    - Connection: **Create new connection (2)**
    - Connection name: **Connection<inject key="DeploymentID" enableCopy="false"/> (3)**
    - Authentication kind : **Anonymous (4)**
    - Click on **Next (5)**
  
   ![Account-manager-start](./Images/lab1-image11.png)
    
6. Make sure the following settings are selected and click on **Next** :
    
    - Relative URL: **Leave blank**
    - Request method: **GET**
    - Additional headers: **Leave blank**
    - Binary copy: **Unselected**
    - Request timeout: **Leave blank**
    - Max concurrent connections: **Leave blank**
  
   ![05](./Images/fabric4.png)
   
7. Wait for the data to be sampled and then ensure that the following settings are selected:
    
    - File format: **DelimitedText (1)**
    - Column delimiter: **Comma (,) (2)**
    - Row delimiter: **Line feed (\n) (3)**
    - Select **Preview data (4)** to see a sample of the data that will be ingested.

   ![Account-manager-start](./Images/lab1-image12.png)

8. Observe the sample of the data that will be ingested. Then close the data preview and click on **Next**.

   ![Account-manager-start](./Images/lab1-image13.png)

9. On the **Choose data destination** page, click on **OneLake (1)** and select the lakehouse **fabric_lakehouse_<inject key="DeploymentID" enableCopy="false"/> (2)**.

    ![](./Images/ap20.png)

10. Set the following data destination options, and then select **Next (4)**:

    - Root folder: **Files (1)**
    - Folder path: **new_data (2)**
    - File name: **sales.csv  (3)**
   
    ![08](./Images/fabric9.png)

11. Set the following file format options and then select **Next (4)**:

    - File format: **DelimitedText (1)**
    - Column delimiter: **Comma (,) (2)**
    - Row delimiter: **Line feed (\n) (3)**
   
    ![09](./Images/fabric10.png)

12. On the **Copy summary** page, review the details of your copy operation and then select **Save + Run**.

    ![09](./Images/b777.png)

13. A new pipeline containing a **Copy data** activity is created, as shown here:

    ![Screenshot of a pipeline with a Copy Data activity.](./Images/E1T3S13.png)

14. When the pipeline starts to run, you can monitor its status in the **Output** pane under the pipeline designer. Use the **&#8635;** (*Refresh*) icon to refresh the status, and wait until it has succeeded.

    ![Screenshot of a pipeline with a Copy Data activity.](./Images/01/Pg3-CpyOutput.png)

15. Navigate to **fabric_lakehouse_<inject key="DeploymentID" enableCopy="false"/> (1)** from the left pane, expand **Files (1)** and select the **new_data (2)** folder, refresh the page and verify that the **sales.csv (3)** file has been copied.

    ![Account-manager-start](./Images/lab1-image16.png)

## Task 2 : Create a notebook

1. On the **Home** page for your lakehouse, in the **Open notebook** menu, select **New notebook**.

   ![](./Images/imag6.png)

1. Select the existing cell in the notebook, which contains some simple code, and then replace the default code with the following variable declaration and click on **&#9655; Run**.

    ```python
   table_name = "sales"
    ```

   ![11](./Images/01/Pg3-Notebook-S2.png) 

1. In the **Ellipsis(...) (1)** menu for the cell (at its top-right) select **Toggle parameter cell (2)**. This configures the cell so that the variables declared in it are treated as parameters when running the notebook from a pipeline.

     ![Account-manager-start](./Images/lab1-image17.png)

1. Under the parameters cell, use the **+ Code** button to add a new code cell. Then add the following code to it:

     ![](./Images/E2-T4-S9.png) 

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

    This code loads the data from the sales.csv file that was ingested by the **Copy Data** activity, applies some transformation logic, and saves the transformed data as a **managed table** - appending the data if the table already exists.

1. Verify that your notebooks look similar to this, and then use the **&#9655; Run all** button on the toolbar to run all of the cells it contains.

    ![Screenshot of a notebook with a parameters cell and code to transform data.](./Images/fab8.png)

    > **Note**: Since this is the first time you've run any Spark code in this session, the Spark pool must be started. This means that the first cell can take a minute or so to complete.

1. When the notebook run has completed, in the **Lakehouse explorer** pane on the left, in the **Ellipsis(...)** menu for **Tables** select **Refresh** and verify that a **sales** table has been created.

    ![.](./Images/fab-6.png)

1. Navigate to notebook menu bar, use the ⚙️ **Settings (1)** icon to view the notebook settings. Then set the **Name** of the notebook to **Load Sales Notebook (2)** and close the settings pane.

     ![.](./Images/fab-7.png)

## Task 3 : Modify the pipeline

Now that you've implemented a notebook to transform data and load it into a table, you can incorporate the notebook into a pipeline to create a reusable ETL process.

1. In the hub menu bar on the left select the **Ingest Sales Data** pipeline you created previously.

2. On the **Activities** tab, in the **More activities** list, select **Delete data**. Then position the new **Delete data**  activity to the left of the **Copy data** activity and connect its **On completion (the blue arrow)** output to the **Copy data** activity, as shown below:

    ![](./Images/imag13.png)

    ![](./Images/ap22.png)

3. Select the **Delete data** activity, and in the pane below the design canvas, set the following properties:
    - **General**:
        - **Name**: Delete old files

          ![](./Images/imag14.png)

    - **Source**
        - **Connection**: **fabric_lakehouse_<inject key="DeploymentID" enableCopy="false"/> (1)**
        - **File path type**: **Wildcard file path (2)**
        - **Folder path**: **new_data** **(3)**
        - **Wildcard file name**: **.csv (4)**    
        - **Recursively**: **Selected (5)**

          ![](./Images/ap23.png)

    - **Logging settings**:
        - **Enable logging**: *<u>Un</u>selected*

          ![](./Images/imag16.png)

    These settings will ensure that any existing .csv files are deleted before copying the **sales.csv** file.

4. In the pipeline designer, on the **Activities (1)** tab, select **Notebook (2)** to add a **Notebook** activity to the pipeline.

    ![](./Images/ap24.png)

5. Select the **Copy data** activity and then connect its **On Completion** output to the **Notebook** activity as shown here:

    ![](./Images/ap25.png)

6. Select the **Notebook** activity, and then in the pane below the design canvas, set the following properties:
    - **General**:
        - **Name**: Load Sales notebook
    - **Settings**:
        - **Notebook**: **Load Sales notebook  (1)**
        - **Base parameters**: *Click on **Add (2)** Add a new parameter with the following **properties (3)**:*
            
            | Name | Type | Value |
            | -- | -- | -- |
            | table_name | String | new_sales |

             ![](./Images/ap26.png)

    The **table_name** parameter will be passed to the notebook and override the default value assigned to the **table_name** variable in the parameters cell.

7. On the **Home** tab, use the **&#128427;** (*Save*) icon to save the pipeline. Then use the **&#9655; Run** button to run the pipeline, and wait for all of the activities to complete.

    ![Screenshot of a pipeline with a Dataflow activity.](./Images/pipeline-run1.png)

8. In the hub menu bar on the left edge of the portal, select your lakehouse.

9. In the **Explorer** pane, expand **Tables** and select the **new_sales** table to see a preview of the data it contains. This table was created by the notebook when it was run by the pipeline.

   ![Screenshot of a dataoutput.](./Images/new-sales-endoutput.png)

In this exercise, you implemented a data ingestion solution that uses a pipeline to copy data to your lakehouse from an external source, and then uses a Spark notebook to transform the data and load it into a table.

## Summary

In this lab, you created a pipeline to automate data processing, developed a notebook for writing and testing your pipeline logic, and then modified the pipeline to refine its functionality. These tasks helped you gain practical experience in building and optimizing data workflows within a pipeline.

## You have successfully completed the lab
