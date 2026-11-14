
# 6.6 Autoscaling ECS Service

```
curl -v -m 3 http://AWSCookbookLoadtestLoadBalancer-36821611.us-east-1.elb.amazonaws.com:80/cpu
*   Trying 52.4.148.24...
* TCP_NODELAY set
* Connected to AWSCookbookLoadtestLoadBalancer-36821611.us-east-1.elb.amazonaws.com (52.4.148.245) port 80 (#0)
> GET /cpu HTTP/1.1
> Host: AWSCookbookLoadtestLoadBalancer-36821611.us-east-1.elb.amazonaws.com:80
> User-Agent: curl/7.64.1
> Accept: */*
>
* Operation timed out after 3002 milliseconds with 0 bytes received
* Closing connection 0
curl: (28) Operation timed out after 3002 milliseconds with 0 bytes received
```

## Validation Checks

Wait approximately five minutes. Then log into the AWS Console, locate Elastic Container Service, go to the Clusters page, select the cluster deployed, and select the ECS service. Verify that the Desired Count has increased to 4, the maximum scaling value that you configured. You can click the Tasks tab to view four container tasks now running for your service.

Click the Metrics tab to view the CPU usage for the service. You set the scaling target at 50% to trigger the autoscaling actions, adding two additional containers to the service as a result of high CPU usage. An example metrics graph is shown in [Figure 6-11](#ch06.html#ecs_service_metrics_on_the_aws_console).

![](assets/awsc_0611.png)

## Cleanup

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/Containers).

## Discussion

Autoscaling is an important mechanism to implement to save costs associated with running your applications on AWS services. It allows your applications to provision their own resources as needed during times when load may increase, and it removes their own resources during times when the application may be idle. Note that whenever you have an AWS service doing something like this on your behalf, you have to specifically grant permission for services to execute these functions via IAM.

The underlying data that provides the metrics for such operations is contained in the CloudWatch metrics service. There are many data points and metrics that you can use for configuring autoscaling; some of the most common ones are listed here:

*   Network I/O
*   CPU usage
*   Memory used
*   Number of transactions

In this recipe, you monitor the CPU usage metric on the ECS service. You set the metric at 50% and trigger the CPU load with a `curl` call to the HTTP endpoint of the ECS service. Scaling metrics are dependent upon the type of applications you are running and the technologies you use to build them. As a best practice, you should observe your application metrics over a period of time to set a baseline before choosing metrics to implement autoscaling.

## Challenge

Replace the provided sample CPU load application with your own containerized application and configure the target scaling policy to meet your needs.

# 6.7 Launching a Fargate Container Task in Response to an Event

## Problem

You need to launch a container task to process incoming files.

## Solution

Use Amazon EventBridge to trigger the launch of ECS container tasks on Fargate after a file is uploaded to S3, as shown in [Figure 6-12](#ch06.html#flow_of_container_eventbridge_pattern).

![](assets/awsc_0612.png)

### Preparation

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/Containers).

### Steps

1.  Configure CloudTrail to log events on the S3 bucket:

    ```
    aws cloudtrail put-event-selectors --trail-name $CLOUD_TRAIL_ARN --event-selectors "[{ \"ReadWriteType\": \"WriteOnly\", \"IncludeManagementEvents\":false, \"DataResources\": [{ \"Type\": \"AWS::S3::Object\", \"Values\": [\"arn:aws:s3:::$BUCKET_NAME/input/\"] }], \"ExcludeManagementEventSources\": [] }]"
    ```

    Now create an assume-role policy JSON statement called `policy1.json` to use in the next step (this file is provided in the repository):

    ```
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Principal": {
            "Service": "events.amazonaws.com"
          },
          "Action": "sts:AssumeRole"
        }
      ]
    }
    ```

2.  Create the role and specify the `policy1.json` file:

    ```
    aws iam create-role --role-name AWSCookbook607RuleRole \
        --assume-role-policy-document file://policy1.json
    ```

