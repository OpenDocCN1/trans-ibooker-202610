![](bbox=[0.125, 0.09, 0.875, 0.53])

Figure 10.5: Data Wrangler data sources

In this chapter, we will use a publicly available example, the Titanic dataset. The Titanic dataset is considered the “Hello World” of machine learning datasets due to the number of commonly used data processing and machine learning techniques that are possible with this example dataset. We will use a slightly changed version of the Titanic dataset. You can learn more about the dataset here: https://www.openml.org/search?type=data&sort=runs&id=40945&status=active. The dataset is available via a public S3 bucket. To use this dataset with Data Wrangler, we can either upload it to an S3 bucket in our AWS account, or we can directly import the dataset directly into Data Wrangler via the Use sample data option at the top right of the Data sources section. In this chapter, we will use the latter option.

![](bbox=[0.125, 0.78, 0.875, 0.82])

Figure 10.6: Data Wrangler sample data

After you click on Use sample data, a Data Wrangler connection is created in the S3 bucket hosting the dataset, and a sample subset of this dataset is loaded into Data Wrangler. This also creates the