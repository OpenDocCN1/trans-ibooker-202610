MLflow Tracking is an API and UI for logging parameters, code versions, metrics, and artifacts when running your machine learning code and for later visualizing the results. You can use MLflow Tracking in any environment (for example, a standalone script or a notebook) to log results to local files or to a server, then compare multiple runs. It is library-agnostic and integrates with multiple frameworks.

MLflow Tracking is organized around the concept of runs, which are executions of some piece of data science code. MLflow Tracking provides a UI that lets you visualize, search and compare runs, as well as download run artifacts or metadata for analysis in other tools. It contains the following key features:

+   - Experiment-based run listing and comparison
+   - Searching for runs by parameter or metric value
+   - Visualizing run metrics
+   - Downloading run results

Let’s add MLflow tracking to our decision tree code in the PySpark shell. It’s assumed that you have downloaded the Covtype data set and are familiar with it. The Covtype dataset is available online as a compressed CSV-format data file, covtype.data.gz, and accompanying info file, covtype.info.

Start pyspark-shell. As mentioned previously, building decision trees can be resource-intensive. If you have the memory, specify --driver-memory 8g or similar.

We start by preparing the data and machine learning pipeline.

```
from pyspark.ml import Pipeline
from pyspark.sql.functions import col
from pyspark.sql.types import DoubleType
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.classification import DecisionTreeClassifier
data_without_header = spark.read.option("inferSchema",
```