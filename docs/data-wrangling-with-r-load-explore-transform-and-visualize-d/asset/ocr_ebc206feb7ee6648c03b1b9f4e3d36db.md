Figure 3.11: awswrangler Lambda installation

You can then choose to add a layer; choose the AWSSDKPandas–Python39 layer, as shown in the following screenshot. The version number might change in the future as new releases are added by the AWS team. This is a layer that the AWS team has curated for your usage, removing and optimizing the components, thereby reducing the package size.

![](bbox=[0.12, 0.22, 0.88, 0.5])

Figure 3.12: awswrangler Lambda installation – the managed layer options for Python 3.9

The usage of AWS SDK for pandas from an AWS managed layer is the simplest and easiest option if you want to use a package without any custom modifications of your own. Also, note that there might be a delay of up to one week or more before the new features are released or made available in the AWS managed layer. Now, you can also create the Lambda function with different Python versions and different architecture and still utilize awswrangler for that version. The following screenshot shows the awswrangler layer available in the Lambda function created with Python 3.8 and ARM architecture.

![](bbox=[0.12, 0.7, 0.88, 0.87])

Figure 3.13: awswrangler Lambda installation – the managed layer options for Python 3.8