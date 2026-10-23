Now that we have defined a label that our algorithm has to predict, let’s look into what features we can use. Remember that *features* in an ML problem are the parameters that the model will look at to discriminate between classes. These can be an attribute of the user (for example, demographics) or its interaction with your product (for example, the number of uses of a specific service in the last month).

What we have just described has the form of a *supervised learning* problem: an ML algorithm is asked to learn a mapping between a set of features (customer characteristics) and a label (churned / not churned) based on historical data. Let’s recap the necessary steps to solve it, as visualized in figure 3.2:

+   1.  Define an ML task starting from a business one (identifying customers who are likely to leave our service).
+   2.  Clearly identify a label: churned or not churned.
+   3.  Identify the features: elements of a customer that are likely to influence the likelihood of churning. You can come up with possible examples by thinking about what you would look at if you had to do this job by yourself:
+       - Age
+       - How long the customer has used the service
+       - Money spent on the service
+       - Time spent using the service in the last two months
+   4.  Gather historical data of churned and active customers.
+   5.  Train the model: the ML model will learn how to predict the label, given the features.
+   6.  Perform inference: use the model on new data to identify which of your current customers are likely to churn.

Notice that the label must be found retroactively by looking at past customer records. Let’s consider the easiest situation first. Assume you have a subscription-based business model, like Netflix or Spotify. Subscriptions are usually renewed automatically, so

![](bbox=[0.15, 0.65, 0.85, 0.95])