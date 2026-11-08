
## Data Processing

3.  *Bias detection and mitigation*: Measure the bias along specific demographic attributes and balance it in the training set or alternatively remove identifying details.
4.  *Privacy protection*: Remove any Personal Identifiable Information (PII), including names, phone numbers, credit card numbers, ids, email addresses, and so on. Check for potential leakage of private documents or code.
5.  *De-duplication*: Remove duplicate text segments from the dataset to improve model accuracy and reduce hallucinations.
6.  *Formatting and tagging*: Organize and label the data in a contextual format to improve model reliability. For example, break a document into paragraphs or sections with clear headers (covering the content of that section). Another example is organizing the text as questions and answers or text and class pairs.
7.  *Vector embeddings and indexing*: Convert paragraphs or images into a numerical vector representation to capture the semantic and syntactic relationships and index the vectors to quickly identify similarities.
8.  *Tokenization*: Break down text sequences into smaller units called tokens. These tokens can be words, subwords, or characters, depending on the chosen tokenization strategy. Tokenization enables text transformation into a format that machines can process and analyze.

[Figure 7-9](#ch07.xhtml#fig-8-nlp-data-pipeline) illustrates a data processing pipeline in an NLP (natural language processing) application.

![](assets/8-nlp-data-pipeline.png)

Data controls can be implemented in one or more of the following tasks:
1.  *Data pre-processing*: Data cleaning and preparation before the data is sent to the model tuning or vector indexing process.
2.  *Model and application testing*: Testing the model with different input prompts and verifying that the outputs and model behavior conforms with the expected standards and quality.
3.  *Processing of user requests*: Checking the incoming user requests and prompts (in real-time) before they are sent to the model.
4.  *Validating model responses*: Verifying the compliance and quality of the results in real-time before they are returned to the user, and blocking them or providing an alternative response.
5.  *Monitoring the application logs*: Sending all the requests (prompts) and responses to a monitoring system that checks the compliance, accuracy, and quality of the results.

There are advantages and disadvantages to each approach, it is always better to address the problem in the earliest stage (data pre-processing), but in many cases, you cannot implement a solution on the application data path due to performance or complexity reasons and the tracking is done in the monitoring system and its feedback is used to further tune the system.

Another significant element in ensuring high quality and reliability is adding user feedback *(humans in the loop)*. This means sampling the results in the production data and verifying by humans that they meet the expected behavior. If they don’t, correcting them and re-training or tuning the model to fix that behavior.

## MLOps Pipelines For Efficiently Using and Customizing LLMs

Earlier in this chapter, we described two methods to customize LLMs for specific data and applications, prompt engineering and fine-tuning. In many applications, we will use both to maximize performance and reliability. Data preparation and validation are crucial parts of the process in both cases.

In fine-tuning, you ingest and prepare the application data and feedback data, run a transfer learning job to tune the base foundation model and extensive testing, and finally deploy the newly built model into the staging or production environment (see [Figure 7-10](#ch07.xhtml#fig-8-llm-tuning-pipe)).

![](assets/8-llm-tuning-pipe.png)

The pipeline can run anytime you have new data or enough feedback data (generated through human labeling). In addition, since the target model is large, it requires distributed training (tuning) and validation over multiple systems and GPUs, which can be done with frameworks such as Horovod/MPI or Ray.

When you implement prompt engineering and need to feed the prompt with reference documents or context, the context should be prepared and indexed for efficient retrieval. For example, using embeddings and vector or keyword databases (see [Figure 7-11](#ch07.xhtml#fig-8-llm-indexing-pipe)). Furthermore, the data fed and indexed in the database must be thoroughly cleaned and prepared to maximize the response quality and avoid risks.

![](assets/8-llm-indexing-pipe.png)

Multiple new frameworks target data processing and indexing for language processing tasks. For example, [LangChain](https://python.langchain.com/en/latest/index.xhtml), [Llama Index](https://gpt-index.readthedocs.io/en/latest/), [Spacy](https://spacy.io/), and [Unstructured](https://unstructured-io.github.io/unstructured/index.xhtml) to name a few.

The pre-indexed data and tuned model are used in the interactive or real-time pipeline, which intercepts the user requests and responds with the expected answer. [Figure 7-12](#ch07.xhtml#fig-8-llm-serving-pipe) illustrates the real-time pipeline. The first step is to receive and process the request, followed by data enrichment, prompt engineering, LLM prediction, post-processing, and returning a response to the user. In addition, the incoming data and responses are sent to the monitoring system, stored, and used to identify drift, performance problems, or risks. Finally, portions of the monitored data can be sent to human labeling and used for re-tuning the model (RLHF).

![](assets/8-llm-serving-pipe.png)

The real-time pipeline introduces several challenges:
*   Relative complexity and integration of disparate elements (data processing and enrichment, application logic, model prediction, monitoring, human feedback, and so on).
*   Partitioning the large model to multiple GPU devices and systems.
*   Model performance (LLMs are notoriously slow and may require multiple calls for a single user flow).
*   Real-time request and response validation to avoid risks.
*   Continuous deployment and rolling upgrades.

Elastic serverless frameworks and automation can reduce the complexity.

## Application Example: Fine-Tuning an LLM Model

The following tutorial provides an example of building and operationalizing an intelligent question-and-answering application using a fine-tuned large language model.

The tutorial contains two main parts:
1.  Data preparation and tuning pipeline
2.  Real-time application and model serving pipeline

The application is tuning an LLM with the content of [Iguazio’s MLOps blog](https://www.iguazio.com/blog/) and it answers MLOps-related questions. It uses [MLRun](https://www.mlrun.org/) to rapidly build, run, and monitor the two pipelines.

The complete source code can be found at: [https://github.com/mlrun/demo-llm-tuning](https://github.com/mlrun/demo-llm-tuning). Open the `tutorial.ipynb` file to see the tutorial flow.

### Data Preparation and Tuning

In this tutorial, the open-source LLM model is downloaded from [Hugging Face](https://huggingface.co/) and fine-tuned with the MLOps dataset generated from the blog pages. The first steps of the pipeline are reading the pages from the internet (blog), converting them to cleaned-up text, and pre-processing them to maximize the model’s performance. The following step uses Horovod, MPI, and [DeepSpeed](https://www.microsoft.com/en-us/research/project/deepspeed/) to scale the tuning process across multiple systems and GPUs. The final step evaluates the model. See the flow described in [Figure 7-13](#ch07.xhtml#fig-8-training-pipeline).

![](assets/8-training-pipeline.png)

**Note**
Many more tests will likely be added in a real-world application.

But first, let’s verify that the base LLM only knows a little about MLOps by loading it and asking a question. See the code in [Example 7-1](#ch07.xhtml#code-8-test-base-llm).
