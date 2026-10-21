with performance metrics, a data scientist typically needs to calculate bias metrics offline, which heightens the risk of errors (because they are not using production data). Moreover, bias issues can be correlated to other issues; for example, a spike in the number of male job applicants could cause bias in job candidate matching. Investigating causes of bias in a silo can cause data scientists to overlook such correlations, resulting in them spending time unnecessarily on debugging issues that could have been easily corrected.

# MPM detects training/serving skew

Features used to train and serve the models in online and offline environments can differ. When this happens, the model will behave differently and needs to be debugged. By keeping the training data and prediction log in one place, MPM enables developers to detect these skews.

# MPM explains past predictions

Imagine a bank is using ML to approve loans, and a customer files a complaint about a particular loan being denied. Because MPM tracks a model’s behavior from training to serving, a risk operations officer can go back in time to reproduce the prediction along with the explanation (this is known as “prediction time travel”).

In the next chapter, we’ll discuss how Explainable AI enhances the benefits of MPM even further.