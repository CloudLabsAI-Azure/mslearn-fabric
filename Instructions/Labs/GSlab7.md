# Cloud Scale Analytics with Microsoft Fabric

### Overall Estimated Duration : **90 Minutes**

## Overview

Microsoft Fabric is a unified data platform that combines data engineering, data warehousing, and business intelligence tools into a cohesive environment. By leveraging Microsoft Fabric, organizations can effectively manage, analyze, and visualize large datasets, enabling powerful data-driven decision-making processes.

In this hands-on lab, you will explore and set up Microsoft Fabric by creating a workspace and assigning the Fabric Administrator role. You will then create a Lakehouse to centralize and manage data, followed by uploading files for analysis. Additionally, you will learn how to create and manage dataflows in Microsoft Fabric to efficiently ingest, transform, and store data. You will create a Dataflow (Gen2) to ingest data from an external source, perform transformations using Power Query, and define a data destination in a Lakehouse. Furthermore, you will integrate the Dataflow into a Data Pipeline to automate and orchestrate the ETL (Extract, Transform, Load) process. This lab provides practical experience with Microsoft Fabric’s data management and pipeline orchestration capabilities.

## Objective

Learn to manage workspaces and perform data transformations in Microsoft Fabric by creating Lakehouses, Dataflows, and integrating them into Pipelines for seamless ETL processes.

- **Getting Started with Microsoft Fabric:** Learn how to create and manage a workspace in Microsoft Fabric by assigning the Fabric Administrator role, setting up a new workspace for data and analytics projects, and organizing your work effectively. Additionally, gain hands-on experience in setting up a Lakehouse within the workspace and uploading data files (e.g., CSV) to the Lakehouse, laying the foundation for future data analysis and processing.

- **Create a Dataflow (Gen2) in Microsoft Fabric:** In this lab, participants will learn how to create and manage Dataflows (Gen2) in Microsoft Fabric to efficiently ingest, transform, and store data. They will explore how to import data from external sources, apply transformations using Power Query, and define a destination in a Lakehouse. The lab will also cover integrating Dataflows into Data Pipelines to automate and orchestrate the ETL (Extract, Transform, Load) process, providing practical experience in managing data workflows and optimizing data processing tasks within Microsoft Fabric.

## Prerequisites

Participants should have:

- **Basic Knowledge of Microsoft Azure**: Familiarity with the Azure portal and the process of role assignments within Azure Active Directory (Entra ID).
- **Understanding of Workspace Creation**: Familiarity with the concept of workspaces in cloud platforms and how to create and configure them.
- **Basic Knowledge of Cloud Data Storage**: Understanding the concept of Lakehouses for organizing and storing data in cloud environments.
- **Basic File Management Skills**: Ability to upload files into a cloud-based data platform like Microsoft Fabric for further analysis.
Here are the prerequisites based on the lab objectives you provided:
- **Basic Understanding of Dataflows (Gen2)**: Familiarity with the concept of Dataflows, especially Dataflow (Gen2) in Microsoft Fabric, and how they are used for data ingestion and transformation.
- **Basic Knowledge of Data Pipelines**: Understanding of how to create and manage data pipelines in Microsoft Fabric to orchestrate ETL (Extract, Transform, Load) processes.
- **Familiarity with Microsoft Fabric Workspace**: Comfort with navigating Microsoft Fabric’s workspace, including creating new items like Dataflows and Data Pipelines.
- **Experience with Data Destination Configuration**: Ability to configure and set data destinations, such as a Lakehouse, for storing ingested data from Dataflows.

## Architecture

The architecture leverages Microsoft Fabric to create an efficient and automated data workflow, integrating workspaces, lakehouses, dataflows, and pipelines. It begins with the assignment of the **Fabric Administrator Role** to ensure proper permissions and access control. The **Workspace** is then created, serving as the central hub for managing and collaborating on data resources. Following this, a **Lakehouse** is set up as the data storage solution, allowing for scalable and efficient storage of large datasets. Files are uploaded into the Lakehouse, populating it with the necessary data for further processing. Next, a **Dataflow (Gen2)** is configured to ingest, transform, and load (ETL) data from external sources. The transformed data is directed into the Lakehouse as the destination for storage. Finally, the **Dataflow** is integrated into a **Data Pipeline**, which automates the data ingestion and transformation process, ensuring the data remains up-to-date and organized within the Lakehouse.

This architecture establishes a seamless flow from data storage setup to automated data ingestion, transformation, and orchestration, providing an efficient environment for data management and analysis within Microsoft Fabric.

## Architecture Diagram 

