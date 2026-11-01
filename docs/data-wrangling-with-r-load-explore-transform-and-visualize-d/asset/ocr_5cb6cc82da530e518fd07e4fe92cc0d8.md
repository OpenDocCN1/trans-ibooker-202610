more secure, and more performant environment to run your jobs. We will install awswrangler using the following steps:

+   1. Create a Glue 3.0 job and provide the job parameters as follows:

```
- key: --additional-python-modules   values: pyarrow==2,awswrangler==<<version>>
```

![](bbox=[0.12, 0.21, 0.88, 0.29])

Figure 3.31: awswrangler glue standard installation – job parameter configuration

+   2. Paste the following code in the script window, save the job, and click to run it:

```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job
import awswrangler as wr
## @params:
[JOB_NAME]args = getResolvedOptions(sys.argv, ['JOB_NAME'])
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args['JOB_NAME'], args)
print(wr.__version__)
job.commit()
```

+   3. Check CloudWatch Logs once the job completes execution. You should see the version of awswrangler being printed there successfully:

```
2022-09-30 16:06:07,219 main INFO Log4j appears to be running in a Servlet environment, but there's no log4j-web module available. If you support, please add the log4j-web JAR to your web archive or server lib directory.
2.17.0
```

Figure 3.32: awswrangler glue standard installation – verifying the output

# Custom installation of awswrangler on a Glue Spark job

For custom installation of awswrangler, users can follow the following steps to install a custom awswrangler build from a wheel file, in a similar way to the Python shell job:

+   1. The steps are similar to those in the Python shell job. Perform steps 1–3 in the Python shell job custom installation to create the wheel file. Upload your wheel file to an S3 location.

+   2. Attach the wheel file to the Glue spark job by providing the wheel file location as a part of the job parameters, as shown in the following screenshot.

![](bbox=[0.12, 0.74, 0.87, 0.84])

Figure 3.33: awswrangler Glue Spark custom installation – attaching the wheel file to a job

+   3. Copy and paste the code from the earlier Spark program and execute the Spark job. Verify the output logs to see whether the custom library has been installed in the Glue spark job.