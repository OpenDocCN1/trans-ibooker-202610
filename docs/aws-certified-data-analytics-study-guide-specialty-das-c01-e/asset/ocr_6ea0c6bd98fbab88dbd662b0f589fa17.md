management. While there would be cases and situations where you would need to alter the pipeline based on your need, in most cases this is the sort of pipeline the customers would typically build to solve their analytical problems. The technologies might change, but they key idea remains the same.

# FIGURE 1.2 Overview of an analytics pipeline

![](bbox=[0.13, 0.25, 0.875, 0.38])

The general flow of a big data pipeline is as follows:

+   1. Data is collected by a collection tool.
+   2. Data is stored in a persistent way.
+   3. Data is processed and analyzed. Often this processing involves multiple steps, and there is typically movement of data between the processing system and the storage system. The result of an analysis can warrant another analysis and processing.
+   4. Once the processing is done, the data is made available to the business users who can then use the BI tools to visualize and generate further insights from the data.
+   5. The data is also often used for predictive analytics. Examples are churn prediction, customer segmentation, and product recommendation.

Let’s look at each one of these phases in more detail.

# Data Sources

In any analytics pipeline, you will start your work with data being produced from producer systems. The system producing data may be an OLTP system like your customer relationship management (CRM) system or your retail point of sale system running on MySQL, Postgres, Aurora, or another commercial database, or it may be your existing data warehouse, like Teradata, Oracle, or Netezza. The producers are typically not limited to relational data sources as they can often include sensors and devices on shops and factory floors producing event data like temperature, humidity, noise, and heat levels or even social media feeds that can be captured from Twitter and Facebook. The data may therefore be in various formats and need to be captured at a varying velocity, variety, and volume. These data sources are what produce the data, and quite often the analytics pipeline’s key objective is to transform this raw data into information that can be consumed by business users.

This leads us to the collection part of the big data pipeline.