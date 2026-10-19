
In this chapter, you will explore how attackers exploit publicly exposed Lambda URLs and run arbitrary code in the function’s execution environment. You’ll examine how a code-injection vulnerability in a Lambda function, when combined with an overly permissive IAM role, lets an attacker exfiltrate secrets stored in a secrets manager service.

To help you experience the attacker’s perspective firsthand, this chapter has been organized into five sections:

*   [“Preparing and Deploying a Vulnerable Serverless Web Application”](#ch06.html#preparing_and_deploying_a_vulnerable_serverless_web_application_ch06-hacking-publicly-accessible-lambda-functions_1762887997958)
*   [“Testing for Common S3 Bucket Misconfigurations”](#ch06.html#testing_for_common_s3_bucket_misconfigurations_ch06-hacking-publicly-accessible-lambda-functions_1762888018450)
*   [“Using Code Injection Attacks to Execute Arbitrary Code in Lambda Functions”](#ch06.html#using_code_injection_attacks_to_execute_arbitrary_code_in_lambda_functions_ch06-hacking-publicly-accessible-lambda-functions_1762888039235)
*   [“Exfiltrating Sensitive Data via Code Injection and Outbound Network Access”](#ch06.html#exfiltrating_sensitive_data_via_code_injection_and_outbound_network_access_ch06-hacking-publicly-accessible-lambda-functions_1762888055692)
*   [“Stealing Secrets Manager Secrets Through a Vulnerable Lambda Function”](#ch06.html#stealing_secrets_manager_secrets_through_a_vulnerable_lambda_function_ch06-hacking-publicly-accessible-lambda-functions_1762888073588)

By the end of this chapter, you will be better equipped to identify vulnerable serverless configurations before attackers can exploit them.

# Reviewing Technical Prerequisites

To follow along in this chapter, you will need the following:

*   Two AWS accounts: one to set up and host the vulnerable-by-design serverless lab environment (the Serverless Lab account) and another for running simulated exploits (the Attacker account). Make sure to have both accounts ready and open in separate browser sessions (or incognito/private windows) to avoid session conflicts.
*   A code editor installed on your local machine (such as VS Code or Sublime Text).

If you are using Windows, ensure you have a way to run macOS or Linux commands—for example, by using WSL, or a VM with a Linux distribution.

> Note
>
> To help you work through the exercises and simulations, a copy of the code and commands used in this chapter is available in a [GitHub gist (see `ch06.md`)](https://oreil.ly/0mSPU).

# Preparing and Deploying a Vulnerable Serverless Web Application

Code injection is one of the leading causes of remote code execution in serverless applications. It allows attackers to run arbitrary code, access sensitive credentials or configuration, exfiltrate data over the network, and alter a function’s behavior or response. When combined with overly permissive IAM roles, a single code-injection vulnerability can let attackers leverage the function’s permissions and access other cloud resources.

In this section, you will set up a vulnerable-by-design serverless web application to explore how code injection attacks lead to unauthorized code execution in AWS Lambda. This will help you build a deeper understanding of how insecure code along with function-level misconfigurations can expose serverless applications to various security threats.

The serverless web application you’ll work with in this chapter consists of a static web frontend hosted in an Amazon S3 bucket, which makes requests to a Lambda function that’s intentionally vulnerable to code injection ([Figure 6-1](#ch06.html#lab-setup-00)).

![](assets/lsse_0601.png)

This application lets users enter math expressions in the browser, which the backend evaluates and sends back as a response to the frontend. As you work through the hands-on examples, you’ll learn how these issues and misconfigurations can be exploited, and you’ll see practical countermeasures you can implement to mitigate these risks.

> Caution
>
> Once you’ve finished the hands-on examples and simulations in this chapter and the next, make sure to delete all Lambda functions, S3 buckets, IAM roles, and other resources in your cloud account(s) to avoid unintended costs and security risks.

## Setting Up a Vulnerable AWS Lambda Function

Let’s start by setting up a vulnerable-by-design AWS Lambda function that accepts and evaluates user-provided mathematical expressions. In this section, you will re-create and explore several coding and configuration issues found in real-world serverless applications, including the use of `eval()` in the function code, hardcoded credentials in the source code, and secrets stored in environment variables. You will also leave the Lambda function exposed via an unauthenticated function URL, which is invoked directly by the static web frontend hosted in the S3 bucket.^[1](#ch06.html#id659)

Before you dive into the hands-on portion, let’s define a few key terms relevant to this chapter:

**AWS Lambda function**

This self-contained piece of code serves as a fundamental building block of serverless applications. It runs in response to specific events, such as API requests, file uploads, or database updates, and executes within a secure, isolated environment managed by AWS.^[2](#ch06.html#id661) Each function has a single handler method that receives event data and executes the logic you specify.

**Lambda execution environment**

This secure, isolated space manages the resources for running your function code. You provide configuration—such as memory and timeout—when creating the function, and Lambda uses this configuration to provision the execution environment.

**Lambda runtime**

The language-specific environment that manages the flow of invocation events, context information, and function responses. The Lambda function’s runtime runs as a process inside the Lambda execution environment.

**Lambda execution role**

An AWS IAM role that the function assumes at runtime. It defines which actions the function can perform and on which resources.

**Function URL**

An HTTPS endpoint that allows you to invoke your Lambda function directly from web browsers, `curl`, or any HTTP client, without requiring an API gateway. It provides a fast and simple way to expose your function, making it ideal for prototyping and building small or simple applications.

You can think of the vulnerable Lambda function you’ll set up in this section as a maintenance console inside a high-security building. It’s meant for authorized staff to perform limited system tasks, but if left exposed or misconfigured, attackers can use it as an entry point to tamper with or take control of the building’s systems.

> Note
>
> This section assumes you are using the Serverless Lab account, which hosts the vulnerable-by-design serverless lab environment.

Let’s now walk through the setup process:

1. Sign in to the [AWS Management Console](https://oreil.ly/qe5es).
2. Type `lambda` in the search bar and then select Lambda from the list of search results.
3. In the left navigation pane, select Functions and then click “Create function.”

   > Caution
   >
   > In this chapter, all examples assume that your AWS Region is set to `us-east-1`. Make sure to validate that the selected region in the upper-right corner of the AWS Console is set to `United States (N. Virginia)`.

4. On the “Create function” page, select “Author from scratch” ([Figure 6-2](#ch06.html#author-from-scratch)).

   ![](assets/lsse_0602.png)

   In the “Basic information” pane, specify `lambda-0010` in the “Function name” field. Then choose Python 3.13 for the Runtime.

   > Tip
   >
   > If a newer Python runtime is available by the time you’re reading this—such as Python 3.14 or later—you can choose that instead.
