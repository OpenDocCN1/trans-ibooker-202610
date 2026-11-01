![](bbox=[0.12, 0.08, 0.88, 0.41])

Figure 3.16: awswrangler custom Lambda installation – container memory configuration

We will now get started, as follows:

+   1. Go to the source code path and execute the command to build Lambda layers that can be added to AWS Lambda functions. Now, execute the `sh build-Lambda-layers.sh` command to kickstart the `awswrangler` custom build.

```
(awspandas) $ls
build-docs.sh      build-wheel.sh      publish.sh
build-lambda-layers.sh  lambda
(awspandas) $sh build-lambda-layers.sh
++ python -c 'import awswrangler as wr; print(wr.__version__)'
+ VERSION=2.17.0
```

Figure 3.17: awswrangler custom Lambda installation – building the Lambda layer

# NOTE

*This step will take a lot of time to get completed. This code will build Lambda layers for the Python 3.7, Python 3.8, and Python 3.9 distributions.*

Open the code for `build-Lambda-layers.sh`, and comment out the Python 3.7 and Python 3.8 lines in the build process if you plan to deploy this library with the Python 3.9 environment only.