
# Operationalizing Machine Learning with Microsoft Azure

We will use Azure to operationalize a machine learning model with Azure Machine Learning + Python SDK. The main goals are the following:
- train a ML model in Azure ML (using AutoML),
- deploy the model (with Azure Container Instances, ACI),
- cosume the model (through REST endpoints)

A detailed list of steps consist of the following:
- Authentication
- Automated ML Experiment
- Deploy the best model
- Enable logging
- Swagger Documentation
- Consume model endpoints
- Create and publish a pipeline
- Documentation (README.md file)


## Architectural Diagram

![Steps](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step0/steps.png)

First step is configuring authentication, then we create an AutoML experiment which consist of: 
- importing the dataset,
- configuring the run and 
- setting up the environmet for AutoML run.
After the AutoML run is completed, we deploy the best model inside an Azure Container Instance. Then we enable the application insights using the script file *logs.py*. Next step is creating swagger documentation to build the API documentation easily (we do this with the file *swagger.json*). Next step is to consume the model via *endpoint.py* and benchmarking is done via *benchmark.sh*. Finally the pipeline is created, published and consumed.

## Key Steps

### Step 1 - Authentication (Skipped)
Authentication is crucial for the continuous flow of operations. Continuous Integration and Delivery system (CI/CD) rely on uninterrupted flows. When authentication is not set properly, it requires human interaction and thus, the flow is interrupted. An ideal scenario is that the system doesn't stop waiting for a user to input a password. So whenever possible, it's good to use authentication with automation. Authentication types 
1. Key Based 
2. Token based 
3. Interactive

Udacity Classroom - "If you are using the lab Udacity provided to you, you can skip this step since you are not authorized to create a security principal". So, this step is skipped in Azure.

### Step 2 - Automated ML experiment
1. Create a new Automated ML run
![Step2.1](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step2-automated-ml-experiment/1-create-a-new-automated-ml-run.jpg)

2 - Select dataset
![Step2.2](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step2-automated-ml-experiment/2-select-dataset.jpg)

3 - Create new experiment
![Steps2.3](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step2-automated-ml-experiment/3-create-new-experiment.jpg)

4 - Configure new compute cluster
![Steps2.4](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step2-automated-ml-experiment/4-configure-new-compute-cluster.jpg)

5 - Run experiment
![Steps2.5](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step2-automated-ml-experiment/5-run-experiment.jpg)

6 - Completed experiment
![Steps2.6](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step2-automated-ml-experiment/6-completed-experiment.jpg)

7 - Best model

![Steps2.7](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step2-automated-ml-experiment/7-best-model.jpg)


### Step 3 - Deploy the best model
1 - Select best model
![Steps3.1](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step3-deploy-the-best-model/1-select-best-model.jpg)

2 - Deploy best model
![Steps3.2](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step3-deploy-the-best-model/2-deploy-best-model.jpg)


### Step 4 - Enable logging

1 - Modifying the logs.py file:
![Steps4.1](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step4-enable-logging/1-modyfing-logs-script.jpg)

2 - Running logs.py to enable app insights:
![Steps4.2](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step4-enable-logging/2-running-logs-file.jpg)

3 - Check the app insights is enable:
![Steps4.2.1](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step4-enable-logging/3-check-app-insight-enabled.jpg)
![Steps4.2.2](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step4-enable-logging/4-check-logs.jpg)


### Step 5 - Swagger Documentation
Swagger is a tool that helps build, document, and consume RESTful web services like the ones you are deploying in Azure ML Studio. It further explains what types of HTTP requests that an API can consume, like POST and GET. We will use swagger.json file that to create a web site that documents the HTTP endpoint for our deployed model.

1 - Run swagger.py to create a container (using docker) to use swagger:
![Steps5.1.1](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step5-swagger-documentation/1-run-swagger-file.jpg)

![Steps5.1.2](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step5-swagger-documentation/2-check-i-swagger-running.jpg)

![Steps5.1.3](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step5-swagger-documentation/3-check-ii-swagger-running.jpg)

2 - Run serve.py to create a container (using docker) to use swagger:
![Steps5.2.1](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step5-swagger-documentation/4-run-server-file.jpg)

![Steps5.2.2](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step5-swagger-documentation/5-check-server-running.jpg)


### Step 6 - Consume model endpoints
You can consume a deployed service via an HTTP API. An HTTP API is a URL that is exposed over the network so that interaction with a trained model can happen via HTTP requests. Users can initiate an input request, usually via an HTTP POST request. HTTP POST is a request method that is used to submit data. The HTTP GET is another commonly used request method. HTTP GET is used to retrieve information from a URL. The allowed requests methods and the different URLs exposed by Azure create a bi-directional flow of information. The APIs exposed by Azure ML will use JSON (JavaScript Object Notation) to accept data and submit responses. It served as a bridge language among different environments.

1 - In order to test the model deployed, we will fill the scoring uri and key values in the endpoint.py file:
![Steps6.1](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step6-consume-model-endpoints/1-modifying-uri-and-key.jpg)

![Steps6.2](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step6-consume-model-endpoints/2-run-endpoint-file.jpg)


### Step 7 - Create and publish a pipeline

In this final set of steps we will move to the notebook to create and publish the pipeline:

![Steps7.1](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step6-consume-model-endpoints/2-create-compute-instance.jpg)

![Steps7.2](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step6-consume-model-endpoints/2-run-automl.jpg)

![Steps7.3](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step6-consume-model-endpoints/3-run-completed.jpg)

![Steps7.4](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step6-consume-model-endpoints/4-pipeline-deployed.jpg)

![Steps7.5](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step6-consume-model-endpoints/5-pipeline-endpoint.jpg)

![Steps7.6](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step6-consume-model-endpoints/6-pipeline-gui.jpg)

![Steps7.7](https://github.com/MangelFdz/machine-learning-engineering-with-azure/blob/main/operationalizing-machine-learning/images/step6-consume-model-endpoints/7-pipeline-overview.jpg)



