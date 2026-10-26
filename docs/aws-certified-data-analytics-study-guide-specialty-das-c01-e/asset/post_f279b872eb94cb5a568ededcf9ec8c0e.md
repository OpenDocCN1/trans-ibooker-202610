
1. The app calls a third-party identity provider to authenticate the user and the app. The identity provider returns a web identity token to the app.
2. The app calls AWS STS and passes the web identity token as input. AWS STS authorizes the app and gives it temporary AWS access credentials. The app is allowed to assume an IAM role and access AWS resources in accordance with the role’s security policy.
3. The app calls Amazon DynamoDB to access a table (for example). Because it has assumed a role, the app is subject to the security policy associated with that role. The policy document prevents the app from accessing data that does not belong to the user.

## How to Access Amazon DynamoDB

Amazon DynamoDB is a web service that uses HTTP and HTTPS as a transport and JavaScript Object Notation (JSON) as a message serialization format. There are three primary ways to access Amazon DynamoDB:

- Amazon DynamoDB API
- AWS Software Development Kits (SDKs)
- AWS Management Console

Applications access Amazon DynamoDB using the DynamoDB API or using AWS SDKs. Instead of making the requests to the DynamoDB API directly from your application, AWS recommends that you use AWS SDKs. The SDKs contain libraries that can take care of request authentication, serialization, and connection management. To further simplify application development, the SDKs also provide Java and .NET APIs.

Amazon DynamoDB also provides a management console that enables you to work with tables and items. You can create, update, and delete tables without writing any code. You can view all the existing items in a table or use a query to filter the items in the table. You can add new items or delete items.

Optionally, you can use the AWS Command Line Interface (CLI) for one-time operations, such as creating a table. You can also use it to embed DynamoDB operations within utility scripts.

By default, the AWS SDKs and console for Amazon DynamoDB reference the US West (Oregon) Region. As Amazon DynamoDB expands availability to new Regions, new endpoints for these Regions are also available to use in your own HTTP requests, the AWS SDKs, and the console.

## Data Protection with DynamoDB

Data protection refers to protecting data in transit and at rest. To protect data in transit, you can use Amazon DynamoDB SSL-encrypted endpoints. The encrypted endpoints are accessible from the Internet and from within Amazon EC2 instances.

Additionally, Amazon DynamoDB requires each request to the service to contain a valid HMAC-SHA256 signature, or the request is rejected. The AWS SDKs automatically sign your requests; however, if you want to write your own requests, you must provide the signature in the header of your request to Amazon DynamoDB. To calculate the signature, you must request temporary security credentials from the AWS Security Token Service. Use the temporary security credentials to sign your requests to Amazon DynamoDB.

For protecting data at rest, encrypt your data before uploading it to Amazon DynamoDB. This client-side encryption can also protect your data while in transit.

You can use SDK libraries to encrypt and sign your Amazon DynamoDB data.

For more information, see java.awsblog.com/post/TxI32GE4IG2SNS/Client-side-Encryption-for-Amazon-DynamoDB.

Amazon DynamoDB point-in-time recovery (PITR) provides automatic backups of your DynamoDB table data. Point-in-time recovery helps protect your Amazon DynamoDB tables from accidental write or delete operations. With point-in-time recovery, you don’t have to worry about creating, maintaining, or scheduling on-demand backups. Point-in-time operations don’t affect performance or API latencies.

## Monitoring and Logging with DynamoDB

AWS provides several options to monitor your Amazon DynamoDB databases. Amazon DynamoDB and Amazon CloudWatch are integrated, so you can gather and analyze performance metrics. You can monitor these metrics using the CloudWatch console, the AWS Command Line Interface, or programmatically using the CloudWatch API. CloudWatch also allows you to set alarms when you reach a specified threshold for a metric.

Amazon DynamoDB is integrated with CloudTrail, a service that captures API calls made by or on behalf of DynamoDB in your AWS account and delivers the log files to an Amazon S3 bucket that you specify. CloudTrail captures API calls made from the Amazon DynamoDB console or from the DynamoDB API. Using the information collected by CloudTrail, you can determine what request was made to DynamoDB, the source IP address from which the request was made, who made the request, and when it was made. You can store your log files in your bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically. By default, your log files are encrypted by using Amazon S3 server-side encryption.

