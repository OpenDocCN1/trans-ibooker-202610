
You probably want to avoid constantly staring at dashboards for model or data performance problems. Instead, you can define triggering policies and actions. For example, when a certain threshold is reached, a notification can alert the administrator or initiate an automated process for retraining a model or mitigating potential errors.

## Integrating with a CI/CD Service

CI/CD (continuous integration and deployment) is the standard approach for building and maintaining modern services in an agile process. Chapter 2 covered REFTO: CI/CD for ML and the differences from traditional CI/CD. The reference project uses `MLRun` to extend the GitHub Actions CI service to ML and data applications.

As a first step, you need to create scripts that will execute the different tests and verifications. The standard approach is to use a `Makefile`. In the `Makefile`, add commands to build, test and so on. Here are some examples for make commands (see the complete `Makefile` in the project directory):

```
.PHONY: lint
lint: fmt-check flake8 ## Run lint on the code

.PHONY: fmt-check
fmt-check: ## Format and check the code (using black and isort)
	@echo "Running black+isort fmt check..."
	$(PYTHON_INTERPRETER) -m black --check --diff src tests
	$(PYTHON_INTERPRETER) -m isort --check --diff src tests

.PHONY: flake8
flake8: ## Run flake8 lint
	@echo "Running flake8 lint..."
	$(PYTHON_INTERPRETER) -m flake8 src tests

.PHONY: test
test: clean ## Run tests
	$(PYTHON_INTERPRETER) -m pytest -v --capture=no --disable-warnings tests
```

With this `Makefile`, typing `make lint` or `make test`, will run the lint and pytest tests.

### Note

The CI/CD system (such as Jenkins or GitHub Actions) examines your project and searches for CI scripts in a reserved directory and executes them when the code is changed or merged.

In addition to static tests, you should automatically run the ML pipeline. However, since ML pipelines can consume significant computation, you may want the user to explicitly request running the ML pipeline. This can be done by typing a command in the Git pull request (for example `/run`), which will trigger the execution of the ML pipeline on cloud resources and automate the execution, data movement and tracking.

You can use the same approach to automate deployment, run exhaustive testing, apply governance and more, while adding more CI scripts and ML pipelines to match them and restricting who can execute which workflow and at what stage (development, staging, production).

## Critical Thinking Discussion Questions

*   TBD

## Exercises

*   TBD

# Chapter 4. Working with Data and Feature Stores

Machine learning takes data and turns it into predictive logic. Data is essential to the process, can come from many sources, and must be processed to make it usable. Therefore, data management and processing are the most critical components of machine learning.

Data can originate from different sources:

*   *Files*: Data stored in local or cloud files.
*   *Data warehouses*: Databases hosting historical data transactions.
*   *Online databases*: SQL, NoSQL, graph, or time-series databases hosting up to data transactional or application data.
*   *Data Streams*: Intermediate storage hosting real-time events and messages (for passing data reliably between services).
*   *Online services*: Any cloud service that can provide valuable data (this can include news sites, social, financial, government, and news services).
*   *Incoming messages*: Asynchronous messages and notifications which can arrive through emails or any other messaging services (Slack, WhatsApp, Teams, and so on).

Source data is processed and stored as features for use in the [model training](https://www.iguazio.com/glossary/model-training/) and model flows. In many cases, features are stored in two storage systems. One for batch access (training, batch prediction, and so on) and one for online retrieval (for real-time or online serving). As a result, there may be two separate data processing pipelines, one using batch processing and the other using real-time (stream) processing.

The data sources and processing logic will likely change over time, resulting in changes to the processed features and the model produced from that data. Therefore, applying versioning to the data, processing logic, and tracking data lineage are critical elements in any MLOps solution.

Delivery of accurate and high-quality production models requires high volumes of data and significant processing power. Processing of production data can be scaled using distributed analytics engines ([Apache Spark](https://spark.apache.org/), [Dask](https://www.dask.org/), [Google BigQuery](https://cloud.google.com/bigquery), and more), stream processing technologies (like [Apache Flink](https://flink.apache.org/)), or multi-stage data pipelines.

One of the mechanisms to automate integration with data sources, scalable batch and real-time data processing, data versioning, and feature management is to use a feature store. A feature store is a central hub for producing, sharing and monitoring features. Feature stores are essential in modern MLOps implementations and will be described in further detail in this chapter.

## Data Versioning and Lineage

Models and data products are derived from data. Therefore, collecting metadata and tracing the origin of the data allows better control and governance for data products. Furthermore, if you want to examine a specific version of a data product, you must understand the original data used to produce that product or model.

Data versioning, lineage, and metadata management are a set of essential MLOps practices that address the following:

1.  *Data quality*: Tracing data through an organization’s systems and collecting metadata and lineage information can help identify errors and inconsistencies. This makes it possible to take corrective action and improve data quality.
2.  *Model reproducibility and traceability*: Access to historical data versions allows us to reproduce model results and can be used for model debugging, troubleshooting, and trying out different parameter sets.
3.  *Data governance and auditability*: By understanding the origin and history of data, organizations can ensure that data follows expected policies and regulations, track sources of errors, and perform audits or investigations.
4.  *Compliance*: Data lineage can help organizations demonstrate compliance with regulations such as GDPR and HIPAA.
5.  *Simpler data management*: Metadata and lineage information enables better data discovery, mappings, profiling, integration, and migrations.
6.  *Better collaboration*: Data versioning and lineage can facilitate cooperation between data scientists and ML engineers by providing a clear and consistent view of the data used in ML models and when handling upgrades.
7.  *Dependency tracking*: Understanding how each data, parameters, or code change contributes to the results and providing insights into which data or model objects need to change due to data source modification.

### How It Works

As shown in [Figure 4-1](#ch04.xhtml#data-lineage), the data generation flow can be abstracted as having a set of data sources and parameters that are used as inputs to a data processing (computation) task that produces a collection of data products or artifacts. The output artifacts can be of different types, files, tables, machine learning models, charts, and so on.

![](assets/data-lineage.png)
