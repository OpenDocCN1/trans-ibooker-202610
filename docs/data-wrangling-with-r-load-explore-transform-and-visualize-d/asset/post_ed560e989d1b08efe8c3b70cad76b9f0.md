
`install` path on your machine. You can then move those packages to the deployment folder of your Lambda function and archive them as a ZIP file to be uploaded into the Lambda function.

Lambda functions can also have layers, which are saved in a ZIP file with the additional code/libraries. It can also contain a custom runtime, data, or configuration file. Using layers in a Lambda function will reduce the size of the deployment package, and the same Lambda layer can be shared across multiple Lambda functions, promoting reusable code components. Also, this helps to separate the responsibility of managing code segments among different teams. In addition, when you deploy code and dependencies in a single ZIP file, the Lambda function is not editable in the console, adding more difficulty in code maintenance. Currently, Lambda layers can be created with `.zip` file archives.

With all the preceding content, where would we use Lambda layers? Consider a situation where there is a central data warehouse, and different project teams access data from it using Lambda functions as microservices, and use it for different purposes. There are requirements to provide access only to certain user groups within the organization and also to perform data masking when accessing data. The database administrators are also worried about too many concurrent users accessing the database and leaving idle open connections that could strain the database. The individual project teams are also concerned about using and maintaining database drivers to connect to the warehouse, and they don’t have experience in managing database connection state, configurations, and so on. The ideal solution is to create a Lambda layer to access the warehouse that meets all preceding requirements, which will be maintained by the database administration team, and individual teams can utilize that Lambda layer to develop their code in Lambda functions. This promotes code reuse, separation of responsibilities, and quicker development cycles.

In the same context, users can use AWS SDK for pandas from Lambda layers. There are two options for using AWS SDK for pandas in your Lambda code.

## Standard installation – a Lambda managed layer

Create a Python Lambda function in the AWS Console with a Python 3.9 runtime. Scroll down the Lambda function overview screen until you reach the bottom of the page, where you can find the **Layers** section:

![](img/ec1933a597ce63afa3bcf7496364f412_109_0.png)

Figure 3.11: awswrangler Lambda installation

You can then choose to add a layer; choose the AWSSDKPandas–Python39 layer, as shown in the following screenshot. The version number might change in the future as new releases are added by the AWS team. This is a layer that the AWS team has curated for your usage, removing and optimizing the components, thereby reducing the package size.

![](img/ec1933a597ce63afa3bcf7496364f412_110_0.png)

Figure 3.12: awswrangler Lambda installation – the managed layer options for Python 3.9

The usage of AWS SDK for pandas from an AWS managed layer is the simplest and easiest option if you want to use a package without any custom modifications of your own. Also, note that there might be a delay of up to one week or more before the new features are released or made available in the AWS managed layer. Now, you can also create the Lambda function with different Python versions and different architecture and still utilize awswrangler for that version. The following screenshot shows the awswrangler layer available in the Lambda function created with Python 3.8 and ARM architecture.

![](img/ec1933a597ce63afa3bcf7496364f412_110_1.png)

Figure 3.13: awswrangler Lambda installation – the managed layer options for Python 3.8

## Custom installation – creating a custom managed layer

