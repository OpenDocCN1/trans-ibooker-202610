

MLflow Tracking 是一个 API 和 UI，用于在运行机器学习代码时记录参数、代码版本、指标和工件，并稍后可视化结果。您可以在任何环境中使用 MLflow Tracking（例如，独立脚本或笔记本）将结果记录到本地文件或服务器，然后比较多次运行。它与库无关，并可与多个框架集成。

MLflow Tracking 围绕“运行”概念组织，运行是某段数据科学代码的执行。MLflow Tracking 提供一个 UI，让您可视化、搜索和比较运行，以及下载运行工件或元数据以在其他工具中进行分析。它包含以下主要功能：

- 基于实验的运行列表和比较
- 按参数或指标值搜索运行
- 可视化运行指标
- 下载运行结果

让我们在 PySpark Shell 中将 MLflow 追踪添加到我们的决策树代码中。假设您已下载 Covtype 数据集并对其熟悉。Covtype 数据集可作为压缩的 CSV 格式数据文件 covtype.data.gz 和随附信息文件 covtype.info 在线获取。

启动 pyspark-shell。如前所述，构建决策树可能资源密集。如果您有足够内存，请指定 --driver-memory 8g 或类似参数。

我们首先准备数据和机器学习流水线。

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

要开始使用 MLflow 记录，我们使用 mlflow.start_run() 启动一个运行。我们将使用 with 子句在块结束时自动结束运行。

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

现在，您可以通过追踪 UI 访问实验数据。运行 `mlflow ui` 命令启动它。它默认在端口 5000 上启动。您可以使用 `-p <port_name>` 选项更改默认端口。成功启动 UI 后，访问 http://localhost:5000/。您将看到如图 11-3 所示的 UI。您可以搜索所有运行、筛选符合特定条件的运行、并排比较运行等。如果需要，还可以将内容导出为 CSV 文件以在本地分析。在 UI 中点击名为 "decision-tree" 的运行。

## Apache Spark

在实验中追踪机器学习训练运行。了解更多

实验 ID：0

### 注记

显示 2 个匹配的运行

| | 开始时间 | 持续时间 | 运行名称 | 用户 | 来源 | 版本 | 模型 | accuracy | f1 | r2 | max_depth |
|---|---|---|---|---|---|---|---|---|---|---|---|
| | 1 分钟前 | 30.1s | decision-tree | analytical-m... | - | - | spark | 0.701 | 0.664 | - | 5 |
| | 5 个月前 | 41.3s | random-forest | analytical-m... | pyspark_li... | - | spark | - | - | 0.228 | 5 |

图 11-3. MLflow UI

查看单个运行时（如图 11-4 所示），您会发现 MLflow 存储所有相应的参数、指标等。您可以以自由文本形式添加关于此运行的注记，以及标签。

## 决策树

日期：2021-11-06 18:04:18
来源：d:\py\kernel_Launcher.py
用户：analytical-monk
持续时间：31.3s
状态：FINISHED
生命周期阶段：active

- 注记 [2]
- 参数 (2)
- 指标 (2)
- 标签
- 工件

- model
  - sparkml
  - MLmodel
  - conda.yaml
  - requirements.txt
  - feature-importance.csv

完整路径：/home/analytical-monk/pyspark_book/mlruns/0/443abb047751a426e733b09bab767222/artifacts/m...

## MLflow 模型

以下代码片段演示如何使用已记录的模型进行预测。您还可以将其注册到模型注册表以进行版本控制

### 模型架构

您的模型的输入和输出架构。了解更多

| 名称 | 类型 |
|---|---|
| 无架构。请参阅 MLflow 文档，了解如何将输入和输出架构包含在您的模型中。 | |

### 进行预测

在 Spark DataFrame 上进行预测：

```
import mlflow
logged_model = 'runs:/0/443abb047751a426e733b09bab767222/model'
# Load model as a Spark UDF.
loaded_model = mlflow.pyfunc.spark_udf(spark, model_uri=logged_model)
# Predict on a Spark DataFrame.
columns = list(df.columns)
df.withColumn('predictions', loaded_model(*columns)).collect()
```

现在我们能够追踪和复现实验。让我们现在讨论使用 MLflow 管理模型。

# 管理和服务 ML 模型

MLflow 模型是一种用于打包机器学习模型的标准格式，可在各种下游工具中使用——例如，通过 REST API 进行实时服务或在 Apache Spark 上进行批量推理。该格式定义了一种约定，让您可以以不同的“风格”保存模型，这些风格可被不同的库理解。

“风格”是使 MLflow 模型强大的关键概念。它们使得可以编写适用于任何 ML 库模型的工具，而无需将每个工具与每个库集成。MLflow 定义了几种其所有内置部署工具都支持的“标准”风格，例如描述如何以 Python 函数运行模型的“Python 函数”风格。但是，库也可以定义和使用其他风格。例如，MLflow 的 mlflow.sklearn 库允许将模型加载回作为 scikit-learn Pipeline 对象，用于感知 scikit-learn 的代码，或作为通用 Python 函数，用于只需应用模型的工具（例如，用于将模型部署到 Amazon SageMaker 的 mlflow sagemaker 工具）。

MLflow 模型是包含一组文件的目录。我们之前已使用 log_model API 记录了模型。这创建了一个名为 MLmodel 的文件。打开 decision-tree 运行，向下滚动到工件部分。查看 MLmodel 文件。其内容应与下图 11-5 所示类似。