You can have CloudTrail publish Amazon SNS notifications when new log files are delivered if you want to take quick action upon log file delivery. You can also aggregate Amazon DynamoDB log files from multiple AWS Regions and multiple AWS accounts into a single Amazon S3 bucket. You can also use the AWS Management Console to monitor the performance of your tables. Using CloudWatch metrics in the console, you can monitor table throughput and other performance metrics.

## Summary

In this chapter, we have looked at the security aspects of different analytics services and how you can access the services, protect the data at rest and in motion, and enable federated access. For most services, I have laid out some best practices. Security is one of the most important topics in the exam and your performance in this area can make or break your results. Even more important, when you run your applications in production, security is a topic that needs to be addressed on day one and hence it is important to get things right.

While we have discussed a number of different services and options, please go through the workshops listed later.

## Exam Essentials

Security is the most important component when it comes to the AWS platform, and hence data security accounts for 18 percent of all exam questions. You need to be aware of data protection mechanisms offered by various services covered under the analytics umbrella, setting up identity and access management, configuring logging of activities and monitoring the usage of the service, and ensuring that you are aware of the resiliency mechanisms.

Security is a very critical component of the AWS platform, analytics services, and the AWS Data Analytics Specialty certification exam.

## Exercises/Workshops

The following are essential workshops that you need to run on your own to prepare yourself better for the security aspects of the exam as well as your general knowledge of AWS security.

- Securing Serverless Workloads - github.com/aws-samples/aws-serverless-security-workshop/blob/master/README.md
- AWS Identity: Using Amazon Cognito for serverless consumer apps - serverless-idm.awssecworkshops.com
- AWS Security Workshops - awssecworkshops.com
- Adding Security into DevOps - devops.awssecworkshops.com
- Scaling threat detection and response in AWS - scaling-threat-detection.awssecworkshops.com

![](img/fc6a7083d65b0716921d7bd4a5298460_377_0.png)

## Appendix: Answers to Review Questions

**4.** B. A is incorrect. Migrating 50 TB of data using CLI interface for Amazon S3 on an Internet connection will be time consuming. Since the requirement is to perform this migration quickly, this solution will not work.

B is correct as the data migration with Snowball will be a better option. Once the data is in S3, crawling with Glue to build a catalog and analyzing using Athena would be preferable because the customer wanted to manage as few servers as possible, and both Athena and Glue are serverless.

C is incorrect as the data migration with Snowball will work but using Hive on EMR running Spark to build a catalog is more expensive and requires more resources and additional scripting, which can be avoided if we select option B.

D is incorrect because of the data migration with CLI interface for Amazon S3 (which is expensive) and more complex (cataloging with Glue).

**5.** C. A is incorrect as the solution will be expensive. Since only 30 TB of data is needed, loading the data into HDFS and paying the associated storage cost on the Hadoop cluster is unnecessary and is less likely to generate any benefits for the customer from a cost perspective.

B is incorrect due to the reasons mentioned in the explanation for option A. While Redshift is a good solution for cloud data warehousing, maintaining all the hot and cold data on Redshift is not a good approach and is an anti-pattern considering the earlier discussion on data lakes. The solution will work but will be more expensive to maintain. While Redshift offers cost advantages and is 10x cheaper than traditional data warehouse solutions, the customer can get better cost savings from other architectures.

C is correct and a good solution to set up a multi-temperature warehouse. This solution will not only reduce the cost in the short term, it will also be a good architecture for the long term when the data grows. The data growth would mean that the customers could start using more of S3 and use Redshift Spectrum when the join between hot and cold data is required.

D is incorrect. Elasticsearch is not a good tool for building a data warehouse where multiple access paths to access the data need to be created. 30 TB data on ES would be very expensive, and with each index, the requirement for the storage would increase manifold. Amazon Redshift is a better architecture choice for building a data warehouse.

**6.** D. A is incorrect because CSV is a row format. Using Athena to query CSV would mean reading lots of unnecessary attributes of data, which would result in lower performance and higher data scanning cost.

B is incorrect as loading the data into a VARCHAR column and then querying using JSON extraction functions would be more expensive and the join performance would be very poor.

C is incorrect as attribute extraction will perform better, but data stored on Redshift is more expensive than on S3. Furthermore, in the long run this solution will not be cost effective.
