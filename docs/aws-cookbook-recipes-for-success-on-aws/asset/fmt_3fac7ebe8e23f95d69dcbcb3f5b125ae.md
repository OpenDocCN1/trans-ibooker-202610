
Follow the steps in this recipe's folder in the [chapter code repository](https://github.com/AWSCookbook/Security).

## Steps

1.  Create an Access Analyzer to use for validation of access:

    ```
    ANALYZER_ARN=$(aws accessanalyzer create-analyzer \
         --analyzer-name awscookbook109\
         --type ACCOUNT \
         --output text --query arn)
    ```

2.  Perform a scan of your S3 bucket with the Access Analyzer:

    ```
    aws accessanalyzer start-resource-scan \
         --analyzer-arn $ANALYZER_ARN \
         --resource-arn arn:aws:s3:::awscookbook109-$RANDOM_STRING
    ```

3.  Get the results of the Access Analyzer scan (it may take about 30 seconds for the scan results to become available):

    ```
    aws accessanalyzer get-analyzed-resource \
         --analyzer-arn $ANALYZER_ARN \
         --resource-arn arn:aws:s3:::awscookbook109-$RANDOM_STRING
    ```

    You should see output similar to the following (note the `isPublic` value):

    ```
    {
      "resource": {
      "actions": [
        "s3:GetObject",
        "s3:GetObjectVersion"
      ],
      "analyzedAt": "2021-06-26T17:42:00.861000+00:00",
      "createdAt": "2021-06-26T17:42:00.861000+00:00",
      "isPublic": true,
      "resourceArn": "arn:aws:s3:::awscookbook109-<<string>>",
      "resourceOwnerAccount": "111111111111",
      "resourceType": "AWS::S3::Bucket",
      "sharedVia": [
        "POLICY"
      ],
      "status": "ACTIVE",
      "updatedAt": "2021-06-26T17:42:00.861000+00:00"
      }
    }
    ```

