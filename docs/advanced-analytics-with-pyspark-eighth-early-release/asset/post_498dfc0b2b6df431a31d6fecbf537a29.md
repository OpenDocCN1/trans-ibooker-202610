
MLflow Tracking is an API and UI for logging parameters, code versions, metrics, and artifacts when running your machine learning code and for later visualizing the results. You can use MLflow Tracking in any environment (for example, a standalone script or a notebook) to log results to local files or to a server, then compare multiple runs. It is library-agnostic and integrates with multiple frameworks.

MLflow Tracking is organized around the concept of runs, which are executions of some piece of data science code. MLflow Tracking provides a UI that lets you visualize, search and compare runs, as well as download run artifacts or metadata for analysis in other tools. It contains the following key features:

- Experiment-based run listing and comparison
- Searching for runs by parameter or metric value
- Visualizing run metrics
- Downloading run results

Let’s add MLflow tracking to our decision tree code in the PySpark shell. It’s assumed that you have downloaded the Covtype data set and are familiar with it. The Covtype dataset is available online as a compressed CSV-format data file, covtype.data.gz, and accompanying info file, covtype.info.

Start pyspark-shell. As mentioned previously, building decision trees can be resource-intensive. If you have the memory, specify --driver-memory 8g or similar.

We start by preparing the data and machine learning pipeline.

```
from pyspark.ml import Pipeline
from pyspark.sql.functions import col
from pyspark.sql.types import DoubleType
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.classification import DecisionTreeClassifier
data_without_header = spark.read.option("inferSchema", True).option("header", False).csv("data/covtype.data")

colnames = ["Elevation", "Aspect", "Slope",
"Horizontal_Distance_To_Hydrology",
"Vertical_Distance_To_Hydrology",
"Horizontal_Distance_To_Roadways", "Hillshade_9am",
"Hillshade_Noon", "Hillshade_3pm",
"Horizontal_Distance_To_Fire_Points"] + \
[f"Wilderness_Area({i})" for i in range(4)] + \
[f"Soil_Type_{i}" for i in range(40)] + \
["Cover_Type"]

data = data_without_header.toDF(*colnames).withColumn("Cover_Type",
col("Cover_Type").cast(DoubleType()))

(train_data, test_data) = data.randomSplit([0.9, 0.1])

input_cols = colnames[:-1]
vector_assembler = VectorAssembler(inputCols=input_cols, outputCol="featureVector")

classifier = DecisionTreeClassifier(seed = 1234,
labelCol="Cover_Type", featuresCol="featureVector",
predictionCol="prediction")

pipeline = Pipeline(stages=[vector_assembler, classifier])
```

To start logging with MLflow, we start a run using mlflow.start_run(). We will use a with clause to automatically end the run at the end of the block.

```
import mlflow
import mlflow.spark
import pandas as pd
from pyspark.ml.evaluation import MulticlassClassificationEvaluator

with mlflow.start_run(run_name="decision-tree"):
    # Log param: max_depth
    mlflow.log_param("max_depth", classifier.getMaxDepth())
    # Log model
    pipeline_model = pipeline.fit(train_data)
    mlflow.spark.log_model(pipeline_model, "model")
    # Log metrics: Accuracy and F1
    pred_df = pipeline_model.transform(test_data)
    evaluator = MulticlassClassificationEvaluator(labelCol="Cover_Type",
                  predictionCol="prediction")
    accuracy = evaluator.setMetricName("accuracy").evaluate(pred_df)
    f1 = evaluator.setMetricName("f1").evaluate(pred_df)
    mlflow.log_metrics({"accuracy": accuracy, "f1": f1})
    # Log artifact: feature importance scores
    tree_model = pipeline_model.stages[-1]
    feature_importance_df = (
    pd.DataFrame(list(zip(vector_assembler.getInputCols(),
                      tree_model.featureImportances)),
             columns=["feature", "importance"])
     .sort_values(by="importance", ascending=False))
    feature_importance_df.to_csv("feature-importance.csv",
               index=False)
    mlflow.log_artifact("feature-importance.csv")
```

