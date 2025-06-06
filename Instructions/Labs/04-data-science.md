# Exercise 5: Use notebooks to train a model in Microsoft Fabric

### Estimated Duration: 70 minutes

In this lab, we will use Microsoft Fabric to create a notebook and train a machine-learning model to predict customer churn. We will use Scikit-Learn to train the model and MLflow to track its performance. Customer churn is a critical business problem that many companies face, and predicting which customers are likely to churn can help companies retain their customers and increase revenue. By completing this lab, you will gain hands-on experience in machine learning and model tracking, and learn how to use Microsoft Fabric to create a notebook for your projects.

## Lab objectives

You will be able to complete the following tasks:

- Task 1: Upload files into the lakehouse
- Task 2: Create a notebook
- Task 3: Load data into a dataframe
- Task 4: Train a machine learning model
- Task 5: Use MLflow to search and view your experiments
- Task 6: Explore your experiments
- Task 7: Save the model
- Task 8: Save the notebook and end the Spark session

### Task 1: Upload files into the lakehouse

In this task, you will create a lakehouse and upload files to facilitate data storage and analysis. Using the same workspace, you'll switch to the *Data Science* experience in the portal to manage and utilize the data effectively.

1. Navigate back to your lakehouse, and in the **Ellipses (1)** menu for the **Files** node in the **Explorer** pane, select **Upload (2)** and **Upload files (3)**. 

   ![](./Images/Pg6-S1.png)

2. Navigate to **C:\LabFiles\Files\churn.csv (1)**, select the **churn.csv (2)** file and click on **Open (3)** to upload it to the lakehouse.   

   ![](./Images/Pg6-S2.png)

3. After the files have been uploaded, expand **Files** and verify that the CSV file has been uploaded.

   ![](./Images/Pg6-S2.1.png)

### Task 2: Create a notebook

In this task, you will create a notebook to facilitate model training and experimentation. Notebooks offer an interactive environment where you can write and execute code in multiple languages, allowing you to conduct experiments effectively.

1. From the left pane, select the workspace named Fabric-<inject key="DeploymentID" enableCopy="false"/>.

    ![](./Images/E2-T4-S1.png) 

2. In the workspace, click on **+ New Item**. In the New Item panel, search for **Notebook** and select it.

    ![](./Images/E2-T4-S2.png) 

1. After a few seconds, a new notebook containing a single *cell* will open. Notebooks are made up of one or more cells that can contain *code* or *markdown* (formatted text).

1. Select the first cell (which is currently a *code* cell), and then in the dynamic toolbar at its top-right, use the **M&#8595;** button to convert the cell to a *markdown* cell.

    ![](./Images/E5-T2-S3.png)

1. When the cell changes to a markdown cell, the text it contains is rendered.

1. Use the **&#128393;** (Edit) button to switch the cell to editing mode, then delete the content and enter the following text:

    ```text
   # Train a machine learning model and track with MLflow

   Use the code in this notebook to train and track models.
    ```    

### Task 3: Load data into a dataframe

In this task, you will load data into a dataframe to prepare for model training. Dataframes in Spark, akin to Pandas dataframes in Python, offer a structured way to work with data in rows and columns, enabling efficient data manipulation and analysis.

1. Click **Add data items (1)** and select **Existing data source (2)** from the drop-down.

    ![](./Images/adddataitem.png)  

1. Select the lakehouse named **Lakehouse-<inject key="DeploymentID" enableCopy="false"/> (1)** and click **Connect (2)**.
 
    ![](./Images/E5T3S2.png) 

1. Once after connecting to the existing lakehouse, we should be able to see the **Lakehouse-<inject key="DeploymentID" enableCopy="false"/>** under **Data Items**.
   
   ![](./Images/E5T3S3.png)

1. Click the **Files (1)** folder so that the CSV file is listed next to the notebook editor.

1. In the **Ellipses** menu for **churn.csv (2)**, select **Load data (3)** > **Pandas (4)**.

    ![](./Images/E5T3S5.png)

