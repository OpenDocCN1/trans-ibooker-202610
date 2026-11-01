![](bbox=[0.12, 0.13, 0.88, 0.56])

Figure 2.66: Data quality check information after fixing the issue

Throughout this section, we have gained an understanding of how Glue DataBrew can assist in recognizing and resolving data quality concerns. Although the techniques presented were manual, it’s important to note that in practice, it’s preferable to implement an event-driven data quality check. This approach involves automatically identifying any data quality issues as they arise, such as the absence of a valid Social Security Number or an employee ID, and taking appropriate measures, such as alerting the business or application owner, allowing them to validate the data and implement necessary fixes.

# Event-driven data quality check using Glue DataBrew

In this architecture, every time a file is uploaded to Amazon S3, an EventBridge rule triggers a Step Functions state machine.