# Built-in data insights

AWS Data Wrangler provides several commonly used data analysis tools out of the box. In this section, we will look at some of these. You can go to the **Data Analysis** section by clicking on the + button next to the data source icon and choosing **Get Data Insights**.

![](bbox=[0.125, 0.205, 0.875, 0.565])

Figure 10.10: Data Wrangler data insights

Let’s create a new analysis report:

+   1. Choose **Data Quality And Insights Report**.
+   2. Choose your target column or the column that your machine learning model will try to predict.
+   3. Here, we also choose the type of machine learning problem we will try to solve, whether it is a classification or a regression problem. In our case, we are trying to predict whether a passenger survived the *Titanic* tragedy. We will change the target column to **Survived**, set the problem type to **Classification**, and click on the **Create** button. Data Wrangler will take a couple of minutes to generate the report. This is, of course, dependent on your dataset and sample size.