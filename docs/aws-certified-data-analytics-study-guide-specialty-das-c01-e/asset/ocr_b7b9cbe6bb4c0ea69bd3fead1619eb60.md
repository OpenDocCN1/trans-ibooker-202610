# Logging and Monitoring

Amazon QuickSight is integrated with AWS CloudTrail, which means all calls to the AWS Console and AWS QuickSight API are recorded.

Amazon CloudWatch is not natively supported at the time of this writing.

# Security Best Practices

The following are security best practices for Amazon QuickSight:

+   - Access to Amazon QuickSight should be using HTTPS or WebSockets secure (wss://).
+   - Access to data sources outside of AWS means that you will need to allow Amazon QuickSight’s IP address range to be able to connect to those sources.
+   - Always use SSL to connect to your databases.
+   - Wherever possible, try to use enhanced security features provided by the Amazon QuickSight Enterprise edition.
+   - (Enterprise edition users only) Use VPC connectivity wherever possible to secure your communication to AWS data sources. Use DirectConnect to connect to your on-premises resources for a secure private connection.

# Amazon DynamoDB Security

We looked at DynamoDB in Chapter 3, “Data Storage.” Let’s look at the Amazon DynamoDB security model in the next section.

# Access Management in DynamoDB

You can use IAM to grant access to Amazon DynamoDB resources and API actions. To do this, first write an IAM policy, which is a document that explicitly lists the permissions you want to grant. Then attach that policy to an IAM user, group, or role. For example, an IAM user named Joe could create an Amazon DynamoDB table and then write an IAM policy to allow read-only access to this table. Joe could then apply that policy to selected IAM users, groups, or roles in his AWS account. These recipients would then have read-only access to Joe’s table. In addition to controlling access at the resource level using IAM, you can control access at the database level using fine-grained access controls. You can create permissions that allow or deny access to specific items (rows) and attributes (columns) in your database.