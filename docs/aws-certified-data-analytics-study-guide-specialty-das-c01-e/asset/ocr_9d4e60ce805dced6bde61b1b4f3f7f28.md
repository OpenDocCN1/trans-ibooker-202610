data scans play an important part. You can enable server access logging on the AWS console or programmatically using the Amazon S3 API or AWS SDKs. While there is no charge for server access logging as such, any log files that are delivered to your bucket accrue the usual charges for storage.

**AWS Trusted Advisor**  AWS Trusted Advisor is an application that draws upon best practices learned from AWS’s aggregated operational history of serving hundreds of thousands of AWS customers. Trusted Advisor inspects your AWS environment and makes recommendations for saving money, improving system performance, or closing security gaps. AWS Trusted Advisor has the following S3-related checks, which are helpful from a security context:

+   - Logging configuration of Amazon S3 buckets
+   - Security checks for Amazon S3 buckets that have open access permissions
+   - Fault tolerance checks for buckets that don’t have versioning enabled or have versioning suspended

# Best Practices for Security on Amazon S3

Now that we have looked at some of the best practices for security on Amazon S3, let’s look at some of the best practices for security to protect your data:

+   - Enable account-level Block Public Access.
+   - Leverage access points to scope application permissions.
+   - Send secure traffic with VPC endpoints and access points.
+   - Use bucket policy to enforce TLS.
+   - Encrypt everything: SSE-KMS & SSE-S3.
+   - Enable object lock, versioning, MFA delete to protect data.
+   - Monitor using AWS tools, such as AWS CloudTrail and AWSConfig.

# Amazon Athena Security

We’ve looked at Amazon Athena in Chapter 4, “Data Processing and Analysis” and how it is a great tool built on Apache Presto to enable ad hoc data analysis. Since Amazon Athena will be used by data analysts and data scientists, it becomes very important to understand how it secures the data.