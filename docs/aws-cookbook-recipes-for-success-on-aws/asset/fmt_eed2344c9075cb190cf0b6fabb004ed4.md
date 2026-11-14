
5. Enter a “Tag key” named `Environment`, enter a “Tag value” `Dev`, and click “Review and apply tag changes” (see [Figure 9-10](#ch09.html#tagging_multiple_resources_with_tag_edi)).

![](assets/awsc_0910.png)

6. In the pop-up window, confirm your selection by clicking “Apply changes to all selected” (see [Figure 9-11](#ch09.html#confirming_changes_to_apply_to_resource)).

![](assets/awsc_0911.png)

### Validation checks

Use the CLI to confirm that all EC2 instances have an Environment tag:

```
aws ec2 describe-instances \
  --output text \
  --query 'Reservations[].Instances[?!not_null(Tags[?Key == `Environment`].Value)] | [].[InstanceId]'
```

You should see no output. You can also repeat step 3 to confirm that all resources now have an Environment tag.

## Discussion

A tag consists of a key and a value and can be applied to many resources at the time of creation and almost all resources after creation. You should implement your own tagging strategy as early as possible in your cloud journey. This ensures that you can identify the growing number of resources you deploy over the lifespan of your AWS account. Tags are useful for auditing, billing, scheduling updates, delegating access to resources with specific tags, and so on.

[Cost allocation tags](https://oreil.ly/YvgIT) can help you make sense of your AWS bill, to see exactly which resources are contributing to certain portions of your bill. In addition to the cost allocation report, you can filter by tags interactively via Cost Explorer in the AWS Billing console to analyze and visualize your costs.

The Tag Editor can help you search and perform batch updates for tags across your AWS resources. Say you might have deployed many resources but forgot to tag them all, or you have historically not used tags and would like to start. You can batch select many resources within the Tag Editor (across all Regions or within a selected set of Regions) and perform these updates to ease the burden of implementing a tagging strategy.

While not an exhaustive list, here are some good tags to include as part of a tagging baseline:

`CreatedBy`

: The User or Identity that created the resource

`Application`

: The service or application of which the resource is a component

`CostCenter`

: Useful for billing identification and to help implement a chargeback model for shared AWS account usage

`CreationDate`

: The date the resource was created

`Contact`

: An email address for the team or individual in case of any issues with the resource (also helpful for configuring automated alerts)

`MaintenanceWindow`

: Useful for defining a window of time that the resource is allowed to not be available in case of patching, updates, or maintenance

`DeletionDate`

: Useful for development or sandbox environments so that you know when it may be safe to delete a resource

### Challenge

Apply a baseline set of tags to your AWS resources and begin to enforce the tags with a [tag policy](https://oreil.ly/7UePx) by enabling [AWS Organizations](https://oreil.ly/Dg3I6).

# 9.3 Enabling CloudTrail Logging for Your AWS Account

## Problem

You just set up your AWS account and want to retain an audit log of all activity for all Regions in your account.

## Solution

Configure an S3 bucket with a bucket policy allowing CloudTrail to write events. Enable CloudTrail for all Regions in your account and configure CloudTrail to log all audit events to the S3 bucket, as shown in [Figure 9-12](#ch09.html#turning_on_logging_for_cloudtrail).

![](assets/awsc_0912.png)

### Prerequisite

* S3 bucket for logging

### Preparation

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/BigData).

### Steps

1. Create a file called `cloudtrail-s3policy-template.json` (file provided in the repository):

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3CloudTrail",
      "Effect": "Allow",
      "Principal": {"Service": "cloudtrail.amazonaws.com"},
      "Action": "s3:GetBucketAcl",
      "Resource": "arn:aws:s3:::BUCKET_NAME"
    },
    {
      "Sid": "S3CloudTrail",
      "Effect": "Allow",
      "Principal": {"Service": "cloudtrail.amazonaws.com"},
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::BUCKET_NAME/AWSLogs/AWS_ACCOUNT_ID/*",
      "Condition": {"StringEquals": {"s3:x-amz-acl": "bucket-owner-full-control"}}
    }
  ]
}
```

2. Replace the values in the `cloudtrail-s3policy-template.json` file with values from your deployment. Here is a way to do it quickly with `sed`:

```
sed -e "s/BUCKET_NAME/awscookbook903-$RANDOM_STRING/g" \
     -e "s|AWS_ACCOUNT_ID|${AWS_ACCOUNT_ID}|g" \
     cloudtrail-s3policy-template.json > cloudtrail-s3policy.json
```

3. Add the S3 bucket policy to your S3 bucket:

```
aws s3api put-bucket-policy \
     --bucket awscookbook903-$RANDOM_STRING \
     --policy file://cloudtrail-s3policy.json
```

4. Enable CloudTrail for all AWS Regions and configure the S3 bucket to send logs to the following:

```
aws cloudtrail create-trail --name AWSCookbook903Trail \
     --s3-bucket-name awscookbook903-$RANDOM_STRING \
     --is-multi-region-trail
```

5. Start the logging of your CloudTrail trail:

```
aws cloudtrail start-logging --name AWSCookbook903Trail
```

#### Note

More information can be found on AWS for the required [S3 bucket policy](https://oreil.ly/jhcXQ) for CloudTrail logging.

#### Validation checks

Describe the trail:

```
aws cloudtrail describe-trails --trail-name-list AWSCookbook903Trail
```

You should see output similar to the following:

```
{
  "trailList": [
  {
    "Name": "AWSCookbook903Trail",
    "S3BucketName": "awscookbook903-<<string>>",
    "IncludeGlobalServiceEvents": true,
    "IsMultiRegionTrail": true,
    "HomeRegion": "us-east-1",
    "TrailARN": "arn:aws:cloudtrail:us-east-1:<<Account Id>>:trail/AWSCookbook903Trail",
    "LogFileValidationEnabled": false,
    "HasCustomEventSelectors": false,
    "HasInsightSelectors": false,
    "IsOrganizationTrail": false
  }
  ]
}
```

Get the trail status:

```
aws cloudtrail get-trail-status --name AWSCookbook903Trail
```

You should see output similar to the following:

```
{
  "IsLogging": true,
  "StartLoggingTime": "2021-06-28T21:22:56.308000-04:00",
  "LatestDeliveryAttemptTime": "",
  "LatestNotificationAttemptTime": "",
  "LatestNotificationAttemptSucceeded": "",
  "LatestDeliveryAttemptSucceeded": "",
  "TimeLoggingStarted": "2021-06-29T01:22:56Z",
  "TimeLoggingStopped": ""
}
```

### Cleanup

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/Security).

## Discussion

You can use CloudTrail to log and continuously monitor all events in your AWS account. Specifically, events refer to all API activity against the AWS APIs. These include actions that you (and all authenticated entities) take in the console, via the command line, API activity from your applications, and other AWS services performing actions (automatic actions like autoscaling, EventBridge triggers, replication, etc.).

#### Tip

You can query your logs in place on S3 using Amazon Athena if you are looking for specific events, and you can index your logs with [Amazon OpenSearch](https://oreil.ly/gdkRW) to perform complex queries against your historical data.
