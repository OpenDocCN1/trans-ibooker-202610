+   - Number of events
+     - Function as a Service executions
+     - Messages sent/received on queues
+     - Number of database queries
+   - Activity of data storage
+     - How much data can be transitioned to less frequently accessed tiers
+     - Count of requests for the individual objects

In the vendor and open source space we are starting to see new tools like Infracost, which enables your engineering teams to see an estimate of the cost impact of their changes before they roll them out to the cloud platform. This kind of shifting left provides your teams the ability to check that their changes will have the expected impacts to costs.

Whether it's before your engineers deploy new infrastructure or before they deploy an update to existing infrastructure, frequently updating forecasts and the communication of project work means your forecasts will be refined as your engineers progress with their projects. Collaboration between engineering teams enables you to capture feedback on what is changing in forecasts as the updates roll in.

# Impacts of Cost Optimization on Forecasts

Just as business decisions and new deployments can drive up cloud spend, future cloud cost optimizations can greatly reduce spending, also making your forecasts inaccurate. The chapters in Part III cover these optimizations in more detail. For now, we want you to understand that just like project work, which contributes to increased cloud spend, you need to generate predicted efficiency calculations that represent the reduction in cloud spending driven through FinOps activities and subtract them from your forecasts.

In the early stages of implementing FinOps, there can be a drastic reduction in cloud spending as you introduce usage and rate optimizations. Without predicting these reductions within your forecasts, you may end up forecasting above actuals.

When you introduce FinOps optimizations, the reduction of your operating costs may hide the natural growth rate of your cloud spend. As these optimizations stabilize, the normal growth rate of spending will return; it's important to model this when tuning your forecasts so the initial reductions don't cause you to underestimate things in the future.

An example of how FinOps optimizations can impact forecasts when not taken into account can be seen in Figure 13-5. With FinOps activities starting in 2022-05, you see a rapid reduction in cloud spend. After a few months, in 2022-09, the natural