+   - **SP3**: Documentation reports that a single storage account may handle a throughput of 10 **gigabytes per second (GBps)**, meaning about 1.25 GB/second largely above the required throughput.
+   - **SP5**: They are in line with the obligation to process everything within the **European Union (EU)** boundaries.

Although the solution seems more appropriate, they identified a trade-off point (**TP1**): while the upload channel approach has a positive impact over several sensitivity points, it has a negative impact on the usability quality attribute (**SP4**) because customers traditionally use SFTP or FTPS to transfer files.

So, here again, we end up with a trade-off. In reality, you are likely to end up with several trade-off points and residual risks.

Let's now wrap up the use case.

# Use case wrap-up

We have to wrap up our use case here, but as you can imagine, similar yet different approaches should go through an analysis, and once all the trade-off points are identified and evaluated, the best approach should be chosen. Possible alternative approaches could have been these:

+   - A combination of a cloud-based data center and an on-premises data center
+   - An active-active cloud-based setup with different storage accounts and different Databricks clusters
+   - Launching a **request for proposal (RFP)** to let suppliers submit proposals

We hope that you understood the different mechanisms consisting of analyzing the business requirements, identifying some quality goals, writing business scenarios, and extracting sensitivity points, trade-off points, risks, and non-risks. The objective is to iterate a few times and review multiple options before choosing the target solution. You are likely to end up with residual risks that should be reported to the business.

We have not focused on code-related NFRs such as maintainability, readability, and upgradability because most critical NFRs lie within infrastructure and security and because we will focus only on the code in *Chapter 5, Design Patterns and Clean Architecture.*

Let's now see how to deal with ATAM in an agile-driven organization.