# **FIGURE 6.9**  S3 encryption options

![](bbox=[0.152, 0.137, 0.858, 0.497])

Amazon S3 provides a number of tools for monitoring your Amazon S3 resources for potential incidents. The key tools are as follows:

**Amazon CloudWatch Alarms**  Amazon CloudWatch allows you to monitor metrics over a time period that you specify. Exceeding a metric threshold can result in notification to Amazon SNS or an autoscaling policy.

**AWS CloudTrail Logs**  Auditing is an essential component of a system for various compliance reasons. AWS CloudTrail enables governance, compliance, operational auditing, and risk auditing of your AWS account and provides event history of your AWS account activity, including actions through the AWS Management Console, AWS SDKs, command-line tools, and other AWS services. This history simplifies security analysis, resource changes tracking, and trouble shooting. You can use the information from AWS CloudTrail to determine the request that was made to Amazon S3, the IP address from which the request was made, who made the request, when it was made, and additional details. Understanding the usage of this service is also important from an exam perspective as you will see certain auditing questions.

**AWS S3 Access Logs**  Server access logging is another useful feature that allows you to track the requests that are made to an Amazon S3 bucket. Server access logs are useful for security auditing and helping customers understand Amazon S3 billing where