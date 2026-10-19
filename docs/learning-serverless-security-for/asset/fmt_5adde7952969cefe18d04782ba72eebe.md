
* establishing persistence via backdoored Lambda version, [Establishing Persistence via a Backdoored Lambda Version](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc21)\-[Establishing Persistence via a Backdoored Lambda Version](#ch05.html#id653)

* establishing persistence with new user credentials, [Establishing Persistence with New IAM User Credentials](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc19)\-[Establishing Persistence with New IAM User Credentials](#ch05.html#id648)

* group/user setup, [Setting Up IAM Groups and Users](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc8)\-[Setting Up IAM Groups and Users](#ch05.html#id628)

* Lambda function, misconfigured, [Deploying a Lambda Function with an Overly Permissive IAM Role](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc11)\-[Deploying a Lambda Function with an Overly Permissive IAM Role](#ch05.html#id633), [Escalating Privileges and Establishing Persistence Through an AWS Lambda Function with a Misconfigured IAM Role](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc13)\-[Establishing Persistence via a Backdoored Lambda Version](#ch05.html#id657)

* overly permissive IAM role setup, [Setting Up an Overly Permissive IAM Role](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc5)\-[Setting Up an Overly Permissive IAM Role](#ch05.html#id624)

* privilege escalation via AssumeRole, [Abusing AssumeRole for Privilege Escalation](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc3)\-[Establishing Persistence with New IAM User Credentials](#ch05.html#id637)

* privilege escalation via Lambda function, [Escalating Privileges and Establishing Persistence Through an AWS Lambda Function with a Misconfigured IAM Role](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc14)\-[Establishing Persistence via a Backdoored Lambda Version](#ch05.html#id656)

* privilege escalation via overly permissive execution role, [Abusing a Function’s Overly Permissive Execution Role to Escalate Privileges](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc17a)\-[Abusing a Function’s Overly Permissive Execution Role to Escalate Privileges](#ch05.html#id644)

* AWS Lambda functions, [Hacking Publicly Accessible AWS Lambda Functions](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc0)\-[Summary](#ch06.html#id705)

  * API Gateway REST API setup for routing requests to, [Setting Up an API Gateway REST API That Routes Requests to the Lambda Function](#ch07.html#ix_ch07-running-and-securing-serverless-functions-in-a-vpc-asciidoc10)\-[Setting Up an API Gateway REST API That Routes Requests to the Lambda Function](#ch07.html#id740)

  * attaching to virtual private cloud (VPC), [Attaching the Lambda Function to the VPC with Restricted Outbound Access](#ch07.html#ix_ch07-running-and-securing-serverless-functions-in-a-vpc-asciidoc5)\-[Attaching the Lambda Function to the VPC with Restricted Outbound Access](#ch07.html#id727)

  * attack process simulation, [Attacking the Vulnerable Serverless Web Application](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc17)\-[Attacking the Vulnerable Serverless Web Application](#ch06.html#id692)

  * code injection attacks executing arbitrary code in, [Using Code Injection Attacks to Execute Arbitrary Code in Lambda Functions](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc9)\-[Using Code Injection Attacks to Execute Arbitrary Code in Lambda Functions](#ch06.html#id678)

  * code injection safeguards, limited, [Updating the Lambda Function with Limited Code Injection Safeguards](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc14)\-[Updating the Lambda Function with Limited Code Injection Safeguards](#ch06.html#id681)

  * code injection, exfiltrating sensitive data via, [Exfiltrating Sensitive Data via Code Injection and Outbound Network Access](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc11)\-[Attacking the Vulnerable Serverless Web Application](#ch06.html#id695)

  * defined, [Setting Up a Vulnerable AWS Lambda Function](#ch06.html#id660)

  * establishing persistence via backdoored Lambda version, [Establishing Persistence via a Backdoored Lambda Version](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc22)\-[Establishing Persistence via a Backdoored Lambda Version](#ch05.html#id652)

  * misconfigured, [Deploying a Lambda Function with an Overly Permissive IAM Role](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc12)\-[Deploying a Lambda Function with an Overly Permissive IAM Role](#ch05.html#id632)

  * outbound network access, exfiltrating sensitive data via, [Exfiltrating Sensitive Data via Code Injection and Outbound Network Access](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc12)\-[Attacking the Vulnerable Serverless Web Application](#ch06.html#id694)

  * privilege escalation via, [Escalating Privileges and Establishing Persistence Through an AWS Lambda Function with a Misconfigured IAM Role](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc15)\-[Establishing Persistence via a Backdoored Lambda Version](#ch05.html#id655)

  * S3 bucket configuration for static website hosting, [Configuring an S3 Bucket for Static Website Hosting](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc3)\-[Configuring an S3 Bucket for Static Website Hosting](#ch06.html#id673)

  * S3 bucket misconfigurations, testing for, [Testing for Common S3 Bucket Misconfigurations](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc7)\-[Testing for Common S3 Bucket Misconfigurations](#ch06.html#id676)

  * server receiving exfiltrated data, setup of, [Setting Up the Server That Receives Exfiltrated Data](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc16)\-[Setting Up the Server That Receives Exfiltrated Data](#ch06.html#id691)

  * serverless web application, preparing/deploying, [Preparing and Deploying a Vulnerable Serverless Web Application](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc1)\-[Configuring an S3 Bucket for Static Website Hosting](#ch06.html#id674)

  * stealing Secrets Manager secrets through vulnerable functions, [Stealing Secrets Manager Secrets Through a Vulnerable Lambda Function](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc18)\-[Stealing Secrets Manager Secrets Through a Code Injection Attack](#ch06.html#id704)

  * updating execution roles, [Updating the Lambda Function Execution Role with the Required VPC Permissions](#ch07.html#ix_ch07-running-and-securing-serverless-functions-in-a-vpc-asciidoc1)\-[Updating the Lambda Function Execution Role with the Required VPC Permissions](#ch07.html#id707)

  * updating via API gateway endpoint URL, [Updating the S3-Hosted Website to Use the API Gateway Endpoint URL](#ch07.html#ix_ch07-running-and-securing-serverless-functions-in-a-vpc-asciidoc13)\-[Updating the S3-Hosted Website to Use the API Gateway Endpoint URL](#ch07.html#id743)

  * vulnerable AWS Lambda function setup, [Setting Up a Vulnerable AWS Lambda Function](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc2)\-[Setting Up a Vulnerable AWS Lambda Function](#ch06.html#id667)
