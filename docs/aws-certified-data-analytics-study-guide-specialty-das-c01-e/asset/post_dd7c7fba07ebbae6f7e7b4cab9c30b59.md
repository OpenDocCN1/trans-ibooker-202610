
For example, we covered Amazon EMR in this chapter and covered AWS Glue in Chapter 2, “Data Collection.” Both of these services offered Apache Spark, one of the most popular data processing frameworks available. Similarly, we covered data orchestration services, and there are a number of orchestration services on the AWS platform. Being a solution architect means you should be able to guide your customers and users to the right technology based on the use case.

Once you are done, please take some rest before we start with the next chapter, “Data Visualization.”

## Exam Essentials

**Understand data processing and data analysis.** Data processing and data analysis are the topics of more than one third of all exam questions, which makes this chapter super important. The main reason for covering these two major domains in one chapter was that I feel that a lot of technologies used for data processing are used for data analysis as well.

**Become familiar with the pertinent Amazon service offerings.** From a services perspective, we’ve looked at Amazon Athena, Apache Presto, Amazon EMR, Apache Hive, Amazon Elasticsearch, Amazon Redshift, and Kinesis Data Analytics. There was a lot of content covered in this chapter, but while this is quite important, it will not suffice on its own. It is highly recommended that you look at the blogs, documentation, and workshops suggested in the references section of this chapter.

## Exercises

For assistance in completing the following exercises, refer to the user guides of the associated services.

### EXERCISE 4.1: Creating an EMR Cluster

Please follow these steps to create an EMR cluster.

1. Go to Service ➤ Analytics ➤ EMR.
2. Select Create Cluster.
3. You have two options to create a cluster: Quick Create or Advanced Options (provides more control and choice). Select Quick Create.
4. Give your cluster a name, e.g., wiley-demo-chapter4.
5. You can check the Logging option. This will log all cluster logs to Amazon S3.
6. Choose the launch mode.

You have two models of launching a cluster:

- Cluster – You create a long-running cluster with a set of applications chosen from the list of apps in the next step.
- Step Execution – With this option, EMR will create a cluster, execute the added steps, and terminate when the steps have completed.

7. For the purpose of this exercise, we will choose the Cluster mode of operation.
8. Choose a software configuration. Select EMR Release. At the time of this writing, the latest release was emr-5.28.0.

You can select from a list of applications to be configured on the cluster that is being spun up:

- Core Hadoop: Hadoop 2.8.5 with Ganglia 3.7.2, Hive 2.3.6, Hue 4.4.0, Mahout 0.13.0, Pig 0.17.0, and Tez 0.9.2
- HBase: HBase 1.4.10 with Ganglia 3.7.2, Hadoop 2.8.5, Hive 2.3.6, Hue 4.4.0, Phoenix 4.14.3, and ZooKeeper 3.4.14
- Presto: Presto 0.227 with Hadoop 2.8.5 HDFS and Hive 2.3.6 Metastore
- Spark: Spark 2.4.4 on Hadoop 2.8.5 YARN with Ganglia 3.7.2 and Zeppelin 0.8.2

9. For the sake of this exercise, we’ll choose Core Hadoop.

You also have the option to choose AWS Glue Data Catalog for Table Metadata. This will provide an option of using an external Hive metastore that you can use with these applications.

10. Hardware configuration:

You have to choose the hardware configuration for your cluster, which includes the following settings:

- Type of an instance
- Number of instances (One of the instances will be a master node, and the remaining will act as core nodes.)

11. Security & Access configuration:

- You can choose an EC2 key pair. If you don’t select an EC2 key pair, you won’t be able to SSH into your master node.
- You can choose from two levels of permissions:
  - Default Permission – This will use default IAM roles. If roles are not present, they will be automatically created for you with managed policies for automatic policy updates.
    1. EMR role – EMR_DefaultRole
    2. EC2 instance profile – EMR_EC2_DefaultRole
  - Custom Permission – You can select custom roles to tailor permissions for your cluster.
    1. Select an EMR role.
    2. Select an EC2 instance profile.

12. Select the Create Cluster option. It will take around 5–7 minutes to spin up a cluster.

### EXERCISE 4.2: Getting Started - Configuring and Testing an Elasticsearch domain

Please complete the following tutorial to use Amazon Elasticsearch Service to create and configure a test domain:

