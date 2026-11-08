
# Retraining
Retraining is achieved by triggering the model development pipeline (data preparation, training, validation, and so on). The trigger can be initiated based on a scheduled event or after a drift indication (triggered by the model monitoring component). An important step is validating the model in a staging environment before deploying it to production. If the results are not as expected, then the models and pipelines need to be retrained.

After retraining, it is recommended to leave the old model running and deployed to production for a specific period of time or until the model has served a certain number of requests. By running these A/B tests you can identify which model performs better, by comparing its predictions with those of the other ones. Another approach is to use an ensemble with the old and new models and change the weight or remove the old model over time.

Finally, model retraining can also be used for training new models. This is called transfer learning. In transfer learning, existing models are reused to retrain new models. This is commonly used in deep learning since it saves resources by reusing models instead of rebuilding them.

## Deployment Strategies
In production, models and applications must always be ready to serve requests and they should not suffer from downtime due to version upgrades. In addition, a more advanced practice is gradually moving requests to the new version, while validating that performance and quality levels are met. Then, if the latest version fails or underperforms, you can roll back to the previous version.

To guarantee no service disruption, you can use a rolling upgrade deployment strategy. This strategy replaces the application version used by the service, one instance at a time. Rolling upgrades are supported out of the box in cloud-native platforms and Kubernetes. However, when you want to evaluate the candidate versions against the current version, you need two or more versions to coexist and serve requests, until you can determine that the new version meets your quality and performance goals. Or, you can use the other versions to serve as a baseline to compare performance with.

There are several standard model deployment and upgrade strategies (see [Figure 6-20](#ch06.xhtml#deployment-strategies)):
- *A/B Testing*: The new model is deployed alongside the old model and the traffic is divided between the two. Then, the output of both models is monitored to determine which one performs best, and the best-performing model is promoted. This method is a good fit for most use cases. In some cases, you may deploy more than two models for A/B/C testing, A/B/C/D testing, and so on.
- *Blue/Green*: Setting up two identical environments, dubbed *Blue* and *Green*, with one being the live Production environment and the other a Staging environment. The new model is deployed to the Staging environment. Once it operates as expected, Staging becomes Production, the old version is removed, and more recent models are deployed into Staging. This method is straightforward and relatively simple, but it incurs high operational costs since you have to operate two identical environments. This method is recommended for use cases that cannot afford any downtime, like fraud detection or e-commerce.
- *Canary*: Incremental rollout of the new model to a percentage of users to validate efficacy, performance, and functionality. Once validated, the model is gradually rolled out to more users and finally to everyone. This strategy enables controlled rollout with no downtime. However, it is slow, and testing occurs in Production. Therefore, it is recommended for non-critical applications that obtain value from gradual testing in Production, like social media.
- *Champion/Challenger (or Shadow)*: Deploying the Champion model to Production alongside one or more Challenger models. Traffic is routed to all models and the output of the Champion is used as the result, while the Challenger(s) output is only monitored. This strategy enables the highest form of model validation, yet it is also the most expensive.

![](assets/deployment-strategies.png)

When traffic is split across multiple models, you must ensure *affinity* (the same user is always routed to the same model and has the same experience for every request). For example, you can use a hash of the user ID to determine where to route the request.

Offline and batch application pipelines do not need to maintain running services or use rolling updates. They can select which version to use on every run. However, you may run two or more batch pipelines in parallel (in A/B or Champion/Challenger deployments), with each pipeline storing its results in different tables/files.

It’s important to remember that deployment is not a one-time process. Instead, deployment is a recurring process that occurs every time we retrain our models and want to ensure the freshest and high-performing models are being used. Therefore, selecting the right deployment strategy is important since you will use it repeatedly in your MLOps pipeline. As always, the strategy you choose depends on multiple factors. The primary considerations to take are:
- Your business use case
- Retraining and deployment frequency
- Your model size
- Whether you stream data in real-time or batches
- How your model is impacted by drift
- Your desired service uptime and quality

## Measuring the Business Impact
Measuring an application’s business impact is essential for determining the model performance. However, measuring it can be a complex task since this involves evaluating the effects of the model on various aspects of the business, such as revenue, customer satisfaction, and operational efficiency.

Here are some ways to measure the impact of ML models:
- *Define success metrics*: Define measurable and trackable metrics that are aligned with business objectives. For example, model accuracy, precision and recall, the F1 score, Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE), lift, conversion rate, and customer satisfaction.
- *Establish a baseline*: Create a benchmark for measuring the impact of the model on the business. This should be determined before deploying the model to production.
- *Conduct A/B testing*: Compare the business metrics (costs, revenue, number of active users, customer satisfaction, and so on) before and after the deployment of the ML model. This will help determine the incremental impact of the model.
- *Conduct a cost-benefit analysis*: Determine the return on investment (ROI) of the ML model. This will help assess the financial impact of the model on the business. For example, how many new users registered, what is the incremental revenue from the new service, how many calls were saved by using a chatbot, and so on.
- *Gather feedback*: Gather feedback from stakeholders, such as customers and employees, to assess the impact of the ML model on their experience. This will help determine the non-financial impact of the model on the business. You can use surveys, user testing, reviews and ratings, support ticket analysis, or NPS scores, among other methods.

Overall, measuring the business impact of ML models requires a comprehensive approach that considers both financial and non-financial metrics, as well as stakeholder feedback. By doing so, businesses can ensure that their ML models deliver tangible value and drive meaningful outcomes.

## Conclusion
In this chapter we finally delved into “the real thing”: building and deploying the application that uses the data and the model that drives decisions and action. We described the steps that will assist in delivering the ML application as a whole. By looking at this bigger picture, we can ensure there are no deficiencies in functionality, operational failures, avoidable risks, and prolonged delays.
