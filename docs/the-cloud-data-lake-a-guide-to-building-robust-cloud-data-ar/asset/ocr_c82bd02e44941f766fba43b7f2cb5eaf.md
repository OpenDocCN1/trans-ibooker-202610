+   - Data science and machine learning solutions that are leveraged by data scientists for ad hoc exploratory analysis
+   - A cloud data warehouse that serves this high-value data to BI use cases and data analysts

![](bbox=[0.143, 0.185, 0.858, 0.547])

*Figure 3-3. Proposed cloud data lake architecture for Klodars Corporation*

The deliverable for their project consists of the following phases:

# *Phase 1: Ingestion*

Load data into the data lake. Set up automated pipelines to take daily backups of the operational databases and CRM system in the data lake. Store data for the past 90 days.

# *Phase 2: Processing*

This consists of two phases that can run in parallel:

# *Process BI data*

Run processing pipelines to refresh data daily into the cloud data warehouse.
Validate with feedback from data analysts on sales and marketing teams.

# *Advanced scenarios*

Develop product recommendation models based on data from the operational databases and CRM system, leveraging data scientists.