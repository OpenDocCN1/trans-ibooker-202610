+   - For situations where you need to provide access to objects in different folders for a group of users, use object tags to restrict access only to certain objects in the S3 bucket. Consider, for example, that we have three different data sources (e-commerce, agents, miscellaneous) that populate user data into the S3 bucket. Each source will upload two files daily, one that contains sensitive data (which shouldn’t be accessed by users) and one that doesn’t contain sensitive data (which can be accessed by all users). Each source will upload the files under a folder with their name. In this case, we can use object tags to provide access to files that don’t contain sensitive data, as shown here:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Principal": {"AWS": [
        "arn:aws:iam::<<accountid>>:role/role_name"
      ]},
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::wranglerbook/*",
      "Condition": { "StringEquals": {"s3:ExistingObjectTag/dataclassification": "sensitive"} }
    }
  ]
}
```

Figure 11.3: S3 access policy statement restricting access based on tags attached to an S3 bucket

You can also ensure the objects are tagged correctly when the data is uploaded into the S3 bucket by making the tag mandatory for the PutObject action. Refer to the following AWS documentation for additional details on object-based tagging:
https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html#example-bucket-policies-object-tags

# Additional options to control data lake access

You can perform the following activities to secure data in your data lake:

+   - **S3 access points** help simplify access control for shared buckets that are accessed by many teams. They are named network endpoints that are attached to the S3 bucket with distinct permission policies. S3 endpoints help manage buckets with complex bucket policies that are difficult to maintain by segregating policies for each use case into a separate endpoint. Some common use cases are buckets that host large shared datasets, with each dataset having a different access policy, and testing new access policy changes on an S3 bucket without impacting the existing policies.
+   - **Object lambda access points** are useful if you want to use a lambda function in front of an access point. They help in cases such as redacting PII data using a Lambda object. Refer to the following blog post to learn how to implement the solution for this: https://docs.aws.amazon.com/AmazonS3/latest/userguide/tutorial-s3-object-lambda-redact-pii.html
+   - **IAM Access Analyzer** can help validate policies for grammar and best practices, identify resources that are shared with external entities, and generate policies based on CloudTrail logs. It helps you apply policies that adhere to the least-privilege model and grant only the minimal permissions needed by users. This should be validated periodically to ensure the policies are updated based on recent user activity.
+   - **Amazon S3 logging options:** Amazon S3 provides a couple of logging options for capturing all user actions for auditing, compliance, and security purposes: