# Exercise 7: Create a Dataflow (Gen2) in Microsoft Fabric

### Estimated Duration: 40 Minutes

In Microsoft Fabric, Dataflows (Gen2) connect to various data sources and perform transformations in Power Query Online. They can then be used in Data Pipelines to ingest data into a lakehouse or other analytical store or to define a dataset for a Power BI report.

This lab is designed to introduce the different elements of Dataflows (Gen2), and not create a complex solution that may exist in an enterprise.

## Lab objectives

You will be able to complete the following tasks:

- Task 1: Create a Dataflow (Gen2) to ingest data
- Task 2: Add data destination for Dataflow
- Task 3: Add a dataflow to a pipeline

### Task 1: Create a Dataflow (Gen2) to ingest data

In this task, you will create a Dataflow (Gen2) to efficiently ingest and transform data from multiple sources for analysis. This process streamlines data preparation, enabling you to prepare the data for further processing and insights.

1. Navigate to your workspace named as **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** from the left navigation pane, click on **+ New item (2)** to create a Dataflow.

    ![](./Images/E1T1S2.png)

1. In the All items search for **Dataflow Gen2** and select it from the list.

   ![01](./Images/02/pg08-creategen2.png)

1. provide a **Name** for the New Dataflow Gen, and make sure **Enable Git intergation, deployment pipelines and Public APU scenarios** is checked. And click on **Create**.

1. Select **Import from a Test/CSV file** and create a new data source with the following settings:
    - **Link to file**: *Selected*
    - **File path or URL**: `https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/orders.csv`
    - **Connection**: Create new connection
    - **Connection name**: dataconnection
      >**Note:** Data connection name would change automatically along with your username, please ignore and continue
    - **data gateway**: (none)
    - **Authentication kind**: Anonymous
    - **Privacy level**: None

       ![01](./Images/02/pg08-createdatasource.png)

1. Select **Next** to preview the file data, and then **Create** the data source. The Power Query editor shows the data source and an initial set of query steps to format the data, as shown below:

   ![Query in the Power Query editor.](./Images/fabric23.png)

1. Select the **Add column** tab on the toolbar ribbon. Then, choose **Custom column** and create a new column named **MonthNo** using the formula `Date.Month([OrderDate])`.

   ![Custom column in Power Query editor.](./Images/fabric24.png)

1. The step to add the custom column is added to the query and the resulting column is displayed in the data pane:

   ![Query with a custom column step.](./Images/lak4.png)

### Task 2: Add data destination for Dataflow

In this task, you’ll add a data destination for the Dataflow to determine where the ingested and transformed data will be stored for future use.

1. From the bottom right corner, choose **Lakehouse** from the **Add data destination** drop-down menu.

   ![Empty data pipeline.](./Images/35.png)

   >**Note:** If this option is greyed out, you may already have a data destination set. Check the data destination at the bottom of the Query settings pane on the right side of the Power Query editor. If a destination is already set, you can change it using the gear.

2. In the **Connect to data destination** dialog box, edit the connection by selecting **Create a new connection**. Then, sign in with your Power BI organizational account to establish the identity that the dataflow will use to access the lakehouse.

   ![Data destination configuration page.](./Images/lak1.png)

4. Select **Next**, Select the **fabric-<inject key="DeploymentID" enableCopy="false"/>**. Choose the **lakehouse**, then specify a new table named **orders**.

   ![Data destination configuration page.](./Images/fabric26.png)

5. On the Destination settings page, observe that **MonthNo** is not selected in the Column mapping, and an informational message is displayed.
 
6. On the Destination settings page, toggle off the **Use Automatic Settings** option. Then, right-click on the **MonthNo** column header and select **Change Type** to set **MonthNo** as a **Whole number**. Finally, click on **Save Settings**.

    ![Data destination settings page.](./Images/lak2.png)

5. Select **save** to publish the dataflow. Then wait for the **Dataflow** to be created in the workspace.

6. Once saved,click on the **Dataflow 1** and rename the dataflow as **Transform Orders Dataflow**.

   ![01](./Images/02/pg08-savedataflow.png)

### Task 3: Add a dataflow to a pipeline

In this task, you’ll add a dataflow to a pipeline to streamline the data processing workflow and enable automated data transformations.

1. Navigate back to the workspace, click on **+ New item** and select **Data pipeline**. Name the pipeline as **Load Orders pipeline**. This will open the pipeline editor.

    ![](./Images/E1T3S1.png)
  
   > **Note**: If the Copy Data wizard opens automatically, close it!

3. Select **pipeline activity**, and add a **Dataflow** activity to the pipeline.

   ![Empty data pipeline.](./Images/dataflow_1.png)

4. With the new **Dataflow1** activity selected, go to the **Settings (1)** tab. In the **Dataflow** drop-down list, choose **fabric-<inject key="DeploymentID" enableCopy="false"/>** or my workspace (2) and select **Transform Orders Dataflow (3)** 

   ![Empty data pipeline.](./Images/transform.png)
   
6. **Save** the pipeline from the top left corner.

7. Use the **Run** button to run the pipeline, and wait for it to complete. It may take a few minutes.

   ![Pipeline with a dataflow that has completed successfully.](./Images/lak8.png)

8. In the menu bar on the left edge, select **fabric_lakehouse<inject key="DeploymentID" enableCopy="false"/>**

9. Expand the **Tables** section and select the **orders** table created by your dataflow.

   ![Table loaded by a dataflow.](./Images/orders_1.png)

   >**Note:** You might have to refresh the browser to get the expected output.

### Summary

In this exercise, you have created a Dataflow (Gen2) to ingest data, added a data destination for Dataflow and added a Dataflow to a pipeline.

### You have successfully completed the lab
