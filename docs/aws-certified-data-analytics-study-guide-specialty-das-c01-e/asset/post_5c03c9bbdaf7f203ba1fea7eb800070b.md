
(the AWS account who creates the resources), who has full access to the resource, no other principal can access the resource. The resource owner has the privileges to grant access permissions to others, and this is usually achieved using an access policy (a JSON document).

The access policies can be broadly categorized into two major categories, resource-based policies and user policies.

## Resource-Based Policies

These are the policies that you can attach to your resources. Examples of resource-based policies are an ACL and a bucket policy.

**ACL** Each bucket and object have an associated ACL, which is basically a list of grants identifying the permission granted and to whom it is being granted. ACLs can be used to provide basic read/write permissions to other AWS accounts and use an AWS S3–specific XML schema.

**Bucket Policy** Bucket policy is a JSON document that grants other AWS accounts or IAM users permissions to the buckets and objects contained in the bucket. Bucket policies supplement and in some cases replace the ACL policies. The following is an example of bucket policy. You can read the policy as follows:

- **Effect:** What does the policy do? [Allow/Deny]
- **Principal:** (*) is a wildcard that means everyone.
- **Action:** What actions can the listed principal(s) do? In this case, we are providing read permission on objects.
- **Resource:** Since a bucket policy is an example of a resource-based policy, you can see that we are providing the name of the resource that gets impacted by the policy.

```json
{
  "Version":"2012-10-17",
  "Statement": [
    {
      "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::analyticscertbucket/*"]
    }
  ]
}
```

## User Policies

The policies that are attached to IAM users, groups, and roles in your account are called *user policies*.

The following is an example of a user policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "FirstStatement",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:ListBucket",
                "s3:DeleteObject",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::analyticscertbucket/*",
                "arn:aws:s3:::analyticscertbucket"
            ]
        },
        {
            "Sid": "SecondStatement",
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",
            "Resource": "*"
        }
    ]
}
```

To protect resources in Amazon S3, you can use either a resource-based policy or a user policy or a combination of both. When Amazon S3 receives a request, it will evaluate all access policies to determine if the requester has the necessary permissions and based on that authorize or deny the request. At runtime, Amazon S3 will convert all relevant policies (user policies, bucket policies, ACLs) into a set of policies for evaluation, which are then evaluated based on the context in which the request was made.

**FIGURE 6.8** S3 Block Public Access settings

![](img/fc6a7083d65b0716921d7bd4a5298460_344_0.png)

These settings apply account-wide for all current and future buckets and access points. While it is highly recommended to switch on public access, it is important to check if your existing applications will continue to work with this setting switched on. To learn more about blocking public access to S3, please visit amzn.to/2Wb8W4.

## Data Protection in Amazon S3

Earlier in Chapter 3, “Data Storage,” we looked at the reliability of Amazon S3 and the fact that it provides 11 nines of durability with a design that is built to sustain concurrent loss of data in multiple facilities in addition to default object versioning. While a standard GET request will always fetch the latest version of an object, you can retrieve older versions by specifying the version number.

S3 provides encryption for data at rest and in motion. This is achieved using S3 client-side encryption and server-side encryption and applying encryption to data in transit.

### Client-Side Encryption

You can encrypt your data on the client side before you send it to Amazon S3. This can be achieved using a master key stored on AWS using AWS Key Management Service (AWS KMS) or a master key stored within your application. Amazon’s SDKs for Java, PHP, .NET, Go, Ruby, and C++ support client-side encryption.

### Server-Side Encryption

As the name indicates, server-side encryption is encryption of data at its destination, in this case Amazon S3; it essentially protects data at rest. Server-side encryption encrypts object data but not object metadata. Amazon S3 automatically encrypts your data when it is stored on the disks in AWS data centers and automatically decrypts the data when it is being accessed. The entire process is transparent to the user, and users have three mutually exclusive options for server-side encryption, with the key difference being how the keys are managed between various options (see Table 6.6).

**FIGURE 6.9** S3 encryption options

![](img/fc6a7083d65b0716921d7bd4a5298460_346_0.png)

Amazon S3 provides a number of tools for monitoring your Amazon S3 resources for potential incidents. The key tools are as follows:

**Amazon CloudWatch Alarms** Amazon CloudWatch allows you to monitor metrics over a time period that you specify. Exceeding a metric threshold can result in notification to Amazon SNS or an autoscaling policy.

**AWS CloudTrail Logs** Auditing is an essential component of a system for various compliance reasons. AWS CloudTrail enables governance, compliance, operational auditing, and risk auditing of your AWS account and provides event history of your AWS account activity, including actions through the AWS Management Console, AWS SDKs, command-line tools, and other AWS services. This history simplifies security analysis, resource changes tracking, and troubleshooting. You can use the information from AWS CloudTrail to determine the request that was made to Amazon S3, the IP address from which the request was made, who made the request, when it was made, and additional details. Understanding the usage of this service is also important from an exam perspective as you will see certain auditing questions.

**AWS S3 Access Logs** Server access logging is another useful feature that allows you to track the requests that are made to an Amazon S3 bucket. Server access logs are useful for security auditing and helping customers understand Amazon S3 billing where data scans play an important part. You can enable server access logging on the AWS console or programmatically using the Amazon S3 API or AWS SDKs. While there is no charge for server access logging as such, any log files that are delivered to your bucket accrue the usual charges for storage.

**AWS Trusted Advisor** AWS Trusted Advisor is an application that draws upon best practices learned from AWS’s aggregated operational history of serving hundreds of thousands of AWS customers. Trusted Advisor inspects your AWS environment and makes recommendations for saving money, improving system performance, or closing security gaps. AWS Trusted Advisor has the following S3-related checks, which are helpful from a security context:

- Logging configuration of Amazon S3 buckets
- Security checks for Amazon S3 buckets that have open access permissions
- Fault tolerance checks for buckets that don’t have versioning enabled or have versioning suspended

## Best Practices for Security on Amazon S3

Now that we have looked at some of the best practices for security on Amazon S3, let’s look at some of the best practices for security to protect your data:

- Enable account-level Block Public Access.
- Leverage access points to scope application permissions.
- Send secure traffic with VPC endpoints and access points.
- Use bucket policy to enforce TLS.
- Encrypt everything: SSE-KMS & SSE-S3.
- Enable object lock, versioning, MFA delete to protect data.
- Monitor using AWS tools, such as AWS CloudTrail and AWS Config.

## Amazon Athena Security

We’ve looked at Amazon Athena in Chapter 4, “Data Processing and Analysis” and how it is a great tool built on Apache Presto to enable ad hoc data analysis. Since Amazon Athena will be used by data analysts and data scientists, it becomes very important to understand how it secures the data.
