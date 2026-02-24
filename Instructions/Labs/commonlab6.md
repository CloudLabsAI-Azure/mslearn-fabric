# Lab 01: Create a Fabric workspace

### Estimated Duration: 30 Minutes

Microsoft Fabric allows you set up workspaces depending on your workflows and use cases. A workspace is where you can collaborate with others to create reports, notebooks, lakehouses, etc. 
This lab will introduce you to creating a workspace in Microsoft Fabric. You will learn how to set up a workspace, which serves as a collaborative environment for organizing and managing your projects, data, and resources.

## Lab objectives

You will be able to complete the following tasks:

- Task 1: Assign Fabric Administrator Role
- Task 2: Sign up for Microsoft Fabric Trial
- Task 3: Create a workspace
- Task 4: Create a Lakehouse and upload files

#### Task 1: Assign Fabric Administrator Role

In this task, you will assign the **Fabric Administrator role** in Microsoft Entra ID through the Azure portal to manage permissions and access within the Azure environment.

1. In the Azure portal, search for **Microsoft Entra ID (1)** using the search bar, and then select **Microsoft Entra ID (2)** from the results.

    ![Navigate-To-AAD](./Images/ws/Fab3.png)

1. Under **Manage (1)**, Navigate to **Roles and administrators (2)**.

    ![Roles-and-Administrator](./Images/ws/Fab4.png)

1. In the **Roles and administrators** page, search for **Fabric Administrator (1)**, and click on it **(2)**.

    ![search-fabric-admin](./Images/ws/Fab5.png)

1. You will be taken to the **Fabric Administrator | Assignments** page. From here, assign yourself the **Fabric Administrator** role by selecting **+ Add assignments**.

    ![click-add-assignments](./Images/ws/Fab6.png)

1. Ensure you **check the box (1)** next to your username, verify that it appears under **Selected (2)**, and then select **Add (3)** to complete the assignment.

    ![check-and-add-role](./Images/ws/Fab7.png)

1. Confirm the **Fabric Administrator** role has been added by selecting **Refresh (1)** on the Fabric Administrators | Assignments page. Once the assignment is visible **(2)**.

    ![check-and-navigate-back-to-home](./Images/ws/Fab8.png)

### Task 2: Sign up for Microsoft Fabric Trial

In this task, you will initiate your 60-day free trial of Microsoft Fabric by signing up through the Fabric app, providing access to its comprehensive suite of data integration, analytics, and visualization tools

1. Copy the **Power BI homepage link** and open it inside the VM in a new browser tab.

   ```
   https://powerbi.com
   ```

   >**Note**: In case a sign-up page asks for a phone number, you can enter a dummy phone number to proceed.

1. Select **Account manager (1)**, and click on **Free trial (2)**.

     ![Account-manager-start](./Images/f1.png)

1. A new prompt will appear asking you to **Activate your 60-day free Fabric trial capacity**, click on **Activate**.

    ![image](https://github.com/user-attachments/assets/d12f89b8-955d-4e0c-b671-977c34e152d1)

    > **Note:** The trial capacity region may differ from the one shown in the screenshot. No need to worry – simply use the default selected region, activate it, and continue to the next step.  

1. Click on **Stay on current page** when prompted.

      ![Account-manager-start](./Images/fabric-2.png)

1. Now, open **Account manager (1)** again, and verify **Trial Status (2)**.

      ![Account-manager-start](./Images/lab1-image5.png)
      
### Task 3: Create a workspace

Here, you create a Fabric workspace. The workspace contains all the items needed for this lakehouse tutorial, which includes lakehouse, dataflows, Data Factory pipelines, notebooks, Power BI datasets, and reports.

1. Now, select **Workspaces (1)** and click on **+ New workspace (2)**.

    ![New Workspace](./Images/f2.png)

1. Fill out the **Create a workspace** form with the following details:
 
   - **Name:** Enter **fabric-<inject key="DeploymentID" enableCopy="false"/>**
 
      ![name-and-desc-of-workspc](./Images/f3.png)
 
   - **Advanced:** Expand it and Under **License mode**, select **Fabric (1)**, Under **Capacity**, select available **fabric<inject key="DeploymentID" enableCopy="false"/> - <inject key="Region"></inject>(2)** and click on **Apply (3)** to create and open the workspace.
 
      ![advanced-and-apply](./Images/ws/Fab20.png)

## Task 4: Create a Lakehouse and upload files

In this task, switch to the Data engineering experience and create a new Lakehouse. You'll use it to ingest and manage data in the following steps.

1. At the bottom left of the Power BI portal, select the **Power BI (1)** icon and switch to the **Fabric (2)** experience.

   ![](./Images/upfab-ric-ex1-g3.png)

   ![](./Images/E1T3S1ii.png)
   
1. In the **Welcome to the Fabric view** window, click **Cancel**.

    ![](./Images/ex1t3p2.png)

1. In the left pane, navigate to your Workspace named as **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)**, click on **+ New item (2)** to create a new lakehouse.

    ![](./Images/newitem.png)

    >**Note:** To navigate to your workspace click on **Workspaces (1)** from the left navigation panel and select your Workspace named as **fabric-<inject key="DeploymentID" enableCopy="false"/> (2)**

    ![](./Images/nav.png)

1. In the search box, search for **Lakehouse (1)** and select **Lakehouse (2)** from the list.

    ![](./Images/Lake1.png)

1. In the **New lakehouse** window, enter the **Name** as **Lakehouse_<inject key="DeploymentID" enableCopy="false"/> (1)** and make sure to **unhcheck Lakehouse Schemas box (2)** click on **Create (3)**.

    ![](./Images/lhcreate.png)

1. On the **Lakehouse_<inject key="DeploymentID" enableCopy="false"/>** tab in the left pane, click the **Ellipsis (...) (1)** menu for the **Files** node, select **New subfolder (2)**.
    
    ![](./Images/filesub.png)

1. In the **New subfolder** window, enter the name as **new_data (1)** and click on **Create (2)**.

    ![](./Images/Lake5.png)

1. Select the **More options (1)** next to the **new_data** folder, choose **Upload (2)**, and then select **Upload files (3)** to add files to the Lakehouse.

     ![03](./Images/ws/Fab10.png)

1. In the **Upload files** dialog, select the **folder icon** to browse and choose the files you want to upload to the Lakehouse.

     ![03](./Images/ws/Fab11.png)

1. Browse the path **C:\LabFiles\Files** and select the file **sales.csv (1)**. Then click on **Open (2).**

     ![03](./Images/ws/Fab12.png)

1. In the **Upload files** dialog, select the file **sales.csv (1)**, and then click **Upload (2)** to upload the file to the Lakehouse.

     ![03](./Images/ws/Fab13.png)

1. Verify that the file **sales.csv (1)** has been uploaded successfully, and then close the **Upload files** dialog by selecting **Close (2)**.

     ![03](./Images/ws/Fab14.png)

1. In the menu bar on the left, select your lakehouse.
   
1. On the **Home** page, in the **Lakehouse explorer (1)** pane, expand **Files**, select the **new_data (2)** folder, and verify that the **sales.csv (3)** file is present.

     ![03](./Images/ws/Fab15.png)


> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
- Navigate to the Lab Validation Page from the upper right corner in the lab guide section.
- Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="10b922e7-a6de-4df2-807d-f4d27a3ee0f6" />

### Summary

In this exercise, you have signed up for the Microsoft Fabric Trial and created a workspace.

### You have successfully completed the exercise. Click on Next >> to proceed with the next exercise.