# Exercise 6: Create a Dataflow (Gen2) in Microsoft Fabric

### Estimated Duration: 50 Minutes

In this exercise, you'll explore data ingestion and transformation in Microsoft Fabric using Dataflow Gen2. You'll begin by creating a Dataflow to import and shape sales data using Power Query Online. Then, you'll define a lakehouse as the data destination, configure column mappings, and publish the Dataflow. Finally, you'll integrate the Dataflow into a pipeline to automate data processing and verify that the transformed data is successfully loaded into the lakehouse for future analysis.

## Objectives

In this exercise, you will be able to complete the following tasks:

- Task 1: Create a Dataflow (Gen2) to ingest data
- Task 2: Add data destination for Dataflow
- Task 3: Add a dataflow to a pipeline

## Task 1: Create a Dataflow (Gen2) to ingest data

In this task, you will create a Dataflow (Gen2) to efficiently ingest and transform data from multiple sources for analysis. This process streamlines data preparation, enabling you to prepare the data for further processing and insights.

1. From the left navigation pane, select your **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** workspace, click on **+ New item (2)**. 

    ![](./Images/p7t1p1.png)

1. In the Search box search for **Dataflow Gen2 (1)** and select **Dataflow Gen2 (2)**. 

   ![](./Images/E6T1S2-1208.png)
   
1. Enter the below-mentioned details to create the Dataflow and click on **Create (3)**.

   - **Name:** Keep as default **(1)**
   - **Enable Git integration, deployment pipelines, and Public API Scenarios:** Uncheck **(2)**

      ![](./Images/E6T1S3.png)

   After a few seconds, the Power Query editor for your new dataflow will open.

1. From the center **Get data** pane, select **Import from a Text/CSV file**.

   ![](./Images/E6T1S4-1208.png)

1. Create a new data source with the following settings:

    - **Link to file: (1)** *Selected*
    - **File path or URL: (2)** `https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/orders.csv`
    - **Connection: (3)** Create new connection
    - **Connection Name:** Connection
    - **Data gateway: (4)** (none)
    - **Authentication kind: (5)** Anonymous
    - **Privacy level: (6)** None
    - Click **Next (7)**

      ![Get data](./Images/p7t1p5.png)

1. Preview the file data, and then click **Create** the data source. The Power Query editor shows the data source and an initial set of query steps to format the data, as shown below:

   ![Query in the Power Query editor.](./Images/E6T1S6-1208.png)

1. Select the **Add column  (1)** tab on the toolbar ribbon. Then, choose **Custom column (2)**.

   ![](./Images/Flow5.png)

1.  On the Custom column pane, create a new column with Name **MonthNo (1)** and enter the formula **Date.Month([OrderDate]) (2)** in the **Custom column formula** box and then click **OK (3)**.

      ![](./Images/E6T1S7.2-1208.png)

1. The step to add the custom column is added to the query, and the resulting column is displayed in the data pane:

   ![Query with a custom column step.](./Images/E6T1S8-1208.png)

1. On the **Power Query editor** page, click the **Close** button at the top-right corner to exit the editor.  

   ![](./Images/e6s12.png)

4. On the **Close** confirmation dialog box, click **Yes** to confirm and exit.  

   ![](./Images/e6s13.png)

1. From the left pane, go to the **fabric_lakehouse<inject key="DeploymentID" enableCopy="false"/> (1)** Lakehouse, and then right-click on the **orders (2)** file and click on **Delete (3)**.

   ![](./Images/E6T1S9-1208.png)

1. On the **Delete "orders"?** pop-up, click **Delete**.
   
   ![](./Images/e6p7t1p13.png)

## Task 2: Add data destination for Dataflow

In this task, you’ll add a data destination for the Dataflow to determine where the ingested and transformed data will be stored for future use.

1. Go back to the previous tab where the Dataflow Gen2 is opened. 

1. In the **Query Settings** in the right pane, click on **+ (1)** for Data destination, then choose **Lakehouse (2)** from the drop-down menu.

   ![Empty data pipeline.](./Images/Flow6.png)

   >**Note:** If this option is greyed out, you may already have a data destination set. Check the data destination at the bottom of the Query settings pane on the right side of the Power Query editor. If a destination is already set, you can change it using the gear.

