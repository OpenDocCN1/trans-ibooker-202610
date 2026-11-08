
## 亚马逊 EMR

[*亚马逊 EMR*](https://aws.amazon.com/emr/) - 用于处理大数据以进行时间序列分析和可视化的服务：

```
import boto3

emr = boto3.client('emr')
response = emr.run_job_flow(
    Name='emr_cluster_name',
    Instances={
        'InstanceGroups': [
            {
                'Name': 'master_instance_group',
                'InstanceRole': 'MASTER',
                'InstanceType': 'm5.xlarge',
                'InstanceCount': 1,
            },
        ],
        'Ec2KeyName': 'ec2_key_name',
        'KeepJobFlowAliveWhenTerminationProtected': False,
    },
    JobFlowRole='EMR_EC2_DefaultRole',
    ServiceRole='EMR_DefaultRole',
    VisibleToAllUsers=True,
    Applications=[
        {
            'Name': 'Spark'
        },
    ],
)
```

这些 AWS 服务支持时间序列分析，可用于为 MLOps 项目构建和部署可扩展的时间序列解决方案。

## 深入探索 DeepAR+ 中的时间序列

DeepAR+ 是一款 AWS 机器学习服务，利用深度学习算法进行时间序列预测。 在时间序列中利用深度学习可以提供独特的能力，如本章前面所述。 DeepAR+ 在管理大规模、复杂的时间序列数据方面表现出色，为各种用例提供有价值的预测，从需求预测到交通和设备维护预测。

DeepAR+ 基于 DeepAR 算法构建，这是一种专门为时间序列预测开发的先进深度学习算法。 还记得本章前面关于统计技术在缺失数据或异常值方面的局限性时我们对“信号与噪声”的定义吗？ DeepAR+ 在此发挥作用，其功能可处理缺失值和不规则间隔的数据。

要使用 DeepAR+，必须将时间序列数据上传到 Amazon `S3` 并创建 DeepAR+ 模型。 DeepAR+ 不仅提供预测，还会为其预测生成置信区间，使组织能够感知潜在结果的范围并做出更明智的决策。

工作流程在 [图 8-2](#ch08.xhtml#Figure-9.1-deepar-forecast) 中说明了使用 `AWS CLI` 接口的每个步骤，直至最终预测。

![](assets/9.1-deepar-forecast.png)

以下代码示例反映了此过程：您可以将其替换为您的时间序列数据集，并根据未来的 `AWS CLI` 更改进行调整。 通过这些示例，我们建立一个 `S3` 存储桶，将时间序列数据上传到 `S3`，创建 DeepAR+ 数据集，将数据导入 DeepAR+ 数据集，创建 DeepAR+ 预测器，最后使用 DeepAR+ 预测器创建预测。

1. 创建 S3 存储桶以存储数据：

```
$ aws s3 mb s3://deepar-example-bucket
```

2. 将时间序列数据上传到 S3：

```
$ aws s3 cp time-series-data.csv s3://deepar-example-bucket/data.csv
```

3. 创建符合模式 `^[a-zA-Z][a-zA-Z0-9_]*` 的 DeepAR+ 数据集：

```
$ aws forecast create-dataset \
  --dataset-name "DeepARExampleDataset" \
  --data-frequency "H" \
  --dataset-type "TARGET_TIME_SERIES" \
  --domain "CUSTOM" \
  --schema '{"Attributes":[{"AttributeName":"timestamp",\
  "AttributeType":"timestamp"},{"AttributeName":"target_value",\
  "AttributeType":"float"},{"AttributeName":"item_id",\
  "AttributeType":"string"}]}'
```

4. 将时间序列数据导入 DeepAR+ 数据集：

```
$ aws forecast create-dataset-import-job \
  --dataset-import-job-name "DeepARExampleDatasetImportJob" \
  --dataset-arn $(aws forecast list-datasets | \
  jq '.Datasets[] | select(.DatasetName == "DeepARExampleDatasett") | \
  .DatasetArn') \
  --data-source "S3" \
  --data-source-config '{"S3Config": {"Path": \
  "s3://deepar-example-bucket/data.csv", "RoleArn": \
  "arn:aws:iam::ACCOUNT_ID:role/ForecastRole"}}'
```

5. 创建 DeepAR+ 预测器：

```
$ aws forecast create-predictor \
  --predictor-name "DeeparExamplePredictor" \
  --algorithm-arn "arn:aws:forecast:::algorithm/Deep_AR_Plus" \
  --forecast-horizon 24 \
  --perform-auto-ml "false" \
  --input-data-config '{"DatasetGroupArn": $(aws forecast list-dataset-groups | \
  jq '.DatasetGroups[] | select(.DatasetGroupName == "DeepARExampleDatasetGroup") | \
  .DatasetGroupArn'), "SupplementaryFeatures": [{"Name": "item_id", "Value": "item1"}, \
  {"Name": "timestamp", "Value": "yyyy-MM-dd HH:mm:ss"}]}' \
  --featurization-config '{"ForecastFrequency": "H"}'
```

6. 最后，使用 DeepAR+ 预测器创建预测：

```
# create a forecast using the DeepAR+ predictor
$ aws forecast create-forecast \
  --forecast-name "DeeparExampleForecast" \
  --predictor-arn $(aws forecast list-predictors | \
  jq '.Predictors[] | select(.PredictorName == "DeeparExamplePredictor") | \
  .PredictorArn')
```

这里的关键见解是 `CLI` 为探索基于云的时间序列预测提供了一个可访问的入口点，在利用 AWS ML 和 AI 服务时，通常比从笔记本或 SDK 开始更简单。 我们将转向一种对比方法：通过 Google `BigQuery` 使用 `SQL`。 这种转变为我们的探索引入了另一种途径，将我们的工具包扩展到笔记本和 `Python` 之外。

## 使用 GCP BigQuery 和 SQL 进行时间序列分析

将时间序列分析灵活性提升到另一个层面的平台之一是 Google `BigQuery`。 之前讨论的权衡变得不那么具有挑战性，因为您可以并排使用统计技术和机器学习技术。 `BigQuery` 在建模时间序列数据方面有几个独特之处：

* *可扩展性*: `BigQuery` 处理大规模数据，非常适合同时分析数百万个时间序列。 它可以快速处理 PB 级数据，使得能够跨多个大量时间序列列分析趋势和预测成为可能。

* *多时间序列预测*: `BigQuery ML` 允许使用单个查询预测多个时间序列。 当处理大量时间序列变量时，此功能非常有利，因为它消除了为每个变量运行单独查询的需求。

* *ARIMA 和 ARIMA_PLUS 模型*: `BigQuery ML` 支持 `ARIMA`（自回归积分滑动平均）模型，特别适合时间序列数据。 `ARIMA_PLUS` 模型还包括假日效应，并且可以根据外部因素改变模型行为，这对某些类型的时间序列数据非常有益。

* *与 Google Cloud 的集成*: `BigQuery` 是 Google Cloud Platform (`GCP`) 的一部分，允许与其它 `GCP` 服务无缝集成。 这种协同作用使得将时间序列分析纳入更广泛的数据工作流程变得更容易。

* *基于 SQL 的 ML*: `BigQuery` 使用 `SQL`，一种许多数据专业人士熟悉的语言，进行机器学习任务。 `SQL` 语言降低了那些想要构建时间序列模型但没有强大机器学习或编程背景的人的入门门槛。 此外，许多人更喜欢使用 `SQL` 查询数据而不是 `Python` 或 `R`。

* *评估工具*: `BigQuery` 提供内置函数如 `ML.EVALUATE` 用于评估时间序列预测的准确性。 此功能简化了评估模型性能并进行必要调整的过程。

这篇关于纽约市 Citi Bike 出行的 [教程](https://cloud.google.com/bigquery/docs/arima-speed-up-tutorial) 演示了如何使用 `BigQuery` 高效训练时间序列模型并使用单个查询执行多个时间序列预测。 该教程利用托管在 `GCP` 上的纽约市 Citi Bike 出行数据集和爱荷华州酒类销售数据集。 主要步骤包括：

* 在 `BigQuery` 中创建数据集以存储 ML 模型。

* 使用 Citi Bike 数据集创建要预测的时间序列。

* 使用默认参数同时预测多个时间序列，与使用多个 `CREATE MODEL` 查询相比加快了流程。

* 使用 `ML.EVALUATE` 函数评估每个时间序列的预测准确性。
