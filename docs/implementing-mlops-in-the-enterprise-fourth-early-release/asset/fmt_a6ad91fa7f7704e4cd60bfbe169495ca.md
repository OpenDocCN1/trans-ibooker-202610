
The pipeline progress can be viewed interactively in the client or notebook. When the pipeline ends it generates a summary of the results (see [Figure 7-14](#ch07.xhtml#fig-8-mlrun-tune-report)).

![](assets/8-mlrun-tune-report.png)

However, the UI provides a more interactive and rich experience (see [Figure 7-15](#ch07.xhtml#fig-8-workflow-train)).

![](assets/8-workflow-train.png)

One of the unique features of `MLRun` is the ability to seamlessly distribute the workload across multiple systems and GPUs (by orchestrating the underline Kubernetes, containers, and MPI resources). For example, in [Figure 7-16](#ch07.xhtml#fig-8-data-vs-model-distribution2) (taken from `MLRun` UI), we can see that `MLRun` distributed the tuning task across 16 workers and GPUs.

![](assets/8-workers-training.png)

When the pipeline ends, the model gets registered automatically and can be used in the application pipeline.

#### Note
It is advised to add deployment tests that try to deploy the application and model pipeline into a staging environment and run exhaustive tests on it to verify its reliability and performance.

### Application and Model Serving Pipeline
The application pipeline, especially in LLM use cases, consists of many steps to accept and process the request. These include steps for data enrichment and pre-processing, prompt engineering, model prediction, application control flow, safety, risk control, data post-processing and formatting, monitoring, and more. Therefore, building, deploying, and scaling the application pipeline is challenging. In addition, addressing operational considerations such as performance, security, availability versioning, and rolling upgrades can drain significant engineering resources and take time.

`MLRun` framework uses elastic serverless functions to automate the build and deployment of application pipelines by allowing the composition of a graph (DAG) made of multiple custom and built-in steps and translating it automatically to a distributed pipeline running over microservices.

This tutorial implements the application pipeline using `MLRun` Serving Graph made of four steps (see [Figure 7-17](#ch07.xhtml#fig-8-serving-graph)):

1. Data pre-processing (`preprocess`): Fit the user prompt into the model prompt structure (“Subject - Content”).
2. LLM prediction (`LLMModelServer`): Serve our trained model and perform inferences to generate answers.
3. Post-processing (`postprocess`): Check if the model generated reliable answers and format the output.
4. Toxicity filter (`ToxicityClassifierModelServer`): Use the Hugging Face Evaluate package model and perform inferences to catch toxic prompt or responses and respond with a proper answer instead.

![](assets/8-serving-graph.png)

Steps 2 and 4 are generic and can be used in multiple applications, while steps 1 and 3 are specific. You can see the full code implementation of the steps in the GIT repository (in `src/serving.py`). Note that a real application will likely include more steps and logic.

The [Example 7-8](#ch07.xhtml#code-8-set-serving-graph) code defines the graph by specifying the function or class per step, step parameters, and graph relations. The `plot()` method allows you to visualize the graph topology.

#### Example 7-8. Define the serving graph topology
```
# Set the topology and get the graph object:
graph = serving_function.set_topology("flow", engine="async")

# Add the steps:
graph.to(handler="preprocess", name="preprocess") \
     .to("LLMModelServer",
         name="mlopspedia",
         model_class="GPT2LMHeadModel",
         tokenizer_class="GPT2Tokenizer",
         model_path=project.get_artifact_uri("gpt2-medium-mlrun"),
         use_deepspeed=True) \
     .to(handler="postprocess", name="postprocess") \
     .to("ToxicityClassifierModelServer",
         name="toxicity-classifier",
         threshold=0.7).respond()

# Plot to graph:
serving_function.plot(rankdir='LR')
```

![](assets/8-serving-pipe-graph.png)

Once the real-time application graph is defined, you can debug it locally, save it, or deploy it to the cluster with a single API call or SDK command, as shown here:

```
# Deploy the serving function:
deployment = mlrun.deploy_function("serving")
```

To test the deployed application, you can use the `invoke()` method and specify the API parameters (the text question and model parameters in this case). The first example (see [Example 7-9](#ch07.xhtml#code-8-test-model1)) demonstrates a simple question and answer.

#### Example 7-9. Test the application pipeline
```
generate_kwargs = {"max_length": 150, "temperature": 0.9, "top_p": 0.5,
                   "top_k": 25, "repetition_penalty": 1.0}

response = serving_function.invoke(
    path='/predict',
    body={"prompt": "What is machine learning?", **generate_kwargs}
)
print(response["outputs"])
```

```
MLRun is an open source MLOps orchestration framework that streamlines the automation
of machine learning projects. MLRun empowers data scientists, data engineers and
DevOps teams to develop, deploy and manage machine learning applications faster and
more accurately.
MLRun integrates with popular tools such as Jupyter, PyCharm, Spark, Etc., and
provides a robust set of built-in orchestration and monitoring capabilities.
```

[Example 7-10](#ch07.xhtml#code-8-test-model2) sends a toxic question, and the model responds with a controlled answer.

#### Example 7-10. Try the toxic language filter
```
response = serving_function.invoke(
    path='/predict',
    body={"prompt": "You are stupid!", **generate_kwargs}
)
print(response["outputs"])
```

```
This bot does not respond to toxicity.
```

### Adding a Web Interface
You can use `Gradio` to rapidly create a UI to demo the behavior of your chat application pipeline. [Example 7-11](#ch07.xhtml#code-8-build-gradio) defines a chat window and control widgets.

#### Example 7-11. Create the GRadio interactive UI
