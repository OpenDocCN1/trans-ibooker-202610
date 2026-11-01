After completing the data profile job and returning to the **Data quality rules** section, select the `raw-human-resource` dataset to view the 53 generated data quality recommendations in the AWS Glue DataBrew console. While not all recommendations may be useful, it’s advisable to review all of them and add relevant ones to the ruleset.

From the recommendations list, the following data quality recommendations were selected and added to the ruleset:

+   1. Check for duplicate rows in the dataset.
+   2. Ensure that all columns have no missing values (0% missing values).
+   3. Verify that the `Age in Yrs.` column contains values between 21 and 60.
+   4. Ensure that the `Quarter of Joining` column only contains values from the list `["Q3" ,"Q2" ,"Q1" ,"Q4"]`.
+   5. Verify that the `SSN` column has a length of 11 characters.
+   6. Ensure that the `Phone No.` column has a length of 12 characters.

These recommendations were provided as part of the data quality rule suggestions. To create custom data quality rules using the Glue DataBrew console, read on.

In order to create custom data quality rules using the Glue DataBrew console, we must first define what rules we want to build. Here are some rules we want to create:

+   1. **Validate the row count**: We have downloaded the dataset of 5 million records. Let’s validate whether the count matches.
+   2. **The employee ID, email address, and SSN should be unique**: These values should always be unique.
+   3. **The employee ID and email address should not be null**: Usually, we don’t want these values to be null.
+   4. **The employee ID and employee age in years shouldn’t have negative values**: We want to validate that the values in these columns are positive.
+   5. **The SSN data format should be in the format (xxx-xx-xxxx).**

Let’s start building the data quality rules:

+   1. Validate the row count.

To check the total row count, let’s perform the following steps:

+   1. Create a new rule.
+   2. Name the rule (e.g., `Row count == 5 million`).
+   3. Choose **Individual check for each column** for the data quality check scope.
+   4. Select **All data quality checks are met (AND)** as the rule success criteria.
+   5. Choose **Number of rows** as the data quality check.
+   6. Select **Is equals** as the condition.
+   7. Enter **5000000** as the value.