```
amzn.to/39lh9gt
```

### EXERCISE 4.3: Getting Started - Hosting an Elasticsearch index

In the following lab, you will use Amazon Elasticsearch Service to create a domain and host two indexes in Amazon Elasticsearch. You will then use various tools within the AWS platform like ElastiCache, Logstash, and Kibana and get an understanding of a real-world example.

```
search-sa-log-solutions.s3-us-east-2.amazonaws.com/logstash/html/Lab_Guide_ABD326.html
```

```
bit.ly/ElasticSearchLab
```

### EXERCISE 4.4: Building a Log Analytics Solution

In this lab, you will build a log analytics solution using Kinesis Agent, Kinesis Firehose, Kinesis Analytics, Amazon Elasticsearch, and Kibana. The exercise should take 60 minutes to complete.

```
aws.amazon.com/getting-started/projects/build-log-analytics-solution
```

## Review Questions

1. You are working as an Enterprise Architect for a large fashion retailer based out of Madrid, Spain. The team is looking to build ETL and have large datasets that need to be transformed. Data is arriving from a number of sources and hence de-duplication is also an important factor. Which of the following is the simplest way to process data on AWS?

- A. Load data into Amazon Redshift, and build transformations using SQL. Build custom de-duplication script.
- B. Using AWS Glue to transform the data using built-in FindMatches ML Transform
- C. Load data into Amazon EMR, build Spark SQL scripts, and use custom de-duplication script.
- D. Use Amazon Athena for transformation and de-duplication.

2. Which of the following is a distributed data processing option on Apache Hadoop and was the main processing engine until Hadoop 2.0?

- A. MapReduce
- B. YARN
- C. Hive
- D. ZooKeeper

3. You are working as a consultant for a telecommunications company. The data scientists have requested direct access to the data to dive deep into the structure of the data and build models. They have good knowledge of SQL. Which of the following tools will you choose to provide them direct access to the data and reduce the infrastructure and maintenance overhead, while ensuring that access to data on Amazon S3 can be provided? Which of the following would you recommend to them? (Choose One.)

- A. Amazon S3 Select
- B. Amazon Athena
- C. Amazon Redshift
- D. Apache Presto on Amazon EMR

4. Which of the following file formats are supported by Amazon Athena? (Choose Three.)

- A. Apache Parquet
- B. CSV
- C. DAT
- D. Apache ORC
- E. Apache Avro
- F. TIFF

- C. Apache Pig on Amazon EMR
- D. Amazon Elasticsearch

9. You are looking to build a Data warehouse solution with the ability to flexibly transfer your data between data lake and Data warehouse. Which of the following is the most cost effective way to meet your requirements? (Choose One.)

- A. Use S3 as your Data lake and Amazon EMR as your Data warehouse.
- B. Use HDFS as your Data lake and Amazon Redshift as your Data warehouse.
- C. Use S3 as your Data lake and Amazon Redshift as your Data warehouse.
- D. Use HDFS as your Data lake and Amazon EMR as your Data warehouse.

10. Which of the following statements are true about Redshift Leader nodes? (Choose Two.)

- A. Redshift cluster can have a single leader node.
- B. Redshift cluster can have more than one leader node.
- C. Redshift Leader node should have more memory than the compute nodes.
- D. Redshift Leader node has the exact same specifications as the compute nodes.
- E. You can choose your own Leader node sizing and it is priced separately.
- F. Redshift leader node is chosen automatically and is free to the users.

## References

### Recommended Workshops

- Data Ingestion and Processing workshop: dataprocessing.wildrydes.com
- Incremental Data Processing on Amazon EMR (Apache Hudi): incremental-data-processing-on-amazonemr.workshop.aws/en
- Serverless Data Lake workshop: incremental-data-processing-on-amazonemr.workshop.aws/en
- Data Engineering 2.0: aws-dataengineering-day.workshop.aws/en
- Amazon Athena workshop: athena-in-action.workshop.aws
- Amazon EMR with Service Catalog: s3.amazonaws.com/kenwalsh testad/cfn/public/sc/bootcamp/emrloft.html
- Realtime Analytics and Serverless DataLake Demos: demostore.cloud
- Streaming Analytics workshop: streaming-analytics.workshop.aws/en
