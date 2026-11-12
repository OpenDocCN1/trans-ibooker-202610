# Utilizing an ML service

Next, we pass this data into an ML service—Microsoft’s Azure ML Studio, for example. We want to train a classification model using our historical data. Or, to put it in simple terms, we want the model to find out what it is about customers that makes them likely to make a purchase. We know lots about our customers; there may be combinations of these aspects (or “features” as they’re called in DSML) that make a particular customer likely to buy something.

For example, maybe male contacts with a lifetime value (LTV) of over $34,000 who have not made a purchase in the last six months, and who have moved from a more affluent area to a less affluent area in the last three years, are more likely to make a purchase than other contacts. There are millions of customers in our dataset, and there are hundreds of features (organization, gender, location, demographics, etc.), so it’s way beyond the capability of humans to determine exactly what it is that leads a customer to make a purchase. There might be billions of potential combinations of features.

ML has no such issues with the scale of a challenge like this, and depending on the available compute resources, it can often determine results in minutes.

# Training and testing a model

We tell our ML model that we want to train it to predict the value of our “sold” column. It iterates through our dataset, testing millions of feature combinations to determine which combinations are associated with an increased likelihood to purchase. These features are assigned weights according to how much they contribute to the likelihood, and the model is run again, each time predicting what the value of the “sold” column is with either a 1 or a 0. The model’s efficacy can be tested by comparing the predicted results with the actual results stored in the historical data, and from this the overall accuracy of our model is scored.