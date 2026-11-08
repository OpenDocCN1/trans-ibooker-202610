
MLRun provides an extensive API/SDK for tracking and searching across data and experiments. However, the real power is that it can deliver most of the features and automation without requiring additional coding.

Take [Example 4-4](#ch04.xhtml#mlrun-tracking). It accepts input data and parameters and generates output data and results. Note that, unlike the previous examples, the code doesn’t include argument parsing, data loading, conversion, logging, and so on.

### Example 4-4. MLRun code example

```
def data_preparation(dataset: pd.DataFrame, test_size=0.2):
    # preform processing on the dataset
    dataset = clean_df(dataset).dropna(how="any", axis="rows")
    dataset = dataset.drop(columns=["key", "pickup_datetime"])
    train, test = train_test_split(dataset, test_size=test_size)
    return train, test, "fare_amount"
```

When executing the function and specifying the input data object URL or path (a file, a remote object, or a complex type), it is automatically loaded into the function. For example, using AWS `boto` drivers to access `S3` objects or `BigQuery` drivers to access a `BigQuery` table. Then, the data is converted to the accepting format (`DataFrame`) and injected into the user code.

MLRun can auto-detect the returned value type (for example, `train` and `test` are of type `DataFrame`) and store it in the best form, along with auto-generated metadata, links to the job details and data input objects, and versioning information. If the data repeats itself, it is deduplicated and stored only once to preserve storage space.

Data objects have type-specific visualized in the UI and client SDK regardless of how and where they are stored, For example, tabular formats with table metadata (schema, stats, and more) for datasets or interactive graphics for chart objects. See [Figure 4-8](#ch04.xhtml#mlrun-dataset-artifact) and [Figure 4-9](#ch04.xhtml#mlrun-artifact-sdk).

![](assets/mlrun-dataset-artifact.png)![](assets/mlrun-artifact-sdk.png)

In summary, MLRun is a complete MLOps orchestration framework with a significant focus on data management, movement, versioning, and automation. In addition, MLRun has a rich object model that covers different types of data and execution objects (functions, runs, workflows, and more), how they are related, and how they are used. MLRun focuses on abstraction and automation to reduce development and deployment efforts. However, MLRun is not a general data management and versioning solution, and its value is maximized when used in the context of MLOps.

### Other Frameworks

Some tools, such as [Delta Lake](https://delta.io/) and [LakeFS](https://github.com/treeverse/lakeFS), handle data lake versioning. However, those tools are not focused on the ML lifecycle and may require integration to make them useful for MLOps.

Cloud vendors provide solutions that are usually tightly bound to their internal services. For example, see [Amazon SageMaker ML Lineage Tracking](https://docs.aws.amazon.com/sagemaker/latest/dg/lineage-tracking.xhtml) and [Azure ML Datasets](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-data-assets).

# Data Preparation and Analysis at Scale

Data processing is used extensively across the different data, ML, and application pipelines. When working with production data, there is a need to support more extensive scale and performance and, in some cases, handle data as it arrives in real time.

Practices that work during interactive development, for example, storing the data in a CSV file and reading it into the notebook, don’t work with gigabytes or terabytes of data. They require distributed or parallel data processing approaches.

The general architecture for distributed data processing is the same, with differences in how data is distributed and collected and which APIs they use. For example, the data is partitioned across multiple computer nodes, the processing requests or queries arrive at one or more nodes for local processing, and the results are collected and merged for a single answer. In addition, complex queries may shuffle data between nodes or execute multiple processing and movement steps.

[Figure 4-10](#ch04.xhtml#map-reduce-example) demonstrates how distributed data processing works using the map-reduce approach for counting words in a document.

![](assets/map-reduce-example.png)

### Structured and Unstructured Data Transformations

Data can be *structured*, meaning it conforms to a specific format or structure and often has a predefined schema or data model. Structured data can be a database table or files with a structured layout (for example, csv, excel, json, ml, parquet). However, most of the world’s data is *unstructured*, is usually more complex, and is more difficult to process than structured data. This includes free text, logs, web pages, images, video, and audio.

Here are some examples of analytic transformations that can be performed on structured data:

1. *Filtering*: Selecting a subset of the data based on certain criteria, such as a specific date range or specific values in a column.
2. *Sorting*: Ordering the data based on one or more columns, such as sorting by date or by a specific value.
3. *Grouping*: Organizing the data into groups based on one or more columns, such as grouping by product category or by city.
4. *Aggregation*: Calculating summary statistics, such as count, sum, average, maximum, and standard deviation, for one or more columns.
5. *Joining*: Combining data from multiple tables or datasets based on common columns, such as joining a table of sales data with a table of customer data.
6. *Mapping*: Mapping values from one or more columns to new column values using user-defined operations or code. Stateful mapping can calculate new values based on original values and accumulated states from older entries (for example, time passed from the last login).
7. *Time series analysis*: Analyzing or aggregating data over time, such as identifying trends, patterns, or anomalies.

The following technique examples can be used to process unstructured data or convert it to structured data:

1. *Text mining*: Using natural language processing (NLP) techniques to extract meaning and insights from text data. Text mining can extract information such as sentiment, entities, and topics from text data.
2. *Computer vision*: Using image and video processing techniques to extract information from visual data. Computer vision can extract information such as object recognition, facial recognition, and image classification.
3. *Audio and speech recognition*: Using speech-to-text and audio processing techniques to extract meaning and insights from audio data. Audio and speech recognition can extract information such as speech-to-text, sentiment, and speaker identification.
4. *Data extraction*: Using techniques such as web scraping and data extraction to extract structured data from unstructured data sources.

Various machine-learning methods can be used to transform raw data into more meaningful data, for example:

1. *Clustering*: Grouping similar data points based on certain characteristics, such as customers with similar purchasing habits.
2. *Dimensionality reduction*: Reducing the number of features in a dataset to make it easier to analyze or visualize.
3. *Regression and classification*: Predicting a class or a value based on other data features.
4. *Imputing*: Determine the expected value based on other data points in case of missing data.

### Distributed Data Processing Architectures

Data-processing architectures can be broken into three main categories:

1. *Interactive*: A request or an update arrives, is processed, and a response is returned immediately. For example, `SQL` and `NoSQL` databases, data warehouses, key/value stores, graph databases, time-series databases, cloud services, and more.