1.  A new code cell containing the following code should be added to the notebook:

    ```Python
    import pandas as pd
    # Load data into pandas DataFrame from "/lakehouse/default/" + "Files/churn.csv"
    df = pd.read_csv("/lakehouse/default/" + "Files/churn.csv")
    display(df)
    ```
    
    > **Tip**: You can hide the pane containing the files on the left by using its **<<** icon. Doing so will help you focus on the notebook.

1. Use the **&#9655; Run cell** button on the left of the cell to run it.

    > **Note**: Since this is the first time you've run any Spark code in this session, the Spark pool must be started. This means that the first run in the session can take a minute or so to complete. Subsequent runs will be quicker.

1. When the cell command has been completed, review the output below the cell, which should look similar to this:

    ![](./Images/output.png)

1. The output shows the rows and columns of customer data from the churn.csv file.

### Task 4: Train a machine learning model

In this task, you will train a machine learning model to predict customer churn using the prepared data. Utilizing the Scikit-Learn library, you'll train the model and track its performance with MLflow to ensure effective monitoring and evaluation.

1. Use the **+ Code** icon below the cell output to add a new code cell to the notebook, and enter the following code in it:

    ```python
   from sklearn.model_selection import train_test_split

   print("Splitting dataEllipses")
   X, y = df[['years_with_company','total_day_calls','total_eve_calls','total_night_calls','total_intl_calls','average_call_minutes','total_customer_service_calls','age']].values, df['churn'].values
   
   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.30, random_state=0)
    ```

1. Run the code cell you added, and note you're omitting 'CustomerID' from the dataset, and splitting the data into a training and test dataset.
1. Add another new code cell to the notebook, enter the following code in it, and run it:
    
    ```python
   import mlflow
   experiment_name = "experiment-churn"
   mlflow.set_experiment(experiment_name)
    ```
    
1. The code creates an MLflow experiment named `experiment-churn`. Your models will be tracked in this experiment.

1. Add another new code cell to the notebook, enter the following code in it, and run it:

    ```python
   from sklearn.linear_model import LogisticRegression
   
   with mlflow.start_run():
       mlflow.autolog()

       model = LogisticRegression(C=1/0.1, solver="liblinear").fit(X_train, y_train)

       mlflow.log_param("estimator", "LogisticRegression")
    ```
    
1. The code trains a classification model using Logistic Regression. Parameters, metrics, and artifacts are automatically logged with MLflow. Additionally, you're logging a parameter called `estimator`, with the value `LogisticRegression`.

1. Add another new code cell to the notebook, enter the following code in it, and run it:

    ```python
   from sklearn.tree import DecisionTreeClassifier
   
   with mlflow.start_run():
       mlflow.autolog()

       model = DecisionTreeClassifier().fit(X_train, y_train)
   
       mlflow.log_param("estimator", "DecisionTreeClassifier")
    ```

    >**Note**: If the node fails, attempt to re-run the previous node and then execute the existing node.

1. The code trains a classification model using a Decision Tree Classifier. Parameters, metrics, and artifacts, are automatically logged with MLflow. Additionally, you're logging a parameter called `estimator`, with the value `DecisionTreeClassifier`.

### Task 5: Use MLflow to search and view your experiments

In this task, you will use MLflow to search for and view your experiments related to model training. By leveraging the MLflow library, you can retrieve detailed information about your experiments, helping you assess model performance and make informed decisions.

1. To list all experiments, use the following code:

    ```python
   import mlflow
   experiments = mlflow.search_experiments()
   for exp in experiments:
       print(exp.name)
    ```

1. To retrieve a specific experiment, you can get it by its name:

    ```python
   experiment_name = "experiment-churn"
   exp = mlflow.get_experiment_by_name(experiment_name)
   print(exp)
    ```

1. Using an experiment name, you can retrieve all jobs of that experiment:

    ```python
   mlflow.search_runs(exp.experiment_id)
    ```

1. To more easily compare job runs and outputs, you can configure the search to order the results. For example, the following cell orders the results by `start_time`, and only shows a maximum of `2` results: 

    ```python
   mlflow.search_runs(exp.experiment_id, order_by=["start_time DESC"], max_results=2)
    ```

