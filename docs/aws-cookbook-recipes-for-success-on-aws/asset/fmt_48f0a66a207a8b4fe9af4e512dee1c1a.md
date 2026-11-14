
Using administrative access for routine development tasks is not a security best practice. Giving unneeded permissions can result in unauthorized actions being performed. Using the `PowerUserAccess` AWS managed policy for development purposes is a better alternative to start rather than using `AdministratorAccess`. Later, you should define your own [customer managed policy](https://oreil.ly/hfQ3p) granting only the specific actions for your needs. For example, if you need to log in often to check the status of your `EC2` instances, you can create a read-only policy for this purpose and attach it to a role. Similarly, you can create a role for billing access and use it to access the AWS Billing console only. The more you practice using the [principle of least privilege](https://oreil.ly/jMW6h), the more security will become a natural part of what you do.

You used an `IAM user` in this recipe to perform the steps. If you are using an AWS account that leverages [federation](https://oreil.ly/hmUw3) for access (e.g., a sandbox or development AWS account at your employer), you should use temporary credentials from the `AWS STS` rather than an `IAM user`. This type of access uses time-based tokens that expire after an amount of time, rather than “long-lived” credentials like access keys or passwords. When you performed the `AssumeRole` in the validation steps, you called the STS service for temporary credentials. To help with frequent `AssumeRole` operations, the `AWS CLI` supports [named profiles](https://oreil.ly/fZSQ9) that can automatically assume and refresh your temporary credentials for your role when you specify the `role_arn` parameter in the named profile.

**Tip**

You can require multi-factor authentication (`MFA`) as a condition within the `AssumeRole` policies you create. This would allow the role to be assumed only by an identity that has been authenticated with `MFA`. For more information about requiring `MFA` for `AssumeRole`, see [the support document](https://oreil.ly/djRQI).

See [Recipe 9.4](#ch09.html#setting_up_email_alerts_for_root_login) to create an alert when a root login occurs.

**Tip**

You can grant cross-account access to your AWS resources. The resource you define in the policy in this recipe would reference the AWS account and principal within that account that you would like to delegate access to. You should always use an `ExternalID` when enabling cross-account access. For more information, see the [official tutorial for cross-account access](https://oreil.ly/wwlF4).

### Challenge

Create additional `IAM roles` for each of the AWS managed policies for job functions (e.g., billing, database administrator, networking, etc.)

# 1.2 Generating a Least Privilege IAM Policy Based on Access Patterns

## Problem

You would like to implement least privilege access for your user and scope down the permissions to allow access to only the services, resources, and actions you need to use in your AWS account.

## Solution

Use the `IAM Access Analyzer` in the `IAM console` to generate an `IAM policy` based on the `CloudTrail` activity in your AWS account, as shown in [Figure 1-2](#ch01.html#iam_access_analyzer_workflow).

![](assets/awsc_0102.png)

### Prerequisite

*   `CloudTrail` logging enabled for your account to a configured `S3` bucket (see [Recipe 9.3](#ch09.html#enabling_cloudtrail_logging_for_your_aw))

### Steps

1.  Navigate to the [IAM console](https://console.aws.amazon.com/iam) and select your `IAM role` or `IAM user` that you would like to generate a policy for.

2.  On the `Permissions tab` (the default active tab when viewing your principal), scroll to the bottom, expand the “Generate policy based on `CloudTrail events`” section, and click the “`Generate policy`” button.

    **Tip**

    For a quick view of the AWS services accessed from your principal, click the `Access Advisor tab` and view the service list and access time. While the `IAM Access Advisor` is not as powerful as the `Access Analyzer`, it can be helpful when auditing or troubleshooting `IAM principals` in your AWS account.

3.  Select the time period of `CloudTrail events` you would like to evaluate, select your `CloudTrail trail`, choose your `Region` (or select “All regions”), and choose “`Create and use a new service role`.” `IAM Access Analyzer` will create a role for the service to use for read access to your trail that you selected. Finally, click “`Generate policy`.” See [Figure 1-3](#ch01.html#generate_policy_in_the_iam_access_analy) for an example.

    ![](assets/awsc_0103.png)

    **Note**

    The role creation can take up to 30 seconds. Once the role is created, the policy generation will take an amount of time depending on how much activity is in your `CloudTrail trail`.

4.  Once the analyzer has completed, scroll to the bottom of the `permissions tab` and click “`View generated policy`,” as shown in [Figure 1-4](#ch01.html#view_generated_policy).

    ![](assets/awsc_0104.png)

5.  Click Next, and you will see a generated policy in `JSON` format that is based on the activity that your `IAM principal` has made. You can edit this policy in the interface if you wish to add additional permissions. Click Next again, choose a name, and you can deploy this generated policy as an `IAM policy`.

    You should see a generated `IAM policy` in the `IAM console` similar to this:

    ```
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": [
            "access-analyzer:ListPolicyGenerations",
            "cloudtrail:DescribeTrails",
            "cloudtrail:LookupEvents",
            "iam:GetAccountPasswordPolicy",
            "iam:GetAccountSummary",
            "iam:GetServiceLastAccessedDetails",
            "iam:ListAccountAliases",
            "iam:ListGroups",
            "iam:ListMFADevices",
            "iam:ListUsers",
            "s3:ListAllMyBuckets",
            "sts:GetCallerIdentity"
          ],
          "Resource": "*"
        }, ...
    }
    ```

### Validation checks

Create a new `IAM user` or `IAM role` and attach the newly created `IAM policy` to it. Perform an action granted by the policy to verify that the policy allows your `IAM principal` to perform the actions that you need it to.

## Discussion

You should always seek to implement least privilege `IAM policies` when you are scoping them for your users and applications. Oftentimes, you might not know exactly what permissions you may need when you start. With `IAM Access Analyzer`, you can start by granting your users and applications a larger scope in a development environment, enable `CloudTrail logging` ([Recipe 9.3](#ch09.html#enabling_cloudtrail_logging_for_your_aw)), and then run `IAM Access Analyzer` after you have a window of time that provides a good representation of the usual activity (choose this time period in the `Access Analyzer` configuration as you did in step 3). The generated policy will contain all of the necessary permissions to allow your application or users to work as they did during that time period that you chose to analyze, helping you implement the [principle of least privilege](https://oreil.ly/jMW6h).

**Note**

You should also be aware of the [list of services](https://oreil.ly/KPEFh) that `Access Analyzer` supports.

### Challenge

Use the `IAM Policy Simulator` (see [Recipe 1.4](#ch01.html#testing_iam_policies_with_the_iam_polic)) on the generated policy to verify that the policy contains the access you need.

# 1.3 Enforcing IAM User Password Policies in Your AWS Account

**Note**

Special thanks to Gaurav Raje for his contribution to this recipe.

## Problem

Your security policy requires that you must enforce a password policy for all the users within your AWS account. The password policy sets a 90-day expiration, and passwords must be made up of a minimum of 32 characters including lowercase and uppercase letters, numbers, and symbols.

## Solution
