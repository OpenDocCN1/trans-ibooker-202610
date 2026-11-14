
# Creating Your First Bare-Bones Pipeline

## Installing Git

Run the appropriate commands to install Git, the distributed version control system. The Git CLI allows you to interact with Git repositories.

Darwin:

```
brew install git
```

Linux:

```
sudo yum install git -y
```

Windows:

```
choco install git
```

After installation, complete the configuration to associate your personal settings. Then, run a command to test for a response from the Git CLI. For more information or troubleshooting, visit the [official site](https://git-scm.com/doc).

## Installing Kubernetes

Run the appropriate commands to install the Kubernetes CLI, which will allow you to interact with Git repositories.

Darwin:

```
brew install kubectl
```

Linux:

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Windows:

```
choco install kubernetes-cli
```

After the installation is complete, test for a response from the Kubernetes CLI. For more information or troubleshooting, visit the [official site](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux).

Now that you’ve installed and verified your tools, it’s time to create a pipeline.

Begin creating your pipeline by downloading the latest version of the sample code:

```
git clone <BOOK_SAMPLE_CODE_REPO_URL>
```

This command will create an AWS Service Catalog portfolio to hold all the AWS Service Catalog infrastructure and cluster products:

```
aws cloudformation create-stack \
       --template-body file://ServiceCatalogPortfolio.yml \
       --stack-name EKSPortfolio \
       --parameters file://parameter-files/ServiceCatalog.json \
       --capabilities CAPABILITY_NAMED_IAM \
       --region <INPUT_YOUR_REGION>
```

>Add s3 bucket creation with KMS key

Next, upload all the AWS Service Catalog files and the AWS Lambda function code:

```
aws s3 cp service_catalog_products/ \
s3://<s3_bucket_for_sc_products_lambda_function>/ \
--recursive \
--exclude "*" \
--include "*.yml" \
--include "*.zip" \
--region <INPUT_YOUR_REGION>
```

Then, provision the AWS Service Catalog networking resources:

```
aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSNetwork \
--parameters file://parameter-files/NetworkProduct.json \
--region <INPUT_YOUR_REGION>
```

The following command will provision the AWS Identity Access Management (IAM) product role resource, which integrates with the AWS Service Catalog:

```
aws cloudformation create-stack \
       --template-body file://ServiceCatalogProduct.yml \
       --stack-name EKSIAM \
       --parameters file://parameter-files/IAMProduct.json \
       --region <INPUT_YOUR_REGION>
```

Provision the AWS Service Catalog EKS Nodegroup product resource:

```
aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSIAM \
--parameters file://parameter-files/IAMProduct.json \
--region <INPUT_YOUR_REGION>
```

Provision the AWS Service Catalog EKS Lambda product resource:

```
aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSLambda \
--parameters file://parameter-files/EKSLambdaProduct.json \
--region <INPUT_YOUR_REGION>
```

Provision the AWS Service Catalog EKS Virtual Private Cloud (VPC) endpoint product resource:

```
aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSVPCENDP \
--parameters file://parameter-files/VPCEndpointProduct.json \
--region <INPUT_YOUR_REGION>
```

Provision the AWS Service Catalog EKS control plane logging product resource:

```
aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSLOG \
--parameters file://parameter-files/EKSLoggingProduct.json \
--region <INPUT_YOUR_REGION>
```

## Summary

[Figure 2-1](#ch02.xhtml#fig_1_infrastructure_as_code_pipeline) diagrams the environment you are deploying in your AWS account.

![](assets/image-to-come.png)

Let’s look at the pieces of this pipeline, corresponding to the numbers in [Figure 2-1](#ch02.xhtml#fig_1_infrastructure_as_code_pipeline):

1.  Your laptop or terminal to upload and download code
2.  The AWS Service Catalog manages logical products and modularized code
3.  AWS IAM role to allow access the the EKS cluster
4.  Amazon CloudWatch for logging and metrics
5.  AWS Lambda container images and Amazon ECR
6.  The EKS managed cluster
7.  The Python application deployed in the the EKS cluster

Take a few minutes to look around and get familiar with your environment. Now that you’ve set up your infrastructure, you’re ready to begin securing it. Chapter 3 will get you started.

# Chapter 3. Securing Your Infrastructure

You likely already know that cloud infrastructure can be spun up and down considerably faster than traditional datacenter infrastructure. Cloud providers use declarative languages to help end users configure the infrastructure they spin up. For example, AWS uses CloudFormation.

In this book, for the sake of simplicity, we’ll be using CloudFormation code. However, you should know that there are other IaC tools–such as Cloud Development Kit (CDK) and Hashicorp’s Terraform–that use different writing and deployment formats, but achieve the same goal of creating infrastructure through code.

## What Makes Infrastructure Secure?

What does it mean to “secure” your infrastructure? IaC is code, and just like you secure your application’s code, you need to plan to secure your IaC. The top three priorities involved in securing IaC are, in order of importance:

1.  Preventing unwanted access to your code
2.  Detecting misconfigurations in the infrastructure to be deployed
3.  Preventing misconfigured code from being deployed

In this chapter, we’ll explore all three priorities in detail.

### Note

We are assuming that you have an intermediate understanding of how to use Git-based version control and will not be covering Git troubleshooting or branching strategy. For more on those topics, we recommend reading [*Head First Git*](https://learning.oreilly.com/library/view/head-first-git/9781492092506/) by Raju Gandhi (O’Reilly).

We also delve into topics, like identity and access management (IAM). We recommend reading the [AWS documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.xhtml) to understand IAM, if you are not familiar with it.

## Hands Off! Preventing Unwanted Access With IAM Permissions

Always store your IaC files in a remote, versioned repository as you integrate changes. AWS has its own native Git-based version control system, known as CodeCommit, which we’ll be using throughout this book. Now, how do you secure this repository from unwanted access?

*Identity and access management* (IAM) is the core method of assigning permissions to resources in AWS accounts. Anyone who needs to create, update, or delete files within your CodeCommit repository should–at the bare minimum–have the permissions laid out in the IAM policy. This policy allows CodeCommit actions to be taken in your specific repository only if it is in the correct region: in this case, as shown below, only end users in the `us-east-2` region can perform actions.

The second statement below allows you to list all CodeCommit repositories. We have separated the two statements because you cannot use `ListRepositories` for a specific resource.