There is also a situation where you will want to use AWS SDK for pandas with some modifications and custom code added to suit your needs. We will upload the .zip archive file into a custom layer and then use it in the Lambda function. There is a Lambda service limit (https://docs.aws.amazon.com/Lambda/latest/dg/gettingstarted-limits.html#function-configuration-deployment-and-execution) – the entire Lambda deployment package size should be less than 250 MB unzipped (50 MB zipped).

As shown in the following screenshots, if we archive the contents of the awswrangler library, it is around 86 MB zipped without including the actual Lambda code. This exceeds the limit of the Lambda deployment package size:

```
(awspandas) $ls
aws-sdk-pandas  awspandas
(awspandas) $cd awspandas/lib/python3.9/site-packages
(awspandas) $zip -r ../../../.../awswrangler.zip .
    adding: pandas-1.5.0.dist-info/ (stored 0%)
    adding: pandas-1.5.0.dist-info/RECORD (deflated 69%)
```

Figure 3.14: awswrangler Lambda installation – creating a custom Lambda package

```
(awspandas) $ls -lh
total 197032
drwxr-xr-x  29 ssm sun  staff   928B Sep 28 14:30 aws-sdk-pandas
drwxr-xr-x   8 ssm sun  staff   256B Sep 28 14:48 awspandas
-rw-r--r--   1 ssm sun  staff    86M Sep 28 16:23 awswrangler.zip
```

Figure 3.15: awswrangler Lambda installation – verify the created Lambda package

So, we must build the Lambda package in an optimal manner to upload it into a Lambda function. Luckily, we have a script that can help us perform exactly that activity. Follow the following steps to get a ZIP file that is less than 50 MB to upload to the Lambda function.

Here are the prerequisites:

- We will utilize the awswrangler local repository that we utilized for the local machine/EC2 instance setup. Remember the changes that we made in the __init__.py for message and description variables? We will print them out in the Lambda function to verify our installation.
- A Docker daemon needs to run on your machine, which we will use to build the Lambda layer. Also, note that Docker should have enough memory allocated to build code to avoid any out-of-memory errors during the process. Note that the following screenshot allocates 8 GB of memory for Docker to utilize, so regardless of your physical machine capacity, Docker needs to have enough memory and compute to complete the build successfully.

![](img/ec1933a597ce63afa3bcf7496364f412_112_0.png)

Figure 3.16: awswrangler custom Lambda installation – container memory configuration

We will now get started, as follows:

- 1. Go to the source code path and execute the command to build Lambda layers that can be added to AWS Lambda functions. Now, execute the `sh build-Lambda-layers.sh` command to kickstart the `awswrangler` custom build.

```
(awspandas) $ls
build-docs.sh      build-wheel.sh      publish.sh
build-lambda-layers.sh  lambda
(awspandas) $sh build-lambda-layers.sh
++ python -c 'import awswrangler as wr; print(wr.__version__)'
+ VERSION=2.17.0
```

Figure 3.17: awswrangler custom Lambda installation – building the Lambda layer

### NOTE

*This step will take a lot of time to get completed. This code will build Lambda layers for the Python 3.7, Python 3.8, and Python 3.9 distributions.*

Open the code for `build-Lambda-layers.sh`, and comment out the Python 3.7 and Python 3.8 lines in the build process if you plan to deploy this library with the Python 3.9 environment only.

```
#  # Python 3.7
#  docker run \
#    --volume "$DIR_NAME":/aws-data-wrangler/ \
##    --workdir /aws-data-wrangler/building/lambda \
#    --rm \
#    awswrangler-build-py37 \
#    build-lambda-layer.sh "${VERSION}-py3.7" "ninja-build"
#fi

# Python 3.8
#docker run \
# --volume "$DIR_NAME":/aws-data-wrangler/ \
# --workdir /aws-data-wrangler/building/lambda \
# --rm \
# awswrangler-build-py38 \
# build-lambda-layer.sh "${VERSION}-py3.8${ARCH_SUFFIX}" "ninja-build"

# Python 3.9
docker run \
  --volume "$DIR_NAME":/aws-data-wrangler/ \
  --workdir /aws-data-wrangler/building/lambda \
  --rm \
  awswrangler-build-py39 \
  build-lambda-layer.sh "${VERSION}-py3.9${ARCH_SUFFIX}" "ninja-build"
```

Figure 3.18: awswrangler custom Lambda installation – Python 3.9

The preceding screenshot shows the commented code segments for Python 3.7 and Python 3.8 highlighted.

- 2. Once the build is complete, you should see zipped files created for three Python versions, 3.7, 3.8, and 3.9. Remember to change the build function to the specific Python version or versions that your Lambda applications use. You can see that the zipped files are now around ~44-46 MB in size.

```
(awspandas) $ls -lh ../dist
total 279048
-rw-r--r--   1 ssmsun  staff   246K Sep 28 22:17 awswrangler-2.17.1-py3-none-any.whl
-rw-r--r--   1 ssmsun  staff    44M Sep 30 11:41 awswrangler-layer-2.17.0-py3.7.zip
-rw-r--r--   1 ssmsun  staff    46M Sep 30 13:33 awswrangler-layer-2.17.0-py3.8.zip
-rw-r--r--@  1 ssmsun  staff    46M Sep 30 16:06 awswrangler-layer-2.17.0-py3.9.zip
```

Figure 3.19: awswrangler custom Lambda installation – verifying the created Lambda layers

- 3. Create a Lambda function with Lambda version 3.9, and then create a custom layer in the Lambda console.
