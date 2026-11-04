+   - Tagging information and metadata that helps with cost allocation
+   - The region and service of the charge

![](bbox=[0.14, 0.14, 0.84, 0.42])

Figure 5-1. Sample line from the AWS CUR billing data

All this granularity gives a FinOps team incredible power as the practice is built out. But granularity creates complexity, and very quickly billing data gets so large that it can't be managed with spreadsheets. Any material amount of cloud spend will generate a massive volume of charges: a sizable cloud spender may have billions of individual charge line items per month with hundreds of columns of meta detail attached to each. The more cloud native you are, the bigger the data set: serverless functions, ephemeral instance use, per-second billing data, and so forth are driving a big data problem for the FinOps practitioner. You must turn to big data analysis techniques for help.

However, you cannot simply throw a data analyst at the raw billing data. None of the existing financial systems used for managing traditional IT spend can adequately handle the volume of data without summarization, nor do they provide data freshness at the near-real-time speed required for FinOps to create a feedback loop.

Each cloud has different levels of granularity and detail, as well as different terms for their dimensions and metrics. Each of the various commitment-based discounts are represented differently in the way they are applied. A skill set is required on the FinOps team to fill the data engineering role (as described in Chapter 3).

Handling the data should be a key consideration when deciding between building in-house tooling versus buying a vendor platform, which is discussed in Chapter 8. The vendors abstract away the problem of keeping your data up to date as you add