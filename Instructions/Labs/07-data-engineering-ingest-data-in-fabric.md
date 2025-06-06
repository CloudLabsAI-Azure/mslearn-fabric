# Exercise 6: Data Engineering Ingest Data in Fabric with Fabric Copilot

### Estimated Duration: 30 minutes
 In the labs, you'll create a simple data pipeline to bring in customer sales data. You are using the KiZAN Fabric Tenant - where we have Copilot enabled, demonstrate doing the same thing, but by using a new Data Flow Gen2 And leveraging the native integration of Copilot to use natural language to ingest and transform your data.

## Lab objectives

You will be able to complete the following tasks:

- Connect to Data Sources  
- Configure Ingestion Settings  
- Ingest Data into Fabric  
- Monitor Ingestion Process  
- Validate Ingested Data  
- Document the Ingestion Process
   
# Create New - Dataflow Gen2

1. Navigate back to your workspace, click on **+ New item**. Select **Dataflow Gen2**.

   ![New dataflow.](./Images/E6T1S1.png)

1. After a few seconds, the Power Query editor for your new dataflow opens as shown here.

   ![New dataflow.](./Images/new-dataflow.png)

1. Select **Import from a Text/CSV file**, and create a new data source with the following settings:

    - **Link to file**: *Selected*
    - **File path or URL**: `https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/sales.csv`
    - **Connection**: Create new connection
    - **data gateway**: (none)
    - **Authentication kind**: Anonymous
    - Click on **Next**.

   ![New dataflow.](./Images/29.png)

1. On **Preview file data** page, Click on **Create**.

   ![New dataflow.](./Images/30.png)

1. Click **Copilot** button on Ribbon. 

    ![New dataflow.](./Images/E6T1S6.png)
   > Note: If you are unable to locate Copilot, click Expand Ribbon in the top-right corner.
   ![New dataflow.](./Images/cop-1.png)

1. To give you a clearer view of everything Copilot is doing, expand the UI slightly to reveal what’s happening behind the scenes.

1. Expand **Query Settings** (these are the steps in the query that bring in the data)
   ![New dataflow.](./Images/cop-2.png)

1. **View menu**: Look for the option or menu labeled "View" in the toolbar or menu bar. Click on it to reveal a dropdown menu.
   ![New dataflow.](./Images/cop-3.png)

1. **Diagram View**: From the dropdown menu, select "Diagram View." This will switch your view to a visual representation of your code.
   ![New dataflow.](./Images/cop-4.png)

1. **Script View**: Once you're in Script View, you should be able to see the M-Code that Copilot has generated. This is the underlying code representation of the actions or commands depicted in the Diagram View.

   ![New dataflow.](./Images/1.png)

1. Looking at the data… Notice the Item Column.

1. This is really three different fields -- It contains a short description of the item, a color and a size.

1.	The fields are not consistently delimited (' ' and then ',')

1. Let's use Copilot to clean this up:

    ```
   	Add a step that
    ```
    ![New dataflow.](./Images/3.png)

1. Type the following into Copilot:
 
    ```
    In the Item column, remove the ','
    ```
 
1. The Item column now consistently has a delimiter of **' '**.

   ![New dataflow.](./Images/4.png)

1. Type the following query into Copilot:
 
    ```
    Split the Item column on the ' ', creating three new fields called Description, Color and Size
    ```
 
1. Three new fields are now created to replace the Item Column.

   ![New dataflow.](./Images/5.png)
 
1. Show the m-code and new query step that the copilot generated
 
   >**Note:** Copilot can do more than transform the table, we can actually change data as well.

1. **Scenario:** think Red bikes are going to be a big seller, so increase the quantity in Inventory
Call out that the quantities for all items are 1.

1. Add a step to type the following query into Copilot:
 
    ```
    Add a new column named QuantityNew where for each row, if the Color column is equal to "Red", multiply the Quantity by 10, otherwise keep the same value. Then remove the old Quantity column and rename QuantityNew back to Quantity.
    ```
 
1. Show that the quantity field for Red bikes is now 10 instead of 1.

   ![New dataflow.](./Images/6a.png)
 
1. Here is a concise summary highlighting the impacts of Visual Query and M-Query/M-Code scripting:

1. Close the Report.

1. **Visual Query**:
   - **Streamlines data exploration**: Visual Query tools offer intuitive interfaces, enabling users to interact with data visually, and facilitating quicker insights without extensive coding.
   - **Enhances accessibility**: With Visual Query, users with varying technical expertise can extract insights from data, reducing reliance on specialized programming skills and promoting broader data utilization across teams.

2. **M-Query/M-Code scripting**:
   - **Enables advanced data manipulation**: M-Query/M-Code scripting provides a robust framework for performing intricate data transformations and analysis, empowering users to tailor processes to specific requirements beyond the capabilities of visual tools.
   - **Facilitates automation and customization**: Through M-Query/M-Code scripting, users can automate repetitive tasks, build custom functions, and create tailored solutions, increasing efficiency and flexibility in data workflows.

## Summary
In this exercise, you have created a notebook and trained a machine-learning model. You used Scikit-Learn to train the model and MLflow to track its performance.

## You have successfully completed the lab. Click on Next >> to procced with next exercise.
