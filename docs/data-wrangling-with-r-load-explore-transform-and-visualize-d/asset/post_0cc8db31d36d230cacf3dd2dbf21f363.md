
## Feature Correlation: Titanic

Linear feature correlation is based on Pearson's correlation. Numeric to categorical correlation is calculated by encoding the categorical features as the floating point numbers that best predict the numeric feature before calculating Pearson's correlation. Linear categorical to categorical correlation is not supported.

Numeric to numeric correlation is in the range [-1, 1] where 0 implies no correlation, 1 implies perfect correlation and -1 implies perfect inverse correlation.

Numeric to categorical and categorical to categorical correlations are in the range [0, 1] where 0 implies no correlation and 1 implies perfect correlation.

Features that are not either numeric or categorical are ignored.

The table below lists for each feature what is the most correlated feature to it. We display a correlation matrix for a dataset with up to 20 columns

|  | Most correlated feature | Correlation |
|---|---|---|
| survived (numeric) | boat (categorical) | 0.950845 |
| boat (categorical) | survived (numeric) | 0.950845 |
| pclass (numeric) | boat (categorical) | 0.608416 |
| fare (numeric) | pclass (numeric) | -0.565255 |
| sex (numeric) | boat (categorical) | 0.556174 |
| age (numeric) | pclass (numeric) | -0.411391 |
| sibsp (numeric) | parch (numeric) | 0.374291 |
| parch (numeric) | sibsp (numeric) | 0.374291 |
| embarked (numeric) | boat (categorical) | 0.278391 |

Correlation matrix:

![](img/ec1933a597ce63afa3bcf7496364f412_163_0.png)

Figure 4.5 – Data analysis report: feature correlation

## Data export

So far, we’ve looked at Data Wrangler capabilities that enable you to import data into Data Wrangler and perform analysis and transformations. SageMaker Data Wrangler enables you to export all or part of these transformations as a data flow. In most cases, data processing consists of a series of transformations. Each of these transformations can be referred to as a step in Data Wrangler. A Data Wrangler flow is made up of a series of nodes that represent the import of your data and the transformations that you’ve performed. As we covered earlier, one of the first steps in Data Wrangler is to import data from a supported data source. As such, the data source is the first node in your data flow. Following the previous step, the next node in the data flow is the Data Types node. This node signifies that Data Wrangler has executed a transformation to convert the dataset into a format that is suitable for further analysis and processing. Each transformation that is added to the data flow is represented as a new node, enabling a visual representation of the sequence of operations performed on the dataset. Think of this data flow as a code file that includes all the code and sequences of your data transformations. Data Wrangler supports exporting these transformations to multiple supported destinations and formats that include Amazon S3, SageMaker Pipelines, SageMaker Feature Store, and as standard Python code. In the case of S3, Data Wrangler allows you to export your processed data to a bucket location in S3. You can specify this S3 location as a destination node or use the **Export** option to export the processed data or transformation results to S3. In addition to the exported data, the flow itself is saved to the S3 bucket location. When you choose S3 as the destination, you do that by adding a destination node, which is the S3 bucket location. Data Wrangler allows you to add multiple destination nodes. The export happens using a Data Wrangler job, which behind the scenes is a SageMaker processing job. When you create a Data Wrangler export job, you can specify the number of instances and a supported instance type. Data Wrangler also allows you to export to SageMaker Pipelines, which is a SageMaker feature for building end-to-end ML workflows that take advantage of direct SageMaker integration. You can export your entire flow or some of the steps from your data flow to SageMaker Pipelines. Data Wrangler will create a Jupyter notebook that you can use to define, instantiate, run, and manage a pipeline. Feature stores have gained prominence in ML. Features are specific columns in your dataset. A feature store contains processed data along with some associated metadata and acts as a central repository for all the processed data or features that can be used by various data science teams within their ML workflows. Data Wrangler enables you to export features you’ve created to Amazon SageMaker Feature Store, which is another SageMaker service to store and discover features. Using Data Wrangler, you can either update existing feature groups or create completely new feature groups in SageMaker Feature Store. You can use either a Jupyter notebook or a destination node to update your feature group with the observations in the dataset or create a feature group in Feature Store. Finally, you can also export all the transformations in your data flow as Python code that you can integrate with any data processing workflow. Since the exported code will have references to SageMaker and AWS resources, you will need to make sure that the environment or workflow with which you integrate has access to the AWS resources referenced.

## SageMaker Studio setup prerequisites

SageMaker Data Wrangler is available as a service within Amazon SageMaker Studio. While you can still use some of the SageMaker Data Wrangler features via APIs, for the purposes of this book, we will be using Data Wrangler from within SageMaker Studio. In this section, we will cover a brief overview of SageMaker Studio and how to set up a SageMaker Studio domain and users in your AWS account.

### Prerequisites

Before we can start setting up SageMaker Studio, there are a few prerequisites, as follows:

- An AWS account.
- An **Identity and Access Management (IAM)** role with the appropriate policy and permissions attached. There is an `AmazonSageMakerFullAccess` AWS managed policy that you can use as is or as a starting point to create your custom policy.

### Studio domain

You will start by creating and onboarding a SageMaker domain using the AWS console. A SageMaker domain includes an **Amazon Elastic File System (Amazon EFS)** volume, a list of authorized studio users, and a variety of security, application, policy, and **Amazon Virtual Private Cloud (Amazon VPC)** configurations. When you are creating a domain, you can choose how you want to onboard users on the domain. You have two options: use AWS IAM Identity Center (successor to **AWS Single Sign-On**, or **AWS SSO**) or AWS IAM for authentication methods. Choose the IAM Identity Center option if you are integrating SageMaker Studio with an existing SSO or creating a new SSO setup; else, choose the IAM authentication option. It is important to remember that you cannot change this after the domain has been created unless you delete and recreate the domain. When you use IAM authentication, you can choose either the **Quick setup** or the **Standard setup** option for onboarding a domain. For the scope of this book, we will consider IAM authentication with **Quick setup**.

### Studio onboarding steps

Let’s look at the steps for studio onboarding:

- 1. Log in to your AWS account and navigate to the SageMaker console.
- 2. From the **Control panel** option on the left, choose **Studio**, as illustrated here:

![](img/ec1933a597ce63afa3bcf7496364f412_166_0.png)

Figure 4.6 – SageMaker console

- 3. Click on the Launch SageMaker Studio button. If this is your first time, you will be taken to the Setup SageMaker Domain page with options for Quick setup and Standard setup:

![](img/ec1933a597ce63afa3bcf7496364f412_166_1.png)

Figure 4.7 – Domain setup options

Quick setup is selected by default. Keep that unchanged.

- 4. Next, you need to provide a username for the default admin user and choose a default execution role for your domain, as illustrated in the following screenshot. If you do not already have a SageMaker execution role in your account, you can create one using the built-in policy mentioned in the Prerequisites section:
