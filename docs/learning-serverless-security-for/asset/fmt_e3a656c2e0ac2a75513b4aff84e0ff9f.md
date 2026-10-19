
*   [“Mitigating the AssumeRole Privilege Escalation Vulnerability”](#ch05.html#mitigating_the_assumerole_privilege_escalation_vulnerability_ch05-exploiting-and-securing-misconfigured-aws-iam-roles_1764685813259)

*   [“Reconfiguring the AWS CLI and Verifying That AssumeRole Can No Longer Be Used to Escalate Privileges”](#ch05.html#reconfiguring_the_aws_cli_and_verifying_that_assumerole_can_no_longer_be_used_to_escalate_privileges_ch05-exploiting-and-securing-misconfigured-aws-iam-roles_1764685841521)

*   [“Abusing a Function’s Overly Permissive Execution Role to Escalate Privileges”](#ch05.html#abusing_a_functions_overly_permissive_execution_role_to_escalate_privileges_ch05-exploiting-and-securing-misconfigured-aws-iam-roles_1764685862431)

*   [“Establishing Persistence with New IAM User Credentials”](#ch05.html#ch05-establishing-persistence-with-new-iam-user-credentials)

*   [“Establishing Persistence via a Backdoored Lambda Version”](#ch05.html#establishing_persistence_via_a_backdoored_lambda_version_ch05-exploiting-and-securing-misconfigured-aws-iam-roles_1764685892771)

After completing this section, you’ll be better equipped to identify red flags, detect suspicious activity, and assess Lambda functions and their IAM roles for potential misconfigurations and security risks.

## Mitigating the AssumeRole Privilege Escalation Vulnerability

Earlier in this chapter, you explored how an attacker could escalate privileges by exploiting the `AssumeRole` permission. In this section, you will simulate a security engineer mitigating the `AssumeRole` privilege escalation vulnerability. You’ll start by removing the `AssumeRole` permission from the `Developers` group and then proceed with updating the `lambda-assumable-role` IAM role’s trust policy.

###### Note

This section assumes you are using the Serverless Lab root account to configure IAM permissions and trust relationships in the Serverless Lab AWS account. While it is not recommended to make IAM changes from the root account, it is used in this section to simplify the lab setup.

Let’s proceed with the simulation:

1. Type `iam` in the search bar and then select IAM from the list of matching services.
2. In the left navigation pane of the IAM Console Home page, select “User groups” under Access Management.
3. From the list of “User groups,” click `Developers`.
4. On the Permissions tab, select `AssumeRolePolicy` and then click Remove.

###### Note

Follow the prompt—such as entering the inline policy name in the text input field—to confirm the deletion.

Next, let’s update the trust policy so that IAM users can no longer assume the `assumable-role` role:

1. In the left navigation pane, select Roles under Access Management.
2. Type `assumable` in the search box to locate the `assumable-role` role. Click `lambda-assumable-role`.
3. On the “Trust relationships” tab, click “Edit trust policy.”
4. On the “Edit trust policy” page, specify the following policy in the text area:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "lambda.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

Click “Update policy.” The `Trust policy updated` notification appears, indicating that the trust policy has been updated successfully.

You’ve now successfully mitigated the `AssumeRole` privilege escalation vulnerability. Keep in mind that mitigating vulnerabilities often involves trade-offs, and modifying IAM roles with overly restrictive permissions can sometimes disrupt normal operations if not carefully planned. Always validate that IAM modifications preserve the access and functionality required by authorized users and services.

## Reconfiguring the AWS CLI and Verifying That AssumeRole Can No Longer Be Used to Escalate Privileges

In this section, you’ll remove all saved AWS CLI profiles and credentials in the CloudShell terminal of the Attacker account, and then reconfigure the CLI with the credentials of the `Developer001` IAM user. You’ll then confirm that with the stolen credentials of `Developer001`, an attacker can no longer use `AssumeRole` to perform privilege escalation.

###### Note

This section assumes you are using the Attacker account for simulating an attacker’s attempts to exploit the resources in the serverless lab environment.

Let’s walk through the steps to reconfigure `Developer001`’s AWS CLI profile and confirm that an attacker can’t use `AssumeRole` to escalate privileges:

1. Run the following commands to remove all saved AWS CLI profiles and credentials in the CloudShell terminal environment:^([7](#ch05.html#id643))

```
mv ~/.aws/config ~/.aws/config.bak
mv ~/.aws/credentials ~/.aws/credentials.bak
```

2. Confirm that no valid identity is configured:

```
aws sts get-caller-identity --profile target-dev-account
```

You should get the following error message confirming that no valid AWS credentials are currently configured for the specified profile:

```
The config profile (target-dev-account) could not be found
```

3. Reconfigure your AWS CLI with the access credentials of `Developer001`:

```
aws configure --profile target-dev-account
```

###### Tip

Make sure to use the access credentials generated after running `aws iam create-access-key --user-name $DEVELOPER` in [“Setting Up IAM Groups and Users”](#ch05.html#ch05-setting-up-iam-groups-and-users).

4. Confirm which IAM identity the AWS CLI is currently authenticated as:

```
aws sts get-caller-identity --profile target-dev-account
```

The following JSON response indicates that the CLI is successfully using the access keys associated with `Developer001`:

```
{
    "UserId": "*********************",
    "Account": "************",
    "Arn": "arn:aws:iam::************:user/Developer001"
}
```

5. Replace `[ACCOUNT_ID]` with the account ID of the Serverless Lab account and then run the following command to assume the `lambda-assumable-role` IAM role:

```
aws sts assume-role \
  --role-arn arn:aws:iam::[ACCOUNT_ID]:role/lambda-assumable-role \
  --role-session-name assume-session-001 \
  --profile target-dev-account
```

You should get the following response indicating that `Developer001` is not authorized to assume the `lambda-assumable-role` IAM role:

```
An error occurred (AccessDenied) when calling the AssumeRole operation: User: arn:aws:iam::...:user/Developer001 is not authorized to perform: sts:AssumeRole on resource: arn:aws:iam::...:role/lambda-assumable-role
```

You’ve verified that `AssumeRole` can no longer be used to escalate privileges. However, as you’ll see in the next section, an attacker can still escalate privileges through an AWS Lambda resource that has been configured with an overly permissive execution role.

## Abusing a Function’s Overly Permissive Execution Role to Escalate Privileges

In this section, you will simulate an attacker abusing a Lambda function’s misconfigured and overly permissive execution role to escalate privileges. You’ll start by downloading the Lambda function code, and proceed with modifying it to return the execution role’s session credentials. After that, you’ll package and deploy the modified function code, and then invoke the function to retrieve temporary credentials (`AccessKeyId`, `SecretAccessKey`, `SessionToken`). Finally, using these credentials, you’ll configure a new local AWS CLI profile to use those credentials for the duration of their validity, as shown in [Figure 5-3](#ch05.html#privilege-escalation-through-lambda-function).

![](assets/lsse_0503.png)
