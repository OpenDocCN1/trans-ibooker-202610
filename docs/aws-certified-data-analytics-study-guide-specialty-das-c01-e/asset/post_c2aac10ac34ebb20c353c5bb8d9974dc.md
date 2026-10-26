
realizes that a particular customer is going to churn (https://tinyurl.com/y9ho4kbu), they may devise policies to retain the customer using various marketing campaigns. We’ll cover descriptive, diagnostic, and predictive analytics later in this book.

### FIGURE 1.3 Business analytics spectrum

Source: Gartner Analytics Maturity Model. https://www.zdnet.com/article/a-guide-for-prescriptive-analytics-the-art-and-science-of-choosing-and-applying-the-right-techniques/

![](img/fc6a7083d65b0716921d7bd4a5298460_50_0.png)

While prescriptive analytics provides recommendations on what needs to be done, the details are out of scope for this book.

## The Big Data Reference Architecture

We had a look at the big data pipeline in an earlier section. It is time to now start looking at the Big Data Reference architecture and the AWS and partner components that fit the analytics ecosystem. AWS re:Invent is often a source of some of the most amazing content you will come across on Analytics in general and AWS Analytics ecosystem in particular. Figure 1.4 has been adapted from Siva Raghupathy’s talk at re:Invent 2017 showing the various stages of the analytics pipeline and also maps the AWS services to the various stages of the pipeline.

### FIGURE 1.4 AWS analytics architecture

![](img/fc6a7083d65b0716921d7bd4a5298460_51_0.png)

## Data Characteristics: Hot, Warm, and Cold

Choosing the right technology for processing depends on the characteristics of data, which can vary from cold to hot. You may have heard of the term multitemperature data management, where the temperature of the data is determined by the frequency with which the data is accessed, the frequency at which the data is updated and maintained, the levels of durability needed, and the depths of history you would like to maintain.

Let us look at an example of a financial institution that has data for all its customers: their online profile, the transactions that they have done over the years, and so forth. Consider the following set of transactions: The customer is going on Amazon.com to purchase some essential items for its daily use. Any financial institution would carry fraud detection on individual transactions; hence, your use case here not only requires you to capture the data in real time, but also to identify if the transaction is fraudulent or not, based on the customer and the transaction profile. The fact that you need to not only capture the transaction in real time but also perform an analytics operation on it indicates that your data collection technology will be defined by the use case, which in this case requires you to do stream processing, and your processing technology will also be defined by the use case, which defines what temperature of data you are dealing with.

As your time to action is relatively lower, this is the case for hot data, and hence the technological choice will be defined where the selected technologies meet your demands of low latency and high-speed access.

Let us consider another transaction or sets of transactions that happened over fourteen years ago. The customer has made a complaint to a regulatory authority about overcharging (such as payment protection insurance, or PPI claims in the UK), and you have been given six weeks to respond to the complaint or reimburse the customer based on this claim.

- High durability
- Intelligent tiering (moving the data from high-durability/high-cost to low-durability/low-cost automatically)
- Secure
- Low cost per gigabyte

Some customers tend to look at Apache Hadoop Distributed File System (HDFS) on a Hadoop cluster as a persistent store; however, there are certain cases where HDFS can be used for the hottest datasets (e.g., when working iteratively on a certain dataset), and Amazon S3 can be used for frequently accessed data. We’ll discuss this in more detail in Chapter 4, “Data Processing and Analysis.”

For the warm and hot data, Amazon supports various options that fall into three major categories:

- **SQL-Based Systems**  Amazon offers Amazon RDS, which is a managed relational database service for SQL-based operations to store the data.

- **NoSQL-Based Systems**  Amazon offers Amazon DynamoDB and Amazon DocumentDB, which are managed NoSQL services. We will learn more about them in Chapter 3, “Data Storage.”

- **Cache Repositories**  Amazon also offers Amazon ElastiCache, which is a managed Memcached and Redis service, and Amazon DynamoDB Accelerator (DAX), which is an in-memory cache for DynamoDB to offer low latency and high throughput.

As discussed earlier in this chapter, it is important to pick the right tool for the right job. The decade-old tradition of trying to retrofit every use case in a relational engine, a NoSQL engine, or any other technology that you have had previous success with on a different use case makes things complicated not only from initial implementation but for long-term management as well. The abundance of managed solutions and partner products from the marketplace on the AWS platform allows you to pick the right tool for the job. Your application architecture can now benefit from a variety of in-memory, graph, NoSQL, SQL, and search technologies, thus allowing you to focus on the correct long-term architecture rather than working around the technological limits to ensure that you can focus on the true essence of your business problem. The amount of integration effort that organizations have spent in retrofitting their use cases to a technological limitation has been huge, and the breadth and depth of the AWS ecosystem allows us to shun these anti-patterns.

## Process/Analyze

Once the data has been ingested and stored based on the optimum storage option, you can either process the data for downstream analytics or run analysis as is if the data is in a suitable format. The types of processing and analytics depends on a number of factors but can be categorized into the following categories:

- **Batch Processing and Analytics** Batch processing includes workloads that take minutes to hours to run and often include things like daily, weekly, or monthly reports. For batch processing, the technologies that generally work well include Amazon EMR. Customers often use AWS Glue, which is a fully managed (serverless) ETL service that makes it simple and cost effective to categorize your data.

- **Interactive Analytics** Interactive analytics includes workloads that take seconds, and examples include self-service dashboards built using technologies like Amazon QuickSight or technologies like Tableau and ad hoc access with Amazon Athena and Amazon EMR with Presto and Spark.

- **Stream Processing and Analytics** Stream processing analytics includes workloads like fraud detection where the latency is seconds to milliseconds and includes technologies like Spark Streaming (within Amazon EMR), Amazon Kinesis Analytics, KCL, AWS Lambda, Apache Flink, and Kafka.

- **Predictive Analytics** Predictive analytics includes workloads like churn prediction, fraud detection, anomaly detection, demand forecasting, and speech recognition with response time from milliseconds (real-time predictive analytics) to minutes (batch) using technologies like Lex, Polly, Amazon Rekognition, and Amazon machine learning (ML) and deep learning AMIs with frameworks like MxNet, TensorFlow, Theano, Torch, CNTK, and Caffe.

## Consumption

Once an entire data pipeline is established to capture, store, and process the data, you then have a number of options to consume the data in various forms. There are various personas who consume the data and insights generated on AWS, and it is important to understand what those personas are and how they consume the information and insights generated on your analytical platform.

- **Developers and Operations** Developers and operations teams often develop, manage, and run apps on EC2 or ECS containers consuming the data assets generated via the data pipeline.

- **Data Scientists** Data scientists work with the data to understand patterns within it, build models, and deploy models in production. They like to work with interactive interfaces like Notebooks (Jupyter, Zeppelin), SageMaker, and other data science platforms and IDEs.

  Data scientists also like to work directly with APIs (e.g., Spark DataFrame API, Glue DynamicFrame API) to work natively with them and have more control over how the data engineering and data modeling happens.

- **Business Users** Business users consume the data to build reports and dashboards using a variety of tools available on AWS, such as first-class services like Kibana or QuickSight or tools available on the AWS marketplace like Tableau, Qlik, MicroStrategy, and Looker.
