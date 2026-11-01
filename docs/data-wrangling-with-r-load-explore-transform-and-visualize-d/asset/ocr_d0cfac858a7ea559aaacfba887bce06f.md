+   - Amazon S3 server logs are used to capture logs in fine-grained detail (life cycle transition events, authentication failures, and so on). These logs have additional fields for object size, HTTP referrers, and more. There is no additional cost except for the storage cost of the logs.
+   
+   - AWS CloudTrail events (S3 API calls) capture object and bucket API operations through CloudTrail. The logs can be delivered to multiple locations and forwarded to Amazon CloudWatch for further analysis. There is an additional cost for CloudTrail event capture in addition to log storage costs, but it has better integration capabilities with other systems.

Refer to the following documentation page to learn the difference between AWS CloudTrail and Amazon S3 server logs: https://docs.aws.amazon.com/AmazonS3/latest/userguide/logging-with-S3.html.

+   - Amazon S3 Inventory helps you export CSV/ORC/Parquet format files with a list of objects in an S3 bucket or prefix in a periodic manner. This report can be used to analyze the replication and encryption status of objects to ensure the data is secure. The inventory file can be queried with Amazon Athena to identify regulatory or compliance issues in the S3 bucket using SQL queries.

# Monitoring, discovering, and identifying security issues

The following services can help you monitor, discover, and identify security issues in your data lake:

+   - AWS Config helps users continuously monitor AWS resources and audit configuration changes. Users can configure rules on AWS Config to monitor and tag resources that don’t adhere to the configured rules. You can set up AWS Config to validate public read/write configuration for all buckets within your organization. Refer to the following blog post to learn how to use AWS Config to identify and update public settings for S3 buckets in your account: https://aws.amazon.com/blogs/security/how-to-use-aws-config-to-monitor-for-and-respond-to-amazon-s3-buckets-allowing-public-access/.

+   - AWS Trusted Advisor scans the AWS infrastructure on your accounts and provides recommendations that follow best practices. It can be used to identify any issues in your data lake configurations and fix them appropriately. This is available to AWS Enterprise support customers. Trusted Advisor provides optimized recommendations that help with the following:
+     - Cost optimization: Identify unallocated, underutilized, and idle AWS resources to help users optimize costs.
+     - Performance: Provides recommendations to improve performance by analyzing the throughput, latency, and compute usage of AWS resources.
+     - Security: Identify and provide recommendations on exposed credentials, security group vulnerabilities, S3 bucket permissions that are more open, and more.
+     - Fault tolerance: Identify and highlight EC2 autoscaling, health checks, data backups, and more.
+     - Service limits: AWS has service limits for different services. They highlight when the user has hit 80% of the service limit so that they can be better prepared.

+   - Amazon Macie identifies sensitive data by using machine learning pattern matching in your S3 buckets. Macie also supports automated sensitive data discovery on S3 buckets. Refer to the following post for an automated, sensitive data discovery framework that can be used across multiple accounts using Amazon Macie: https://aws.amazon.com/blogs/security/use-amazon-macie-for-automatic-continual-and-cost-effective-discovery-of-sensitive-data-in-s3/.

+   - AWS GuardDuty is a threat detection service that continuously monitors your AWS account for suspicious activities. You can use GuardDuty to monitor S3 management events and data events so that you can identify security issues. A report will be