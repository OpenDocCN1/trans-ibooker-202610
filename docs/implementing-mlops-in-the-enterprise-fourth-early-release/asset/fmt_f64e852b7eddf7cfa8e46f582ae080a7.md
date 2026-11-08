
# Advanced Serving and Application Pipelines

## Example 6-7. Deploy a model using Seldon Core (source: Seldon Core)

Step 1: Create a YAML file describing a single (reusable) model server:
```
apiVersion: machinelearning.seldon.io/v1
kind: SeldonDeployment
metadata:
  name: sklearn
spec:
  name: iris
  protocol: v2
  predictors:
  - graph:
      children: []
      implementation: SKLEARN_SERVER
      modelUri: gs://seldon-models/sklearn/iris-0.23.2/lr_model
      name: classifier
    name: default
    replicas: 1
```
Step 2: Deploy the model to the Kubernetes cluster:
```
> kubectl apply -f resources/iris-sklearn-v2.yaml
seldondeployment.machinelearning.seldon.io/sklearn created
```
Step 3: Test the new endpoint:
```
import requests

inference_request = {
    "inputs": [
        {"name": "predict", "shape": [1, 4], "datatype": "FP32",
         "data": [[1, 2, 3, 4]]}
    ]
}

endpoint = "http://localhost:8003/seldon/seldon/sklearn/v2/models/infer"
response = requests.post(endpoint, json=inference_request)

print(response.json())
```
Seldon Core supports various model deployment options, multi-stage pipelines, and a standard (v2) protocol. However, it is more DevOps-oriented and requires manual configuration and an understanding of the Kubernetes API.

## MLRun Serving

The MLRun MLOps framework includes advanced model and application serving functionality. MLRun serving allows users to define multi-stage real-time pipelines and quickly deploy them to production with the help of [Nuclio](https://nuclio.io/), a real-time serverless engine. Nuclio is a high-performance, elastic, open-source “serverless” framework focused on data, I/O, and compute-intensive workloads. It supports advanced functionality and many triggering options (such as HTTP, cron, Kafka, Kinesis, and others). MLRun and Nuclio’s serverless architecture converts the code and high-level definitions into a hardened, high-performance, self-healing, and auto-scaling service with built-in monitoring and observability.

MLRun serving supports two topology options:
*   `Router`: A basic serving of one or more models (the default option).
*   `Flow`: A multi-stage pipeline (DAG - directed acyclic graph) with built-in or custom steps (for example, API integrations, data enrichment and processing, model serving, routing, storage, and so on).

MLRun contains built-in serving classes for the major ML/DL frameworks ([Scikit-Learn](https://scikit-learn.org/), [TensorFlow](https://www.tensorflow.org/), [ONNX](https://onnx.ai/), [XGBoost](https://xgboost.ai/), [LightGBM](https://github.com/microsoft/LightGBM), [PyTorch](https://pytorch.org/), and [Hugging Face](https://huggingface.co/)) and supports a standard serving protocol (like KServe, Seldon, and Triton). In addition, MLRun provides a few container images with the required ML/DL packages pre-installed, or you can choose a base image and additional package requirements, which will automatically build the desired image for you.

In MLRun, the first step is to define a function object that specifies the code, packages, resources, triggers, and so on. Then you define the serving topology (graph). Once the serving function is fully defined, you can simulate it locally or deploy it to a cluster using a single API call.

[Example 6-8](#ch06.xhtml#mlrun-serving-basic) demonstrates the usage of MLRun serving. First, define a function using a standard image, add a model, and then simulate and debug the serving pipeline locally with test data. Finally, deploy the function to the cluster and test the live endpoint (using `invoke()`).

### Example 6-8. Define and deploy a basic MLRun serving topology
```
serving_fn = mlrun.new_function("serving", image="mlrun/mlrun",
                                kind="serving", requirements=[])

# Add a model object or file (can be in S3, GCS, local file, etc.)
serving_fn.add_model(
    "my-model",
    model_path=model_uri,
    class_name="mlrun.frameworks.sklearn.SklearnModelServer")

# Create a mock server (simulator) and test/debug the endpoint
server = serving_fn.to_mock_server()
sample = {"inputs": [[5.1, 3.5, 1.4, 0.2], [7.7, 3.8, 6.7, 2.2]]}
server.test(path=f"/v2/models/my-model/infer", body=sample)

# Result:
{'id': '2b2e1703f98846b386965ce834a6c4ab',
 'model_name': 'my-model',
 'outputs': [0, 2]}

# Deploy the serving function to the cluster
project.deploy_function(serving_fn)
```
```
> 2023-02-14 12:29:12,008 [info] Starting remote function deploy
> 2023-02-14 12:29:12  (info) Deploying function
> 2023-02-14 12:29:12  (info) Building
> 2023-02-14 12:29:12  (info) Staging files and preparing base images
> 2023-02-14 12:29:12  (info) Building processor image
> 2023-02-14 12:29:57  (info) Build complete
> 2023-02-14 12:30:05  (info) Function deploy complete
> 2023-02-14 12:30:05,918 [info] successfully deployed function
```
```
# Send prediction request to the live endpoint
serving_fn.invoke(path=f"/v2/models/my-model/infer", body=sample)
```
The real power of MLRun serving graphs is the ability to develop and deploy complex distributed AI applications rapidly while ensuring maximum performance, scalability, availability, and security. [Example 6-9](#ch06.xhtml#mlrun-serving-nlp) shows an example of a multi-stage NLP application with data pre/post-processing. You can extend the serving graphs to include branching and parallelism. You can also add advanced data processing steps, model ensembles, exception handling, custom monitoring, and more.

### Example 6-9. Define and deploy a multi-stage serving graph topology
```
# Create an MLRun serving function from custom code
serving_function = mlrun.code_to_function(
    filename="src/serving.py",
    kind="serving",
    image="mlrun/mlrun",
    requirements=[],
)

# Set the serving topology
graph = serving_function.set_topology("flow", engine="async")

# Define a 3 step graph (preprocess -> hugging face model -> postprocess)
# the custom preprocess and postprocess functions are in serving.py
# while the HuggingFaceModelServer is a built-in MLRun class
graph.to(handler="preprocess", name="preprocess")\
     .to(mlrun.frameworks.huggingface.HuggingFaceModelServer(
              name="sentiment-analysis",
              task="sentiment-analysis",
              model_name="distilbert-base-uncased",
              model_class="AutoModelForSequenceClassification",
              tokenizer_name="distilbert-base-uncased",
              tokenizer_class="AutoTokenizer"))\
     .to(handler="postprocess", name="postprocess").respond()

# Plot to graph:
serving_function.plot(rankdir='LR')
```
![](assets/mlrun-huggingface-graph.png)
```
# Deploy the pipeline
project.deploy_function(serving_function)

# Send a text request and get the sentiment results
response = serving_function.invoke(path='/predict', body="good morning")
print(response)

# Result:
['The sentiment is POSITIVE', 'The prediction score is 0.7876932144165039']
```
MLRun serving provides a rich user interface that natively integrates with the platform’s other elements (see [Figure 6-5](#ch06.xhtml#mlrun-model-endpoints-ui)).

![](assets/mlrun-model-endpoints-ui.png)

In summary, MLRun serving extends the notion of model serving to rapid delivery of application pipelines and accelerating the deployment of AI applications. In addition, its serverless architecture reduces infrastructure costs and engineering overhead and enables continuous operations.
