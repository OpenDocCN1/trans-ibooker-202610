6. Execute the Lambda function and look at the **Execution results** tab. You should be able to see a message that confirms the `awswrangler` package is built from our custom code base.

```
Response
{
  "statusCode": 200,
  "body": "\"Hello from Lambda!\""
}

Function Logs
OpenBLAS WARNING - could not determine the L2 cache size on this system, assuming 256k
START RequestId: 436fa52d-2b42-4ffb-96ad-fa7769981eb7 Version: $LATEST
2.17.0
Pandas on AWS.Updated for custom intallation.
This was changed as a part of Learning exercise.
END RequestId: 436fa52d-2b42-4ffb-96ad-fa7769981eb7
```

Figure 3.22: awswrangler custom Lambda installation – verifying the Lambda output

# Standard and custom installation for AWS Glue jobs

We will look at the scenario where the IT person from Project B wants to use AWS SDK for pandas on a Glue Python shell to perform data-wrangling activities on data from a source database. Glue jobs have three modes of execution – Python shell, Spark ETL, and Spark Streaming ETL. Python shell job mode is used to process small-scale file processing that doesn’t require an Apache Spark environment. For larger data volumes and distributed processing, you can use Spark job mode, and for streaming pipelines, you can choose Spark Streaming job mode. We will see how to install `awswrangler` for Python shell and Spark jobs.

# Standard installation of awswrangler for a Python shell job

The Python shell job environment comes in two different Python versions, 3.6 and 3.9 (the default is Python 3.6). We can attach external libraries to a Glue Python shell job using wheel files. Wheel files are part of the Python ecosystem and help in the Python package installation process, making it faster and more stable:

+   - The AWS Glue Python 3.6 environment doesn’t come preinstalled with `awswrangler`. Refer to the already installed libraries (https://docs.aws.amazon.com/glue/latest/dg/add-job-python.html#python-shell-supported-library) in the Python Glue job environment before adding additional libraries. We have to attach the `wheel` file for the `awswrangler` version that we want to use in our Glue job and attach to it, as documented in the following steps:
+     1. For Python 3.6 jobs, you can download the wheel files from the public artifacts bucket using the following syntax:
+        `s3://aws-data-wrangler-public-artifacts/releases/<version>/ awswrangler-<version>-py3-none-any.whl`.