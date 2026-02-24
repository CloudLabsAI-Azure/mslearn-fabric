# Lab 04: Get started with Real-Time Intelligence in Microsoft Fabric

### Estimated Duration: 30 Minutes

## Overview

In this lab, you'll explore Real-Time Intelligence in Microsoft Fabric using Kusto Query Language (KQL). You'll begin by creating a KQL database and importing sales data into a table. Then, you'll run KQL queries to analyze the data and create a query set. Using this query set, you’ll build a Power BI report to visualize results. Finally, you'll simulate real-time data ingestion using Spark Structured Streaming and Delta tables to process and query IoT-like data dynamically.

## Lab objectives

You will be able to complete the following tasks:

- Task 1: Create a KQL database
- Task 2: Use KQL to query the sales table
- Task 3: Create a Power BI report from a KQL Queryset

## Task 1: Create a KQL database

In this task, you will create a KQL database to facilitate querying of static or streaming data. You will define a table within the KQL database and ingest sales data from a file to enable effective analysis using Kusto Query Language (KQL).

1. In the left pane, navigate to your **Workspace (1)** and click on **fabric-<inject key="DeploymentID" enableCopy="false"/> (2)**, then click on **+ New item (3)** to create a new **Eventhouse**.

    ![](./Images/fabricnav.png)

    ![](./Images/fabricnav2.png)
   
1. In the New item pane, search for **Eventhouse (1)** and select **Eventhouse (2)** from the list.

    ![](./Images/evt.png)

1. Create a new **Eventhouse** with the name **Eventhouse-<inject key="DeploymentID" enableCopy="false"/> (1)** and click **Create (2)**.

    ![](./Images/e3t1p3.png)

1. In the **Welcome to Eventhouse!** pop-up, click **Get started**.

    ![](./Images/e3t1p4.png)

1. Once the Eventhouse is created, in the settings pane, click **+ Database** from the top menu for the **KQL Databases**.

   ![](./Images/e3t1p5.png)

1. Enter the following details:

   - Database Name: **KQL-Database<inject key="DeploymentID" enableCopy="false"/> (1)**.

   - Click on **Create (2)**.

     ![](./Images/kqlcr.png)

1. In the center of the screen, click on **Get data (1)** and then select **Local file (2)**.

   ![01](./Images/getdatalocalfile.png)

1. Use the wizard to import the data into a new table by selecting the following options:
   
    - **Source**:
        - **Database:** *The database you created is already selected*
        - **Table:** Click on **+ New table**.
        - **Name:**  **sales (1)**.
        - **Upload files:** Drag or Browse for the file from **`C:\LabFiles\Files\sales.csv` (2)**
        - Click **Next (3)**

            ![01](./Images/fabric17.png)

    - **Inspect:** Preview the data, enable **First row header (1)** and click on **Finish (2)**.

        ![](./Images/e3t1p7.png)
    
    - **Summary:**
    
        - Review the preview of the table and click on **Close**.

            ![](./Images/e3t1p8.png)
    
    > **Note:** In this example, you imported a very small amount of static data from a file, which is fine for this exercise. In reality, you can use Kusto to analyze much larger volumes of data, including real-time data from a streaming source such as Azure Event Hubs.

## Task 2: Use KQL to query the sales table

In this task, you will use Kusto Query Language (KQL) to query the sales table in your KQL database. With the data now available, you can write KQL code to extract insights and perform analysis on the sales data.

1. Make sure you have the **sales** table highlighted. Click on **Ellipsis (...) (1)** next to the **sales** table, select the **Query with code (2)**, and then click on **Show any 100 records (3)**.

    ![](./Images/E3T2S1.png)

1. A new pane opens displaying the query and its results.

1. Modify the query as follows:

    ```kusto
   sales
   | where Item == 'Road-250 Black, 48'
    ```

1. **Run** the query. Then review the results, which should contain only the rows for sales orders for the *Road-250 Black, 48* product.

    ![](./Images/E4-T2-S4.png)
   
1. Modify the query as follows:

    ```kusto
   sales
   | where Item == 'Road-250 Black, 48'
   | where datetime_part('year', OrderDate) > 2020
    ```

1. **Run** the query and review the results, which should contain only sales orders for *Road-250 Black, 48* made after 2020.

    ![](./Images/e3t2p6.png)

1. Modify the query as follows:

    ```kusto
   sales
   | where OrderDate between (datetime(2020-01-01 00:00:00) .. datetime(2020-12-31 23:59:59))
   | summarize TotalNetRevenue = sum(UnitPrice) by Item
   | sort by Item asc
    ```

1. **Run** the query and review the results, which should contain the total net revenue for each product between January 1st and December 31st 2020, in ascending order of product name.

    ![](./Images/e3t2p8.png)

1. From the top open window, select the **KQL-Database<inject key="DeploymentID" enableCopy="false"/> (1)** and rename it to **Revenue by Product (2)**.

    ![](./Images/E3T2S9.png)

## Task 3: Create a Power BI report from a KQL Queryset

In this task, you will create a Power BI report using your KQL Queryset as the foundation for the analysis. This allows you to visualize and present the insights derived from your KQL queries in an interactive and user-friendly format within Power BI.

1. In the query workbench editor for your query set, run the query and wait for the results.

1. Select **Create Power BI Report** and wait for the report editor to open.

    ![Screenshot of a report from a KQL query.](./Images/upimagekql.png)

1. In the report editor, in the **Data** pane, expand **Kusto Query Result** and select the checkboxes for **Item** and **TotalNet Revenue** fields.

1. On the report design canvas, select the table visualization that has been added, and then in the **Visualizations** pane, select **Clustered bar chart**.

    ![Screenshot of a report from a KQL query.](./Images/update3.png)

1. In the **Power BI (preview)** window, in the **File (1)** menu, select **Save (2)**. Then save the report as **Revenue by Item (3)** in the **fabric-<inject key="DeploymentID" enableCopy="false"/> (4)** where your lakehouse and KQL database are defined using a **Non-Business** sensitivity label from the drop-down. Click on **Continue (5)**

    ![](./Images/bisave.png)

    ![](./Images/rbi.png)

    >**Note:** If you are not getting option to **Save** the report in the **fabric-<inject key="DeploymentID" enableCopy="false"/>** then follow the below steps:

    -  Enter the file name as **Revenue by Item** and click **Continue** to save the Power BI report to the workspace.

         ![](./Images/38.png)

    -  The report has been saved successfully. Now, click on **Open the file in Power BI to view, edit, and get a shareable link** to proceed.

         ![](./Images/39.png)

    - Click **File (1)** and then select **Save a copy (2)** to duplicate the Power BI report to your workspace.
         
         ![](./Images/40.png)

    - Select **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** where you want to save the copied report, enter a name as **Revenue by Item (2)**, and click the **Save (3)** button to finalize the copy.
       
         ![](./Images/up41.png)

        >**Note**: Refresh the Workspace page if necessary to view all of the items it contains.

1. In the list of items in your workspace, note that the **Revenue by Item** report is listed. 
   
> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Navigate to the Lab Validation Page, from the upper right corner in the lab guide section.
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="c742ae58-7ab2-4511-a05c-401839b6ca9e" />

## Summary

In this exercise, you:
- Created a Lakehouse to store and manage structured data.
- Set up a KQL (Kusto Query Language) database to analyze the data stored in the Lakehouse.
- Used KQL queries to explore and extract insights from the data.
- Created a query set based on your KQL analysis.

### You have successfully completed the exercise. Click on Next >> to proceed with the next exercise.

![05](./Images/nextpage(1).png)
