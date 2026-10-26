![](bbox=[0.12, 0.12, 0.33, 0.27])

# Introduction

Security is a day one job at AWS, and it is our top priority. We have covered the entire analytics pipeline from data ingestion, to processing, to analysis, and finally to visualization. However, during the exam you will get a number of questions concerning authorization, authentication, compliance, and encryption, which in essence translates to securely accessing and working with your data and analytics services. Around 18 percent of the questions on the exam will be related to security, thus giving it the same weight as data collection, data analysis, and visualization.

During this chapter we will discuss the AWS shared responsibility model, before looking into the authentication and authorization aspects within Amazon Web Services and how they apply to various data and analytics services that are covered during the exam. We look at security of Amazon S3, Amazon DynamoDB, Amazon Redshift, Amazon Elasticsearch, Amazon EMR, AWS Glue, and Amazon Kinesis. Let’s get started.

> The AWS Data Analytics Specialty Certification exam will test you on various security scenarios around authentication and authorization. It is important to remember that while you will get choices around creating an AWS user or an AWS role to meet a certain security requirement, the general best practices of security—i.e., limiting access to the permissions needed to perform an operation, and creating a role vs. a user—apply to most of the scenarios.

# Shared Responsibility Model

A typical data platform built on the AWS cloud involves five to six different layers, including the global AWS infrastructure like AWS Regions such as us-east-1 (North Virginia), Availability Zones, and edge locations. This is the hardware and the global infrastructure that AWS manages for its customers. On top of the global infrastructure, we have various software components, such as compute, storage, databases, and networking, that connect these hardware components together. These layers essentially form the basis