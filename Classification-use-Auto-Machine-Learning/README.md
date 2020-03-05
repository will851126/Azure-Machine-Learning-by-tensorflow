# Automated Machine Learning on Azure NotebookVM

## Scenario 

In this Lab , we use the UCI Bank Marketing dataset to showcase how can use [AutoML on Azure Notebook VM](https://docs.microsoft.com/en-us/azure/machine-learning/service/concept-automated-ml) for a classification problem and deploy it to an [Azure Container Instance (ACI)](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-overview). The classification goal is to **predict if the client will subscribe to a term deposit with the bank**.

<p align="center">
    <img src="images/023.png" width="70%" height="80%">

## Prerequisite

* Download **bank-market-classification folder**  which is in this repository in your computer folder 

* Include three **datasets** and one **.Ipynb** file




### Create a workspace
An Azure Machine Learning workspace is a foundational resource in the cloud that you use to **experiment, train, and deploy machine learning models**.

1. Sign in to the Azure portal ,and in the upper-left corner of Azure portal, select **Create a resource**.
<p align="center">
    <img src="images/001.png" width="80%" height="80%">

2. Use the search bar to find **Machine Learning service workspace**,and click **Select**.
<p align="center">
    <img src="images/002.png" width="80%" height="80%">

3. In the Machine Learning service workspace pane, select **Create Azure machine learning** to begin.

<p align="center">
    <img src="images/003.png" width="80%" height="80%">

4. For Workspace name, type a **Unique Name**

5. For Subscripition, Select **your Subscripition**

6. For Resource group , Select **your Resource group** ,if not have any, Please click **Create New**


7. For Location , Select **East US**  

8. For Workspace edition, Select **Basic**

9. After you are finished configuring the workspace, select **Review+Create**.
<p align="center">
    <img src="images/004.png" width="80%" height="80%">

10. To view the new workspace, select **Go to resource**.


## Create NotebookVM

 1. In the upper-left corner of Azure Machine , Select **Compute**, and click **NEW** in Notebook VMs.

<p align="center">
    <img src="images/005.png" width="30%" height="80%">

<p align="center">
    <img src="images/007.png" width="80%" height="80%">

2-1. In Notebook VM , type :

*  Notebook VM name : `your unique name`
*  VM type : Select **STANDARD_D1**

2-2  Click **Create**




<p align="center">
    <img src="images/008.png" width="80%" height="80%">

 * Wait about 5 minutes ,and status to **Running**

<p align="center">
    <img src="images/009.png" width="80%" height="80%">

 3. Click **jupyter** and into jupyter Environment page

  <p align="center">
    <img src="images/010.png" width="80%" height="80%">



4. Authenticate Account
    
    * Select your Azure account

<p align="center">
    <img src="images/058.png" width="50%" height="80%">



<p align="center">
    <img src="images/011.png" width="80%" height="80%">

5. Upload **bank-market-classification.ipynb**

   
    * Click **upload**, and Select **bank-market-classification.ipyn** from  your computer folder

<p align="center">
    <img src="images/012.png" width="80%" height="80%">



### Setup

As part of the setup  have already created an Azure ML Workspace object. For AutoML  will need to create an Experiment object, which is a named object in a Workspace used to run experiments.

1. import package what we use
        
       


<p align="center">
    <img src="images/013.png" width="80%" height="80%">



 2. Use a web browser to open the page  

<p align="center">
    <img src="images/014.png" width="80%" height="80%">

3. Enter the code to  authenticate

<p align="center">
    <img src="images/015.png" width="50%" height="80%">


4. Select **your Azure Account**

<p align="center">
    <img src="images/016.png" width="50%" height="80%">


5. Return Jupyter notebook and show workspace infromation

<p align="center">
    <img src="images/018.png" width="80%" height="80%">


### Create  existing AmlCompute


we need to **create** a compute target for  AutoML 

A **compute target** is a designated compute resource/environment where you run your training script or host your service deployment




1. Creat **compute target** and **cluster name**

            


<p align="center">
    <img src="images/019.png" width="80%" height="80%">


### Upload Data into Blob storage

1. Sign in to the Azure portal ,and in the upper-left corner of Azure portal, select **Storage Accounts**

<p align="center">
    <img src="images/059.png" width="30%" height="0%">

2. Select your **Storage Account**

<p align="center">
    <img src="images/063.png" width="80%" height="80%">

3. Click **Container**

<p align="center">
    <img src="images/060.png" width="80%" height="80%">

4. Click **add Container**

<p align="center">
    <img src="images/061.png" width="80%" height="80%">

* Name: **your unique name**
* Public access level : **Container**

<p align="center">
    <img src="images/062.png" width="80%" height="80%">

5. Click **OK**

6. Enter  already Created **Storage Container**

7. Click **Upload**

<p align="center">
    <img src="images/064.png" width="80%" height="80%">


8. Upload three datasets to **Blob storage** from your Computer folder

<p align="center">
    <img src="images/065.png" width="80%" height="80%">


9. Click**Upload**

10. Copy data URL for three files 


<p align="center">
    <img src="images/066.png" width="80%" height="80%">








### Load Data

1. Train datasets , and paste train datasets URL   
            

<p align="center">
    <img src="images/020.png" width="80%" height="80%">


2. Validation Data , and Paste Validation Datasets URL

        
    <p align="center">
    <img src="images/021.png" width="80%" height="80%">

3.Test Data, and Paste test Datasets URL
    

<p align="center">
    <img src="images/022.png" width="80%" height="80%">


4. Add missing values in 75% of the lines.

<p align="center">
    <img src="images/067.png" width="80%" height="80%">


5. Save the train data to a csv to be uploaded to the datastore

<p align="center">
    <img src="images/068.png" width="80%" height="80%">

### Automatically train a model


1. Define settings for the experiment run. Attach your training data to the configuration, and modify settings that control the training process.


2. After submitting the experiment, the process iterates through different machine learning algorithms and hyperparameter settings, adhering to your defined constraints.





1.1 Experiment_timeout_minutes : Maximum amount of time in minutes that all iterations combined can take before the experiment terminates

1.2  Enable_early_stopping : Flag to enble early termination if the score is not improving in the short term.

1.3  Iteration_timeout_minutes: Time limit in minutes for each iteration. Reduce this value to decrease total runtime.

1.4 Max_concurrent_iterations: Maximum number of iterations that would be executed in parallel on an amlcompute cluster

1.5 Max_cores_per_iteration: Maximum number of threads to use for a given training iteration. 

1.6 primary_metric: Metric that you want to optimize. The best-fit model will be chosen based on this metric.

1.7  featurization: By using **auto**, the experiment can preprocess the input data

1.8 verbosity: Controls the level of logging


<p align="center">
    <img src="images/069.png" width="80%" height="80%">
  

2.1 Task: **classification**, **regression**, or **forecasting** depending on what kind of ML problem to solve

2.2 Debug_log : Log file to write debug information to

2.3 experiment_exit_score : arget score for experiment. Experiment will terminate after this score is reached

2.4   blacklist_models: List of algorithms to **ignore** for an experiment

2.5   enable_onnx_compatible_models: Flag to enable/disable enforcing the onnx compatible models

2.6  training_data : DataFrame or Dataset or DatasetDefinition or TabularDataset

<p align="center">
    <img src="images/025.png" width="80%" height="80%">

Call the submit method on the experiment object and pass the run configuration. Execution of local runs is synchronous. Depending on the data and the number of iterations

    

<p align="center">
    <img src="images/026.png" width="80%" height="80%">


3. Wait for the remote run to complete

       

<p align="center">
    <img src="images/027.png" width="80%" height="80%">


4. Select the best model from your iterations. The `get_output` function returns the best run and the fitted model for the last fit invocation

<p align="center">
    <img src="images/070.png" width="80%" height="80%">


### View updated featurization summary

1.  new featurization

<p align="center">
    <img src="images/028.png" width="80%" height="80%">







### Result
Here’s an example of an AutoML run that shows the **iterations explored and the scores**
    
<p align="center">
    <img src="images/071.png" width="80%" height="80%">


<p align="center">
    <img src="images/031.png" width="80%" height="80%">


<p align="center">
    <img src="images/032.png" width="80%" height="80%">


2. Click see the other model detail in Azure Machine Learning studio, and Select **Model**

<p align="center">
    <img src="images/033.png" width="80%" height="80%">

<p align="center">
    <img src="images/034.png" width="80%" height="80%">


### Retrieve the Best Model's explanation

Retrieve the explanation from the best_run which includes explanations for engineered features and raw features. Make sure that the run for generating explanations for the best model is completed.

1. Wait for the best model explanation run to complete

    

2. Get the best run object 
<p align="center">
    <img src="images/035.png" width="80%" height="80%">

### Retrieve the Best **ONNX** Model

 Models from many frameworks **including TensorFlow, PyTorch, SciKit-Learn, Keras, Chainer, MXNet, and MATLAB** can be exported or converted to the standard ONNX format. 
 
 Once the models are in the ONNX format, can be run on a variety of platforms and devices.

1. install onnxruntime

<p align="center">
    <img src="images/073.png" width="80%" height="80%">
    

2.  Save the best ONNX model
    
<p align="center">
    <img src="images/074.png" width="80%" height="80%">

### Prepare deploy the model on Azure Container Instance


1. Get the best run object 

2. Create deployment enviroment

<p align="center">
    <img src="images/075.png" width="80%" height="80%">
       
3. Register the Fitted Model for Deployment

<p align="center">
    <img src="images/076.png" width="80%" height="80%">
    

* This will be written to the script file later in the notebook


4. See already  Register model  in Azure Machine Learning studio

    * Select **model** moudle in the upper-left corner 

<p align="center">
    <img src="images/036.png" width="30%" height="30%">

* click model nane in  Model List 

<p align="center">
    <img src="images/037.png" width="80%" height="80%">


* Can see  model detailed

<p align="center">
    <img src="images/038.png" width="80%" height="80%">


4. Return Jupyer notebook, and deploy model on Azure Container Instance



<p align="center">
    <img src="images/039.png" width="80%" height="80%">


5. Sign in to the Azure portal ,and in the upper-left corner of Azure portal, select **Container Instance**.


<p align="center">
    <img src="images/040.png" width="80%" height="80%">



6. Click **Container Instances Name**

<p align="center">
    <img src="images/042.png" width="80%" height="80%">


7. In the upper-left corner of Azure container Instances, select **Container**

<p align="center">
    <img src="images/041.png" width="30%" height="30%">


8. Monitor and see Containers state

<p align="center">
    <img src="images/043.png" width="80%" height="80%">


9. Test Model

    Run the test data through the trained model to get the predicted values

    
    <p align="center">
    <img src="images/044.png" width="80%" height="80%">
    

    9-1. transform data type
     
    9-2.  Calculate metrics for the prediction 

           

<p align="center">
    <img src="images/077.png" width="80%" height="80%">


9-3.   Visulize Predict values 


<p align="center">
    <img src="images/078.png" width="80%" height="80%">
    
    
<p align="center">
    <img src="images/046.png" width="80%" height="80%">


###  Conclusion

Congratulations! You now have learned how to:

1. Create an experiment using an existing workspace.
Configure AutoML using AutoMLConfig.

2. Train the model using local compute with ONNX compatible config on.
Explore the results, featurization transparency options and save the ONNX model

3. Inference with the ONNX model.

4. Register the model.

5. Create a container image.

6. Create an Azure Container Instance (ACI) service.

7. Test the ACI service.


### Clean up Resource

In the resource group,**Delete resource**

1. Azure Machine Learning Service

2. Azure Container instance

3. other Serice 
