1. Finally, you can plot the evaluation metrics of multiple models next to each other to easily compare models:

    ```python
   import matplotlib.pyplot as plt
   
   df_results = mlflow.search_runs(exp.experiment_id, order_by=["start_time DESC"], max_results=2)[["metrics.training_accuracy_score", "params.estimator"]]
   
   fig, ax = plt.subplots()
   ax.bar(df_results["params.estimator"], df_results["metrics.training_accuracy_score"])
   ax.set_xlabel("Estimator")
   ax.set_ylabel("Accuracy")
   ax.set_title("Accuracy by Estimator")
   for i, v in enumerate(df_results["metrics.training_accuracy_score"]):
       ax.text(i, v, str(round(v, 2)), ha='center', va='bottom', fontweight='bold')
   plt.show()
    ```

1. The output should resemble the following image:

    ![Screenshot of the plotted evaluation metrics.](./Images/plotted-metrics.png)

### Task 6: Explore your experiments

In this task, you will explore your experiments in Microsoft Fabric, which tracks all your training activities. The platform allows for visual exploration of these experiments, enabling you to analyze and compare results effectively.

1. Navigate to your **Workspace (1)**, you will see the **experiment-churn (2)** Experiment created.

   ![](./Images/Pg6-ExpChurn-S1.png)

1. Select the `experiment-churn` experiment to open it.

    > **Tip:**
    > If you don't see any logged experiment runs, refresh the page.

1. Select the **View (1)** tab.

1. Select **Run list (2)**. 

1. Select the **two latest runs (3)** by checking each box.
    As a result, your two last runs will be compared to each other in the **Performance** pane. By default, the metrics are plotted by run name. 

1. Select the **&#128393;** **(Edit) (4)** button of the graph visualizing the accuracy for each run. 

   ![](./Images/Pg6-ExpChurn-S6.png)

1. Change the **visualization type** to `bar`. 

1. Change the **X-axis** to `estimator`. 

1. Select **Replace** and explore the new graph.

By plotting the accuracy per logged estimator, you can review which algorithm resulted in a better model.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
      
   - If you receive an InProgress message, you can hit refresh to see the final status.
   - If you receive a success message, you can proceed to the next task.
   - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="9bc2b595-c7f5-4276-93dc-2293e35d87e2" />

### Task 7: Save the model

In this task, you will save the best-performing machine learning model after comparing the results from various experiment runs. This saved model can then be utilized to generate predictions for future data analysis.

1. In the experiment overview, ensure the **View** tab is selected.

1. Select **Run details**. 

1. Scroll right to see the Save as model option. Select **Save** in the **Save run as an ML model** box.

   ![Empty data pipeline.](./Images/36.png)

1. Select **Create a new model** in the newly opened pop-up window.

1. Select the existing folder and name the model `model-churn`, and select **Save**. 

1. Select **View ML model** in the notification that appears at the top right of your screen when the model is created. You can also refresh the window. The saved model is linked under **Registered version**. 

>**Note**: That the model, the experiment, and the experiment run are linked, allowing you to review how the model is trained. 

### Task 8: Save the notebook and end the Spark session

In this task, you will save your notebook with a meaningful name to preserve your work after training and evaluating the models. Additionally, you will end the Spark session to free up resources and finalize your data processing environment.

1. Select the notebook that you created. In the notebook menu bar.
  ![](./Images/nb-1.png)

2. Click on the ⚙️ **Settings** icon (1) to view the notebook settings, and Set the **Name** of the notebook to **Train and compare models notebook (2)**, and then close the settings pane.
    ![](./Images/nb-2.png)

1. On the notebook menu, select &#9645;**Stop session** to end the Spark session.

   >**Note:** If you can't see the **Stop Session** option, it means the spark session has already ended.

### Summary

In this exercise, you have created a notebook and trained a machine-learning model. You used Scikit-Learn to train the model and MLflow to track its performance.

### You have successfully completed the lab,. Click on Next >> to procced with next exercise.