We can now access our experiment data via the tracking UI. Start it by running `mlflow ui` command. It starts by default on port 5000. You can use the `-p <port_name>` option to change the default port. Once you have successfully started the UI, go to http://localhost:5000/. You will see a UI as shown in Figure 11-3. You can search across all the runs, filter for those that meet particular criteria, compare runs side by side, etc. If you wish, you can also export the contents as a CSV file to analyze locally. Click on the run in the UI named "decision-tree".

## Apache Spark

Track machine learning training runs in an experiment. Learn more

Experiment ID: 0

### Notes

Showing 2 matching runs

| | Start Time | Duration | Run Name | User | Source | Version | Models | accuracy | f1 | r2 | max_depth |
|---|---|---|---|---|---|---|---|---|---|---|---|
| | 1 min. ago | 30.1s | decision-tree | analytical-m... | - | - | spark | 0.701 | 0.664 | - | 5 |
| | 5 months ago | 41.3s | random-forest | analytical-m... | pyspark_li... | - | spark | - | - | 0.228 | 5 |

Figure 11-3. MLflow UI

When viewing an individual run, as shown in Figure 11-4, you'll notice that MLflow stores all the corresponding parameters, metrics, etc. You can add notes about this run in free text, as well as tags.

## decision tree

Date: 2021-11-06 18:04:18
Source: d:\py\kernel_Launcher.py
User: analytical-monk
Duration: 31.3s
Status: FINISHED
Lifecycle Stage: active

- Notes [2]
- Parameters (2)
- Metrics (2)
- Tags
- Artifacts

- model
  - sparkml
  - MLmodel
  - conda.yaml
  - requirements.txt
  - feature-importance.csv

Full Path: /home/analytical-monk/pyspark_book/mlruns/0/443abb047751a426e733b09bab767222/artifacts/m...

## MLflow Model

The code snippets below demonstrate how to make predictions using the logged model. You can also register it to the model registry to version control

### Model schema

Input and output schema for your model. Learn more

| Name | Type |
|---|---|
| No schema. See MLflow docs for how to include input and output schema with your model. | |

### Make Predictions

Predict on a Spark DataFrame:

```
import mlflow
logged_model = 'runs:/0/443abb047751a426e733b09bab767222/model'
# Load model as a Spark UDF.
loaded_model = mlflow.pyfunc.spark_udf(spark, model_uri=logged_model)
# Predict on a Spark DataFrame.
columns = list(df.columns)
df.withColumn('predictions', loaded_model(*columns)).collect()
```

We are now able to track and reproduce our experiments. Let’s now discuss managing our models using MLflow.

# Managing and serving ML models

An MLflow Model is a standard format for packaging machine learning models that can be used in a variety of downstream tools—for example, real-time serving through a REST API or batch inference on Apache Spark. The format defines a convention that lets you save a model in different “flavors” that can be understood by different libraries.

Flavors are the key concept that makes MLflow Models powerful. They make it possible to write tools that work with models from any ML library without having to integrate each tool with each library. MLflow defines several “standard” flavors that all of its built-in deployment tools support, such as a “Python function” flavor that describes how to run the model as a Python function. However, libraries can also define and use other flavors. For example, MLflow’s mlflow.sklearn library allows loading models back as a scikit-learn Pipeline object for use in code that is aware of scikit-learn, or as a generic Python function for use in tools that just need to apply the model (for example, the mlflow sagemaker tool for deploying models to Amazon SageMaker).

An MLflow model is a directory containing a set of files. We had earlier logged our model using the log_model API. This created a file called MLmodel. Open the decision-tree run and scroll down to the Artifacts section. Check out the MLmodel file. Its contents should be similar to what’s depicted below in Figure 11-5.
