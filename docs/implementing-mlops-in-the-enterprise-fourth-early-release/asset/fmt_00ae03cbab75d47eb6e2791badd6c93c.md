
# Feature Store

* Labor-intensive data engineering to produce high-quality features.
* Duplicate efforts and resources in generating offline and online features which also lead to inaccurate results.
* Hard to incorporate data versioning and governance.
* Feature development work is duplicated for every new project.
* Lack of simple access to production-ready features at scale.
* Disjointed or nonexistent model and feature monitoring.

ML teams need a way to continuously deploy AI applications in a way that creates real, ongoing business value for the organization. Features are the fuel driving AI for the organization, and feature stores are the architectural answer that can simplify processes, increase [model accuracy](https://www.iguazio.com/glossary/model-accuracy-in-ml/) and accelerate the path to production.

A feature store provides a single pane of glass for sharing all available features across the organization along with their metadata. When data scientists start a new project, they can access this catalog and easily find features. But a feature store is not just a data layer; it is also a data transformation service enabling users to manipulate raw data and store it as features ready to be used for offline (training) and online (serving), without duplicating the work. In addition, some feature stores support strong security, versioning, and data snapshots, enabling better data lineage, compliance, and manageability.

Some of the largest tech companies that deal extensively with AI have built their own feature stores (Uber, Twitter, Google, Netflix, Facebook, Airbnb, and so on). The open-source and commercial landscape for feature stores has exploded in the last few years. This is a good indication to the rest of the industry of how important it is to use a feature store as a part of an efficient ML pipeline.

Most feature stores are limited to structured data handling (ML), but some can support both structured data and unstructured (text, documents, images, audio, and so on).

Feature stores are described in detail later in chapter four book. As illustrated in [Figure 2-6](#ch02.xhtml#feature-store-arch), they provide a mechanism to read data from various online or offline sources, conduct a set of data transformations, and persist the data in online and offline storage. Features are stored and cataloged along with all their metadata (schema, labels, statistics, and so on), allowing users to compose `feature Vectors` (joint multiple features from different feature sets) and use them for training or serving. The feature vectors are generated when needed, taking into account data versioning and time correctness (time traveling). Different engines are used for feature retrieval, real-time engine for serving, and batch one for training.

![](assets/feature-store-arch.png)

Here are some major benefits of a feature store:

* Faster development with far fewer engineering resources.
* Smooth migration from development to production.
* Increased model accuracy (same pipeline for online and offline).
* Better collaboration and security across teams.
* The ability to track lineage and address regulatory compliance.

### Note

Not all feature stores are born equal. Some are focused on cataloging and don’t automate the process of ingestion and online or offline transformation, which are the most labor-intensive tasks. Therefore, make sure you conduct a proper evaluation before selecting a solution.

# Model Development and Training

Data scientists generally go through the following process when developing models:

1. Extracting data manually from external sources.
2. Data labeling, exploration, and enrichment to identify potential patterns and features.
3. Model training and validation.
4. Model evaluation and testing.
5. Going back to step one and repeating until the desired outcomes (accuracy, loss, and so on.) have been achieved.

The traditional way is to use notebooks, small-scale data, and manual processes, but this does not scale and is not reproducible. Furthermore, in order to achieve maximum accuracy, experiments often need to be run with different parameters or algorithms (`AutoML`).

With `MLOps`, ML teams build machine learning pipelines that automatically collect and prepare data, select optimal features, run training using different parameter sets or algorithms, evaluate models, and run various model and system tests. All the executions, along with their data, metadata, code, and results, must be versioned and logged, providing quick results visualization, comparing them with past results, and understanding which data was used to produce each model.

Pipelines can be more complex—for example, when ML teams need to develop a combination of models or use deep learning or NLP. You can see a basic model development flow example in [Figure 2-7](#ch02.xhtml#model-development).

![](assets/model-dev.png)

ML pipelines can be started manually or preferably triggered automatically when:

1. The code, packages, or parameters change.
2. The input data or feature engineering logic change.
3. Concept drift is detected, and the model needs to be retrained with fresh data.

ML pipelines have the following features:

* Are built using microservices (containers or serverless functions), usually over `Kubernetes`.
* Have all their inputs (code, package dependencies, data, parameters) and the outputs (logs, metrics, data/features, artifacts, models) tracked for every step in the pipeline in order to reproduce or explain our experiment results.
* Use versioning for all the data and artifacts used throughout the pipeline.
* Store code and configuration in versioned `Git` repositories.
* Use continuous integration (`CI`) techniques to automate the pipeline initiation, test automation, review, and approval process.

Pipelines should be executed over scalable services or functions, which can span elastically over multiple servers or containers. This way, jobs complete faster, and computation resources are freed up once they are complete, saving high costs.

The resulting models are stored in a versioned model repository along with metadata, performance metrics, required parameters, statistical information, and so on. Models can be loaded later into batch or real-time serving microservices or functions.

## Writing and Maintaining Production ML Code

Many data scientists like the usability and interactivity of `Jupyter Notebooks` when they develop and evaluate models. It is convenient indeed to manipulate some code and immediately see a visual table or a chart, and most ML tutorials, examples, and Kagle projects are consumed as Notebooks.

You can find projects where the data preparation, training, evaluation, and even prediction are all made in one huge Notebook, but this approach can lead to challenges when moving to production, for example:

* Very hard to track the code changes across versions (in `Git`).
* Almost impossible to implement test harnesses and unit testing.
* Functions cannot be reused in various projects.
* Moving to production requires code refactoring and removal of visualization or scratch code.
* Lack of proper documentation.

The best approach is to use functional programming for code segments and Notebooks for interactive and visualization parts. [Example 2-1](#ch02.xhtml#data-prep-func) implements a data preparation function that accepts a dataset (`dataframe`) and some properties as inputs and returns the manipulated dataset. The function is documented and allows users to understand the purpose and usage.

### Example 2-1. Data prep function (`data_prep.py`)
