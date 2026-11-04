# Forecasting Models

Now let’s consider the tools or models you might use to generate a forecast. We won’t go into details about different algorithms or machine learning models that generate forecasts. The aim of this chapter is to assist with the way you think about your cloud forecasts and consider techniques to help you work with cloud spend forecasts within FinOps. If learning specifics on forecasting interests you, there are many good books that cover forecasting such as Forecasting: Principles and Practice by Rob J. Hyndman and George Athanasopoulos (OTexts, 2021). Forecasting models include:

# Machine learning forecasts

This is a term that is often overused to describe forecast technologies. Some people consider only deep learning models to be truly machine learning forecasts; others include statistical and other nondeep learning models in this category. No matter which model you choose to use, we recommend you shop around. Don’t be afraid to try another model. The first one you use is unlikely to be the best. Also consider evaluating multiple models against history to see which ones would have been most accurate.

# Composite forecasts

This is an approach that combines different forecasts to achieve more accurate predictions using more than one model. A simple method for combining is to average the results given from multiple models.

# Static forecast

This type of forecast is generated once at the beginning of a set period, such as 12 months. It is not updated until the period ends, when a new forecast is generated for the next period. This type of forecasting is good for very predictable spending.

# Rolling forecast

This type of forecast is also generated at the beginning of a set period, such as 12 months. However, unlike static forecasts, it is regenerated frequently (weekly, monthly, quarterly) during the period. Each time it is regenerated it is created for the same length of time, effectively adding more data to the end of the forecast horizon.

# Cloud Forecasting Challenges

To generate more accurate forecasts, you must first work through a set of common challenges. You are likely working on some of these items already, as the capabilities needed to solve for forecasting relate to many other items in the FinOps Foundation Framework covered in Chapter 7. Depending on the structure of your organization