![Navigate-To-AAD](./Images/Arch6.png)

## Explanation of Components

The architecture for this lab involves the following key components:

### Lab 01 Components:

- **Fabric Administrator Role:** Ensures that the necessary permissions are granted to manage the workspace effectively, enabling access control and administrative capabilities.

- **Workspace:** A central environment where data and resources are managed. The workspace acts as the foundation for organizing and collaborating on data storage and analysis tasks.

- **Lakehouse:** A data storage solution designed for structured and unstructured data. The Lakehouse enables efficient data organization and facilitates analysis tasks.

- **Data Ingestion Tools:** Enables uploading of files into the Lakehouse to populate datasets required for analysis and querying. 

### Lab 02 Components:

- **Dataflow (Gen2) for Data Ingestion:**  
  The Dataflow (Gen2) is a powerful component in Microsoft Fabric that enables users to perform data extraction, transformation, and loading (ETL). Dataflow is used to ingest data from an external source, such as a CSV file. It allows users to define the steps needed to clean, transform, and structure the data for further processing or analysis.

- **Power Query Editor for Data Transformation:**  
  The Power Query Editor is used within the Dataflow to perform data transformations. users can apply transformations like adding custom columns, changing data types, and modifying data formats to suit the required output. This transformation process prepares the data for optimal storage and future use.

- **Lakehouse as Data Destination:** Lakehouse is configured as the destination for the dataflow. The Lakehouse serves as a storage solution for the transformed data, allowing users to store large datasets in a scalable, organized manner. Data from the Dataflow is mapped and written to Lakehouse tables for further use in analysis or reporting.

- **Data Pipeline for Orchestration:** Dataflow is integrated into a Data Pipeline to automate the data ingestion and transformation process. The Data Pipeline orchestrates the flow of data, ensuring that the Dataflow runs as part of an automated process, reducing manual intervention and ensuring that data is regularly updated in the destination (Lakehouse).

## Getting Started with the Lab 

Once you're ready to dive in, your virtual machine and lab guide will be right at your fingertips within your web browser.

![08](./Images/Gs001.png) 

## Virtual Machine & Lab Guide

In the integrated environment, the lab VM serves as the designated workspace, while the lab guide is accessible on the right side of the screen.

**Note**: Kindly ensure that you are following the instructions carefully to ensure the lab runs smoothly and provides an optimal user experience.

## Exploring Your Lab Resources

To get a better understanding of your lab resources and credentials, navigate to the **Environment** tab.

![08](./Images/GS2.png)
   
## Utilizing the Split Window Feature
 
For convenience, you can open the lab guide in a separate window by selecting the **Split Window** button from the Top right corner.
 
![Use the Split Window Feature](./Images/GS3.png) 

## Lab Guide Zoom In/Zoom Out
 
To adjust the zoom level, select the **A↕ (1)** icon next to the timer, and then choose the required **zoom percentage (2)** from the dropdown.

![08](./Images/GS4.png)  

## Managing Your Virtual Machine

Feel free to start, stop, or restart your virtual machine by selecting **More (1)**, choosing **Resources (2)**, and using the available **VM actions (3)** to manage your lab environment as needed.

![](Images/GS5.png)
  
## Let's Get Started with Azure Portal

1. On your virtual machine, click on the Azure Portal icon as shown below:

   ![Launch Azure Portal](./Images/GS01.png)
   
1. You'll see the **Sign into Microsoft Azure** tab. Here, enter your credentials:
 
   - **Email (1):** <inject key="AzureAdUserEmail"></inject>

   - click **Next (2)**.
 
      ![Enter Your Username](./Images/GS002.png)
 
1. Next, provide your **Enter Temporary Access Pass**:
 
   - **Password (1):** <inject key="AzureAdUserPassword"></inject>

   - click **Sign in (2)**.
 
      ![Enter Your Password](./Images/GS003.png)

1. If **Action Required** window pop up click on **Ask later**.
 
1. If prompted to stay signed in, you can click "No."

1. If you see the pop-up **Sign in to sync data**, Click on **No,thanks.** 

1. If you see the pop-up **You have free Azure Advisor recommendations!**, close the window to continue the lab.

1. If a **Welcome to Microsoft Azure** popup window appears, click **Cancel** to skip the tour.

## Support Contact
 
The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

Learner Support Contacts:
- Email Support: cloudlabs-support@spektrasystems.com
- Live Chat Support: https://cloudlabs.ai/labs-support

Now, click on **Next** from the lower right corner to move on to the next page. 

![Launch Azure Portal](./Images/GS004.png) 

### Happy Learning!!