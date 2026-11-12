versions below 3.6 have been deprecated. From Spark 3.2 onwards, Python 3.6 support has been deprecated too.

Over the last 4 years, the data science ecosystem has also changed at a rapid pace. There is an increased focus on putting machine learning models in production. Deep learning has provided remarkable results and Spark team is currently experimenting to allow the project’s scheduler to leverage accelerators such as GPUs.

# PySpark Addresses Challenges of Data Science

For a system that seeks to enable complex analytics on huge data to be successful, it needs to be informed by—or at least not conflict with— some fundamental challenges faced by data scientists.

+   First, the vast majority of work that goes into conducting successful analyses lies in preprocessing data. Data is messy, and cleansing, munging, fusing, mushing, and many other verbs are prerequisites to doing anything useful with it.

+   Second, *iteration* is a fundamental part of data science. Modeling and analysis typically require multiple passes over the same data. Popular optimization procedures like stochastic gradient descent involve repeated scans over their inputs to reach convergence. Iteration also matters within the data scientist’s own workflow. Choosing the right features, picking the right algorithms, running the right significance tests, and finding the right hyperparameters all require experimentation.

+   Third, the task isn’t over when a well-performing model has been built. The point of data science is to make data useful to non–data scientists. Uses of data recommendation engines and real-time fraud detection systems culminate in data applications. In such systems,