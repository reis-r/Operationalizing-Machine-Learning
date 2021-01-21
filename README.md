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

## Screen Recording

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
