the dataset. You can change this sampling method to **random** or **stratified**. You can also set the sample size here.

![](bbox=[0.12, 0.15, 0.88, 0.43])

Figure 10.9: Editing the data types and source

A key tip to remember here is that, if you are planning to use Data Wrangler for your model training, you will need to re-import the data if you use sampling here. We will cover model training in the latter part of this chapter. In the next step, we will do some exploratory data analysis to understand our data better.

# Exploratory data analysis

Before we do any data transformation or manipulations, we need to get a good understanding of our data. **Exploratory data analysis (EDA)** is a crucial step in data science because it allows us to understand the structure and characteristics of the data we’re working with. EDA involves the use of various techniques and tools to summarize and visualize data in order to identify patterns, trends, and relationships. It is also important that we perform this step before we do any data transformations or modeling because EDA can help us understand which features are relevant and which are most important for the machine learning problem we are trying to solve. EDA can help you understand the distribution of data and identify any relationships that exist between the features in your dataset. When working with real-world data, you will inevitably encounter data quality issues such as missing data, imbalance in various classes, errors in data collection, and outliers. EDA helps to identify these data quality issues, which can have a detrimental impact on the quality of your machine learning models. Data Wrangler provides commonly used EDA tools via a built-in data analysis report, and you also can generate your own custom data analysis.