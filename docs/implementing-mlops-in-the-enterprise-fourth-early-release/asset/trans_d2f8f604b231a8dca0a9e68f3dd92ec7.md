
Airflow 还提供基于 Web 的用户界面（参见 [图 4-12](#ch04.xhtml#airflow-graph)）用于管理和 [监控](https://www.iguazio.com/glossary/model-monitoring/) 工作流，包括查看每个任务的状态、重试失败任务以及手动触发或调度任务的能力。它还包括用于管理和组织工作流的功能，例如在任务之间定义依赖关系和设置任务重试逻辑。

![](assets/airflow-graph.png)

[示例 4-5](#ch04.xhtml#airflow-pipeline-code) 是 Python 代码的一个示例，可用于在 Apache Airflow 中创建 `DAG`，该 DAG 从 CSV 文件读取数据、处理数据并将其写入目标位置。

### 示例 4-5. Airflow 数据管道代码示例

```
import csv
from airflow import DAG
from airflow.operators.python_operator import PythonOperator
from datetime import datetime, timedelta

def process_data(**kwargs):
    ti = kwargs['ti']
    input_file = ti.xcom_pull(task_ids='read_file')
    processed_data = do_data_processing(input_file)
    return processed_data

def do_data_processing(input_file):
    # Placeholder function that performs data processing
    processed_data = input_file
    return processed_data

def read_csv_file(file_path):
    with open(file_path, 'r') as file:
        reader = csv.reader(file)
        return list(reader)

def write_csv_file(file_path, data):
    with open(file_path, 'w') as file:
        writer = csv.writer(file)
        writer.writerows(data)

default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': datetime(2021, 1, 1),
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
}

dag = DAG(
    'data_processing_dag',
    default_args=default_args,
    description='A DAG that reads data from a CSV file, processes it'
                ', and writes it to a destination',
    schedule_interval=timedelta(hours=1),
)

read_file = PythonOperator(
    task_id='read_file',
    python_callable=lambda: read_csv_file('/path/to/input_file.csv'),
    xcom_push=True,
    dag=dag,
)

process_data = PythonOperator(
    task_id='process_data',
    python_callable=process_data,
    provide_context=True,
    dag=dag,
)

write_file = PythonOperator(
    task_id='write_file',
    python_callable=lambda: write_csv_file('/path/to/output_file.csv',
                                           ti.xcom_pull(task_ids='process_data')),
    provide_context=True,
    dag=dag,
)

read_file >> process_data >> write_file
```

有多个基于云的批处理数据管道服务，例如 [AWS Glue](https://aws.amazon.com/glue/)、[Google Cloud Composer](https://cloud.google.com/composer)（基于 Airflow）和 [Azure Data Factory](https://azure.microsoft.com/en-us/products/data-factory)。

Hadoop 或其他批处理管道的一个缺点是需要在每个步骤中从磁盘读取数据、处理它，然后再次将其写入磁盘。然而，像 [Spark](https://spark.apache.org/) 和 [Dask](https://www.dask.org/) 这样的框架知道如何将处理管道编译成最优图，在可能的情况下在内存中完成任务，从而最大限度地减少磁盘 IO 并最大化性能。

[示例 4-6](#ch04.xhtml#spark-pipeline-code) 演示了 Spark 代码，该代码读取 CSV 文件、处理数据并将结果写入目标文件。

### 示例 4-6. PySpark 数据管道代码示例

```
from pyspark.sql import SparkSession

# Create a Spark session
spark = SparkSession.builder.appName("SimpleBatchProcessing").getOrCreate()

# Load a CSV file into a Spark DataFrame
df = spark.read.csv("/path/to/input_file.csv", header=True, inferSchema=True)

# Perform some data processing on the DataFrame
processed_df = df.groupBy("column_name").agg({"column_name": "mean"})

# Write the processed DataFrame to a new CSV file
processed_df.write.csv("/path/to/output_file.csv", header=True)

# Stop the Spark session
spark.stop()
```

同一示例在 [示例 4-7](#ch04.xhtml#dask-pipeline-code) 中使用 Dask 实现。Dask 的优势在于其操作与 Python pandas 非常相似，这对数据科学家来说是一个巨大的优势。然而，Spark 通常更具可扩展性和鲁棒性。

### 示例 4-7. Dask 数据管道代码示例

```
import dask.dataframe as dd

# Load a CSV file into a Dask DataFrame
df = dd.read_csv('/path/to/input_file.csv')

# Perform some data processing on the DataFrame
processed_df = df.groupby('column_name').column_name.mean().compute()

# Write the processed DataFrame to a new CSV file
processed_df.to_csv('/path/to/output_file.csv', index=False)
```

可以看出，Spark 和 Dask 示例与 Airflow 示例相比要简单得多。然而，Airflow 更适合管理和跟踪长且复杂的作业。

## 流处理

*流处理* 能够实现可扩展、容错和实时数据处理。它通常用于实时处理大量数据的应用程序中，例如实时分析、欺诈检测或推荐。

在流处理中，数据和传入事件被推送到流（队列）中，并由一个或多个 worker 读取。Worker 按顺序处理数据，进行转换、聚合结果，并将结果写入数据库或输出流。与传统消息队列不同，流处理是有序发生的。例如，假设流包含两个事件，一个是客户登录，另一个是客户注销。未按顺序处理它们可能导致状态损坏。另一个例子是发生资金存款操作，随后是取款。如果操作按错误顺序处理，取款操作可能会被拒绝。

流被设计为可扩展。它们被划分为分区，每个分区处理特定的一组数据对象，因此不会违反顺序。例如，用户活动流按用户 ID 分区，以便特定用户的活动始终存储在同一分区并由同一 worker 处理。

流 ([Kafka](https://kafka.apache.org/)、[AWS Kinesis](https://aws.amazon.com/kinesis/) 等) 与消息队列 ([RabbitMQ](https://www.rabbitmq.com/)、[AMQP](https://www.amqp.org/)、[AWS SQS](https://aws.amazon.com/sqs/)、[Google PubSub](https://cloud.google.com/pubsub) 等) 不同。消息队列不保证消息顺序。然而，它们保证消息的可靠传递，而在流的情况下，客户端管理可靠性。此外，由于流提供的更直接的逻辑和并行性，它们速度要快得多。

[图 4-13](#ch04.xhtml#streaming-application) 说明了一个流应用程序，其中客户端发布的数据分布在各个分区之间（基于分区键的哈希）。一个 worker 从每个分区读取并处理数据。Worker 可以使用数据库在已知间隔（检查点）存储状态，以便在发生故障时可以恢复状态，或者 worker 可以释放未使用的内存。最后，结果可以写入目标数据库或输出流。

流提供“至少一次语义”。因此，同一条消息可能出现多次。通过检查点的帮助提供“恰好一次语义”（同一条消息仅处理一次）的一种方法。流是有序处理的，并且状态可以在每个微批处理后持久化。在发生故障的情况下，worker 可以恢复最后一个检查点数据（状态），从该点开始处理事件，并忽略旧事件。

### 流处理框架
