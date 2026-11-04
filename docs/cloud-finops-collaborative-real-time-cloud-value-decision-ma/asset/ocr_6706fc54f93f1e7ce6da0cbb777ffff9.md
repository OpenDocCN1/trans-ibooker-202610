Using business drivers to correlate revenue to increases in AWS cost, he was able to adjust cloud budgets. He worked with the CTO office to get support and then reached out to the approximately 90 account owners to meet every two weeks to review forecasts. Attendance is high, and they focus on the main drivers of variance. The account owners are aware of the forecasting formulas used. They have been able to achieve 82%–85% forecast accuracy, which is sufficient for Finance but still leaves room for improvement over time.

His takeaways: “It really helps to see trends and map events to specific months. You need good training around tagging, e.g., when hiring engineers they need to know about tagging and how important it is. Culture is hard to change; you need to set expectations at the beginning.”

# Cost Estimation

Predicting the cost of a future deployment can be very difficult for engineering teams, especially if there is no existing proof-of-concept work or vendor-supplied calculations for the application deployment.

Cost calculators made available by all the major cloud service providers help get a ballpark value for the new workload, but these tools are only able to estimate the costs of the items you manually enter into the calculator. Due to the complexity of cloud deployments and the granularity of costs, it’s easy for your engineers to miss entering items or underestimate the size/volume of the resources needed to operate their new workload.

Comparing existing workloads that are similar in design to the new workload being introduced can help teams estimate the future cost of the new workload. Have your engineers compare the design of the new workload being introduced to existing workloads to identify if you have any analogous systems. Where possible, using existing workloads will lead to identifying cost items that could be missed when using cost calculators. If there is no existing system, use the variable cost model of the cloud to your advantage; have your teams deploy the new workload in a test environment and operate it for an hour or so. After this test you can then identify the costs associated with the test run of the workload.

Some of the most difficult costs for your engineers to estimate prior to deployment include the following:

+   - Network traffic costs
+     - Data transfers out of region
+     - Cross-zone transfers
+     - Volume of traffic handled by individual resources like load balancers