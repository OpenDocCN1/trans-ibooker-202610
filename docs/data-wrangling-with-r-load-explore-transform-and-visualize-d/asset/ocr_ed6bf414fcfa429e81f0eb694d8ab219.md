5. Specify the Amazon S3 path where you want to store your data profile information as JSON in the job output settings.

6. Enable PII statistics under Dataset level configurations, and select All categories under PII categories. Note that in production, you may want to choose what is relevant to your data.

![](bbox=[0.12, 0.16, 0.88, 0.47])

Figure 2.70: PII configuration in AWS Glue DataBrew

7. Keep the rest of the options as the defaults.

8. Select the role name that we created during the data quality rule.

![](bbox=[0.12, 0.55, 0.88, 0.73])

Figure 2.71: Added role in the Glue DataBrew job

9. Wait for the profile job to complete.

Once the job is complete, you will be able to see the stats in the Data Profile overview section in Datasets. For our dataset, it shows that 17 columns are PII.