1. In the **Connect to data destination** dialog box, make sure **Create a new connection (1)** is selected and the **<inject key="AzureAdUserEmail"></inject> (2)** account is signed in. Click on **Next (3)**.

   ![](./Images/p7t2p3.png)

1. Select the **fabric-<inject key="DeploymentID" enableCopy="false"/>** Workspace. Choose the **fabric_lakehouse<inject key="DeploymentID" enableCopy="false"/> (1)** then specify the new table name as **orders (2)**, then click **Next (3)**.

   ![](./Images/p7t2p4.png)

1. On the Destination settings page, observe that **MonthNo** is not selected in the Column mapping, and an informational message is displayed.

   ![](./Images/E6T2S4-1208.png)
 
1. On the Destination settings page, first **toggle off (1)** the **Use automatic settings** option. Next, under the **MonthNo** column header, set the **Source type** to **Whole number (2)**. Finally, click **Save settings (3)** to apply the changes.

   ![](./Images/e6p7t2p6.png)

1. Select **Publish** to publish the dataflow. Then wait for the **Dataflow** to be created in the workspace.

   ![](./Images/p7t2p7.png)

1. After publishing, you will be taken back to the home page of the **Fabric portal**. Wait for a few minutes for the Publish to complete, then open the **Dataflow**. 

1. Click on the **Dataflow (1)** on the top left, and rename the dataflow as **Transform Orders Dataflow (2)**.

   ![](./Images/p7t2p9.png)

## Task 3: Add a dataflow to a pipeline

In this task, you’ll add a dataflow to a pipeline to streamline the data processing workflow and enable automated data transformations.

1. From the left navigation pane, select your **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** workspace, click on **+ New item (2)**. 

    ![](./Images/E1T3S2-1108.png)

1. In the search box, search for **Pipeline (1)**, and select **Pipeline (2)**.

    ![](./Images/p7t3p2.png)

1. Set the Name as **Load Orders pipeline (1)** and click on **Create (2)**. This will open the pipeline editor.

   ![](./Images/Flow10.png)

   > **Note:** If the Copy Data wizard opens automatically, close it!

1. Click on the **Pipeline activity (1)**, and select **Dataflow (2)** activity.

   ![](./Images/p7t3p4.png)

1. With the new **Dataflow1** activity selected, go to the **Settings (1)** tab in the bottom. In the **Workspace** drop-down list, choose **fabric-<inject key="DeploymentID" enableCopy="false"/> (2)** and in **Dataflow** drop-down list, select **Transform Orders Dataflow (3)** (the data flow you created previously).

   ![Empty data pipeline.](./Images/E6T3S5-1208.png)
   
1. **Save** the pipeline from the top left corner.

   ![](./Images/Flow11.png)

1. Use the **Run** button to run the pipeline, and wait for it to complete. It may take a few minutes.

   ![](./Images/Flow12.png)
   
   ![](./Images/p7t3p7.png)

1. From the left navigation pane, select **fabric_lakehouse<inject key="DeploymentID" enableCopy="false"/>** Lakehouse.

1. Expand the **Tables** section and verify that the **orders** table is created by your dataflow.

   ![Table loaded by a dataflow.](./Images/Orders11.png)

   >**Note:** You might have to refresh the browser to get the expected output.

## Summary

In this exercise, you:

- Created a **Dataflow (Gen2)** to ingest and prepare data.
- Added a **data destination** to store the output of the Dataflow.
- Integrated the **Dataflow into a pipeline** for automated data processing.

### Conclusion
By completing this **MS Fabric Foundation for Enterprise Analytics** hands-on lab, you have gained valuable, hands-on experience with the end-to-end capabilities of Microsoft Fabric for enterprise analytics. Starting from creating a collaborative workspace, you have learned to ingest and prepare data using pipelines, perform advanced analyses in a data warehouse, and access real-time insights through live analytics. You also practiced training machine learning models in notebooks, harnessed the scalability of Apache Spark for large-scale analysis, and designed sophisticated data transformation workflows with Dataflow Gen2. This comprehensive journey empowers you to confidently manage, analyze, and transform data, equipping you with practical skills to drive informed, data-driven decision-making within your organization.

### You have successfully completed the Hands-on lab.