3.  You will also need a policy document with the following content called `policy2.json` (this file is provided in the repository):

    ```
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": [
            "ecs:RunTask"
          ],
          "Resource": [
            "arn:aws:ecs:*:*:task-definition/*"
          ]
        },
        {
          "Effect": "Allow",
          "Action": "iam:PassRole",
          "Resource": [
            "*"
          ],
          "Condition": {
            "StringLike": {
              "iam:PassedToService": "ecs-tasks.amazonaws.com"
            }
          }
        }
      ]
    }
    ```

4.  Now attach the IAM policy JSON you just created to the IAM role:

    ```
    aws iam put-role-policy --role-name AWSCookbook607RuleRole \
        --policy-name ECSRunTaskPermissionsForEvents \
        --policy-document file://policy2.json
    ```

5.  Create an EventBridge rule that monitors the S3 bucket for file uploads:

    ```
    aws events put-rule --name "AWSCookbookRule" --role-arn "arn:aws:iam::$AWS_ACCOUNT_ID:role/AWSCookbook607RuleRole" --event-pattern "{\"source\":[\"aws.s3\"],\"detail-type\":[\"AWS API Call via CloudTrail\"],\"detail\":{\"eventSource\":[\"s3.amazonaws.com\"],\"eventName\":[\"CopyObject\",\"PutObject\",\"CompleteMultipartUpload\"],\"requestParameters\":{\"bucketName\":[\"$BUCKET_NAME\"]}}}"
    ```

6.  Modify the value in `targets-template.json` and create a `targets.json` for use:

    ```
    sed -e "s|AWS_ACCOUNT_ID|${AWS_ACCOUNT_ID}|g" \
        -e "s|AWS_REGION|${AWS_REGION}|g" \
        -e "s|ECSClusterARN|${ECS_CLUSTER_ARN}|g" \
        -e "s|TaskDefinitionARN|${TASK_DEFINITION_ARN}|g" \
        -e "s|VPCPrivateSubnets|${VPC_PRIVATE_SUBNETS}|g" \
        -e "s|VPCDefaultSecurityGroup|${VPC_DEFAULT_SECURITY_GROUP}|g" \
        targets-template.json > targets.json
    ```

7.  Create a rule target that specifies the ECS cluster, ECS task definition, IAM role, and networking parameters. This specifies what the rule will trigger; in this case, launch a container on Fargate:

    ```
    aws events put-targets --rule AWSCookbookRule \
        --targets file://targets.json
    ```

    You should see output similar to the following:

    ```
    {
      "FailedEntryCount": 0,
      "FailedEntries": []
    }
    ```

8.  Check the S3 bucket to verify that it’s empty before we populate it:

    ```
    aws s3 ls s3://$BUCKET_NAME/
    ```

9.  Copy the provided `maze.jpg` file to the S3 bucket. This will trigger the ECS task that launches a container with a [Python library](https://pypi.org/project/mazesolver) to process the file:

    ```
    aws s3 cp maze.jpg s3://$BUCKET_NAME/input/maze.jpg
    ```

    This will trigger an ECS task to process the image file. Quickly, check the task with the `ecs list-tasks` command. The task will run for about two to three minutes:

    ```
    aws ecs list-tasks --cluster $ECS_CLUSTER_ARN
    ```

    You should see output similar to the following:

    ```
    {
      "taskArns": [
        "arn:aws:ecs:us-east-1:111111111111:task/cdk-aws-cookbook-607-AWSCookbookEcsCluster46494E6E-MX7kvtp1sYWZ/d86f16af55da56b5ca4874d6029"
      ]
    }
    ```

## Validation Checks

After a few minutes, observe the output directory created in the S3 bucket:

```
aws s3 ls s3://$BUCKET_NAME/output/
```

Download and view the output file:

```
aws s3 cp s3://$BUCKET_NAME/output/output.jpg ./output.jpg
```