4.  Set the public access block for your bucket:

    ```
    aws s3api put-public-access-block \
         --bucket awscookbook109-$RANDOM_STRING \
         --public-access-block-configuration \
    "BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true"
    ```

    #### Note

    See the AWS [article](https://oreil.ly/3jRmO) on the available `PublicAccessBlock` configuration properties.

### Validation checks

Perform a scan of your S3 bucket:

```
aws accessanalyzer start-resource-scan \
     --analyzer-arn $ANALYZER_ARN \
     --resource-arn arn:aws:s3:::awscookbook109-$RANDOM_STRING
```

Get the results of the Access Analyzer scan:

```
aws accessanalyzer get-analyzed-resource \
     --analyzer-arn $ANALYZER_ARN \
     --resource-arn arn:aws:s3:::awscookbook109-$RANDOM_STRING
```

You should see output similar to the following:

```
{
  "resource": {
  "analyzedAt": "2021-06-26T17:46:24.906000+00:00",
  "isPublic": false,
  "resourceArn": "arn:aws:s3:::awscookbook109-<<string>>",
  "resourceOwnerAccount": "111111111111",
  "resourceType": "AWS::S3::Bucket"
  }
}
```

## Cleanup

Follow the steps in this recipe's folder in the [chapter code repository](https://github.com/AWSCookbook/Security).

## Discussion

One of the best things you can do to ensure data security in your AWS account is to always make certain that you apply the right security controls to your data. If you mark an object as public in your S3 bucket, it is accessible to anyone on the internet, since S3 serves objects using HTTP. One of the most common security misconfigurations that users make in the cloud is marking object(s) as public when that is not intended or required. To protect against misconfiguration of S3 objects, enabling `BlockPublicAccess` for your buckets is a great thing to do from a security standpoint.

### Tip

You can also set public block settings at your account level, which would include all S3 buckets in your account:

```
aws s3control put-public-access-block \
    --public-access-block-configuration \
    BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true \
    --account-id $AWS_ACCOUNT_ID
```

You can serve S3 content to internet users via HTTP and HTTPS while keeping your bucket private. Content delivery networking (CDN), like Amazon CloudFront, provides more secure, efficient, and cost-effective ways to achieve global static website hosting and still use S3 as your object source. To see an example of a CloudFront configuration that serves static content from an S3 bucket, see [Recipe 1.10](#ch01.html#serving_web_content_securely_from_sthre).

## Challenge

Deploy a VPC endpoint for S3 within your VPC and create a bucket policy to restrict access to your S3 bucket through this endpoint only.

# 1.10 Serving Web Content Securely from S3 with CloudFront

## Problem

You have nonpublic web content in S3 and want to configure CloudFront to serve the content.

## Solution

Create a CloudFront distribution and set the origin to your S3 bucket. Then configure an origin access identity (OAI) to require the bucket to be accessible only from CloudFront (see [Figure 1-12](#ch01.html#cloudfront_and_sthree)).

![](assets/awsc_0112.png)

## Prerequisite

*   S3 bucket with static web content

## Preparation

Follow the steps in this recipe's folder in the [chapter code repository](https://github.com/AWSCookbook/Security).

## Steps

1.  Create a CloudFront OAI to reference in an S3 bucket policy:

    ```
    OAI=$(aws cloudfront create-cloud-front-origin-access-identity \
         --cloud-front-origin-access-identity-config \
         CallerReference="awscookbook",Comment="AWSCookbook OAI" \
         --query CloudFrontOriginAccessIdentity.Id --output text)
    ```

2.  Use the `sed` command to replace the values in the `distribution-config-template.json` file with your CloudFront OAI and S3 bucket name:

    ```
    sed -e "s/CLOUDFRONT_OAI/${OAI}/g" \
         -e "s|S3_BUCKET_NAME|awscookbook110-$RANDOM_STRING|g" \
         distribution-template.json > distribution.json
    ```

3.  Create a CloudFront distribution that uses the distribution configuration JSON file you just created:

    ```
    DISTRIBUTION_ID=$(aws cloudfront create-distribution \
         --distribution-config file://distribution.json \
         --query Distribution.Id --output text)
    ```

4.  The distribution will take a few minutes to create; use this command to check the status. Wait until the status reaches "Deployed":

    ```
    aws cloudfront get-distribution --id $DISTRIBUTION_ID \
        --output text --query Distribution.Status
    ```

5.  Configure the S3 bucket policy to allow only requests from CloudFront by using a bucket policy like this (we have provided `bucket-policy-template.json` in the repository):

    ```
    {
      "Version": "2012-10-17",
      "Id": "PolicyForCloudFrontPrivateContent",
      "Statement": [
        {
          "Effect": "Allow",
          "Principal": {
            "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity CLOUDFRONT_OAI"
          },
          "Action": "s3:GetObject",
          "Resource": "arn:aws:s3:::S3_BUCKET_NAME/*"
        }
      ]
    }
    ```

6.  Use the `sed` command to replace the values in the `bucket-policy-template.json` file with the CloudFront OAI and S3 bucket name:

    ```
    sed -e "s/CLOUDFRONT_OAI/${OAI}/g" \
         -e "s|S3_BUCKET_NAME|awscookbook110-$RANDOM_STRING|g" \
         bucket-policy-template.json > bucket-policy.json
    ```

7.  Apply the bucket policy to the S3 bucket with your static web content:

    ```
    aws s3api put-bucket-policy --bucket awscookbook110-$RANDOM_STRING \
         --policy file://bucket-policy.json
    ```

8.  Get the `DOMAIN_NAME` of the distribution you created:

    ```
    DOMAIN_NAME=$(aws cloudfront get-distribution --id $DISTRIBUTION_ID \
         --query Distribution.DomainName --output text)
    ```

### Validation checks

Try to access the S3 bucket directly using HTTPS to verify that the bucket does not serve content directly:

```
curl https://awscookbook110-$RANDOM_STRING.s3.$AWS_REGION.amazonaws.com/index.html
```

You should see output similar to the following:
