# Operationalizing Machine Learning

This project was made as part of the Udacity Nanodegree on Machine Learning Engineering with Microsoft Azure. It consists on deploying a model in production, using monitoring tools to evaluate the deploy and building a deployment pipeline based on continuous integration (CI) and continuous delivery (CD) principles.

For the experiments, an AutoML model was trained based on the Bank Marketing dataset provided by Udacity. After the deployment of the trained AutoML model, we enable logging via Application Insights. After deployment, we benchmark the model endpoint and build the Swagger documentation for the HTTP API created. Then we consume the endpoint and build a pipeline endpoint for AutoML.

## Architectural Diagram
![Architectural diagram of the project](https://raw.githubusercontent.com/reis-r/nd00333_AZMLND_C2/master/architectural_diagram.png)

## Key Steps
### Registering the dataset
First, we need to register the dataset with Azure, this is a simple step that can be easily done using the Azure Machine Learning Studio.

![Registered Dataset](https://raw.githubusercontent.com/reis-r/nd00333_AZMLND_C2/master/screenshots/registered_datasets.PNG)

### Finding the best model with AutoML
First, we create an AutoML experiment for a classification task on the `y` attribute with exit criterion of 1 hour and concurrency of 5.

![Configure AutoML run](https://raw.githubusercontent.com/reis-r/nd00333_AZMLND_C2/master/screenshots/configure_run.PNG)

![AutoML configuration](https://raw.githubusercontent.com/reis-r/nd00333_AZMLND_C2/master/screenshots/automl_tweaks.PNG)

We used a `Standard_DS12_V2 Azure` compute cluster for the training. The training took about half an hour and the result was a *VotingEnsemble* model with 91.96% accuracy.

![Completed Experiment](https://raw.githubusercontent.com/reis-r/nd00333_AZMLND_C2/master/screenshots/completed_experiment.PNG)

### Deploying the best model
We deployed the best model produced in the last section to an Azure Container Instance (ACI). Authentication was enabled for better security and access control.

![Deploying the best model](https://raw.githubusercontent.com/reis-r/nd00333_AZMLND_C2/master/screenshots/deploying.PNG)

### Enable logging with Application Insights
It's important to keep logging of our applications, to fix eventual problems that might occur. For that, we enable Application Insights to our deployiment using the SDK. The full code snippet can be found [here](https://github.com/reis-r/nd00333_AZMLND_C2/blob/master/code/enable_Application_Insights.py).

![Script enabling Application Insights](https://raw.githubusercontent.com/reis-r/nd00333_AZMLND_C2/master/screenshots/enable_application_insights.PNG)

After enabling the logging of activities, we can access them via SDK using the [get_logs method form the Azure Webservice Class](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#get-logs-num-lines-5000--init-false-). The full code can be found at the [logs.py](https://github.com/reis-r/nd00333_AZMLND_C2/blob/master/code/logs.py) file in this Repository.

![Logs from the service](https://raw.githubusercontent.com/reis-r/nd00333_AZMLND_C2/master/screenshots/logs.PNG)

### Get Swagger Documentation up and running
The Azure service provides a swagger.json file with the API documentation for the service deployed. For this step, we created a Docker container for the Swagger service. After the configuration, Swagger shows a list of available API calls, what data they take and what they return. It's also possible to make calls using the Swagger interface. This is very useful for documenting the project as other services that will consume the model can know exactly how the data must be exchanged.

![Swagger running on localhost](https://raw.githubusercontent.com/reis-r/Operationalizing-Machine-Learning/master/screenshots/swagger.PNG)
![Swagger showing the API documentation](https://raw.githubusercontent.com/reis-r/Operationalizing-Machine-Learning/master/screenshots/swagger_methods.PNG)

The code for the Swagger deployment can be found [here](https://github.com/reis-r/Operationalizing-Machine-Learning/tree/master/code/swagger). It consists of a shell script file to start the docker container and a Python script that will expose our swagger.json to the docker container.

### Consume the model endpoint
Now that the application is deployed, it's time to consume it. For this a script called [endpoint.py](https://github.com/reis-r/Operationalizing-Machine-Learning/blob/master/code/endpoint.py) was used. It calls the deployed model with the Python requests library, using the authentication key and the data to be classified. As a result, we get back the predictions, as expected.

![The script endpoint.py running](https://raw.githubusercontent.com/reis-r/Operationalizing-Machine-Learning/master/screenshots/endpoint_python_script_working.PNG)

## Screen Recording

This is a quick screencast showing the trained model, the pipeline endpoint and testing the machine learning HTTP API created by Azure:

[![Screen recording](https://img.youtube.com/vi/DxCXNT5-WhM/0.jpg)](https://www.youtube.com/watch?v=DxCXNT5-WhM)

(click on the image to open the video)

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
