

![](img/7a3ef505ef42a8f03ad37f9a013ac15a_51_0.png)

*Figure 2-7. Real-time stream processing pipelines*

## Cloud Data Warehouses

A *cloud data warehouse* is an enterprise data warehouse offered as a managed service (PaaS) on public clouds with optimized integrations for data ingestion, analytics processing, and BI analytics. *BI analytics* refers to tools that support visualization and interactive querying capabilities. Cloud data warehouse offerings are designed to elastically scale with the customer’s growing needs by abstracting the infrastructure away from the user, and they promise faster performance and lower cost of ownership than traditional on-premises data warehouses. Let’s take a look at some of the most popular cloud data warehouse offerings:

### Amazon Redshift

Amazon Redshift was the first popular cloud data warehouse offering on the public cloud. You can provision a Redshift cluster, where you can specify the number of compute nodes that you want. The cluster can support data at PB scale according to the product documentation. You can use PostgreSQL, a popular query language, to query data in your Redshift cluster. To learn more about Redshift, you can visit the [product page](#). Redshift also announced the capability to [share data across different Redshift clusters without copying data to promote sharing of data and insights](#).

### Google BigQuery

Unlike Redshift, where you provision your data warehouse as a cluster, Google BigQuery is a completely serverless, highly scalable data warehouse solution that abstracts the details of cluster management completely away from the customer. In addition, BigQuery has features like **BigQuery Omni** that allow you to use the BigQuery compute service across other clouds, such as AWS and Azure. To learn more about BigQuery, visit the [product page](product page).

### Azure Synapse Analytics

Azure Synapse Analytics is offered as a unified analytics platform on Microsoft Azure. Similar to Redshift, you can provision a data warehouse cluster and specify the number of nodes you want for your scenarios. You can also provision a Spark cluster for analytics scenarios in the same experience. In addition, you can run serverless queries in SQL or Spark. With serverless queries, you can simply submit a job without provisioning a cluster, similar to BigQuery. Azure Synapse Analytics also offers integrations in the same experience with other Azure services, such as Azure Machine Learning, Azure Cognitive Services, and Power BI. To learn more about Azure Synapse Analytics, visit the [product page](product page).

### Snowflake Data Cloud

Snowflake data warehouse is a managed data warehouse solution that is available on all public clouds—AWS, Azure, and GCP. Designed as a truly scalable solution, Snowflake is offered as a single service, while the implementation runs on a disaggregated compute and storage architecture, making it highly scalable in the compute or storage dimension without compounding the cost. This disaggregation also lets you spin up different virtual warehouses that can access the same data, providing isolation between your different query scenarios. Snowflake also offers data sharing at a table and object level to other Snowflake accounts. To read more about Snowflake, visit the [product page](product page).

In this section, I presented a high-level overview of the concepts of the four components of a cloud data lake architecture: the data, the data lake storage, the compute engines, and the cloud data warehouse. I also provided an overview of the commonly used services and technologies, along with links for you to explore deeper. In the next section, I will go over cloud data lake architectures that represent different ways in which these building blocks can be assembled in a solution. As we are reading this book, rapid innovations are being made in both the data lake and the data warehouse offerings, blurring the boundaries between them. We will address this further in the data lakehouse architecture pattern.

Data in the cloud data lake architecture can serve many purposes. However, there are two major scenarios that are common consumption patterns in an organization:

### *Business intelligence*

Data is used by BI analysts to create dashboards or work on interactive queries to answer key business problems that are well defined and work on highly structured data.

### *Data science and machine learning*

Data is used by data scientists and machine learning engineers to do exploratory analysis and experimental work to answer complex problems that don’t have a defined ruleset and require multiple iterations to get better. The data involved here assumes no structure.

## Modern Data Warehouse Architecture

In a modern data warehouse architecture, both the data lake and the data warehouse peacefully coexist, each serving a distinct purpose. The data lake serves as low-cost storage for a large amount of data and supports exploratory scenarios such as data science and machine learning. The data warehouse stores high-value data and powers dashboards used by the business. It is also used by BI users to query the highly structured data to gain insights about the business.

### Reference Architecture

Data is first ingested into a data lake from various sources: on-premises databases, social media feeds, and so on. This data is then transformed using big data analytics frameworks like Hadoop and Spark, where multiple datasets can be aggregated and filtered to generate high-value structured data. This data is then loaded into a cloud data warehouse to power dashboards, including interactive dashboards for BI analysts using their very familiar tool of choice—SQL. In addition, the data lake empowers a whole new set of scenarios that involve exploratory analysis by data scientists as well as machine learning models that can be fed back into their applications. A simplified representation of the modern data warehouse architecture is shown in Figure 2-8.

![](img/7a3ef505ef42a8f03ad37f9a013ac15a_54_0.png)

Some questions you would naturally ask here are: why not use a cloud data warehouse directly? Why is a data lake necessary in between? If I only have structured data, do I even need a data lake? If I may say so myself, these are great questions. There are a few reasons why you would need a data lake in this architecture:

- Data lakes cost a lot less than a data warehouse and can act as your long-term repository of data. Remember that data lakes are typically used to store large volumes of data (think tens or hundreds of PBs), so the difference in cost is material.
- Data lakes support a variety of modern tools and frameworks around data science and machine learning that you can use to enable completely new scenarios.
- Data lakes let you future-proof your design to scale to your growing needs. As an example, you might use your initial data lake architecture to load data from your on-premises systems on a nightly basis and publish reports or dashboards for your BI users. This same architecture is extensible to support real-time data ingestion without having to rearchitect your solution.
- Data of all forms and structures is largely becoming relevant to organizations. Even if you are focused on structured data today, as you saw in the previous example, you may find value in all kinds of data, such as weather, social media feeds, and the like.

In case you didn’t observe this already, there is a difference in usage patterns that you need to remember: when you load data into a data warehouse, you use an *extract, transform, and load (ETL) pattern*, where you extract data from the source and transform the data to the format respected by the data warehouse before loading it into the data warehouse. In a data lake, you follow an *extract, load, and transform (ELT) pattern*, where you extract data from the source, load into the data lake as is, and then do your transformations.

### Sample Use Case for a Modern Data Warehouse Architecture

Let’s revisit our model company, Klodars Corporation. It will leverage the modern data warehouse architecture and start loading data from its operational databases into the data lake. Instead of continuing to store its backups on its on-premises systems, it can now create daily backups and store up to one year’s worth of backups (or more if it wants to) in its data lake. Klodars can do this while the operational databases on its servers continue to serve their existing applications, thereby ensuring the continuity of the company’s operations. In addition, Klodars will plan to load data from social media feeds that relate to rain gear and winter gear to analyze patterns. This architecture will also enable the company to load other data, such as its clickstream, into the data lake storage in real time using real-time ingestion techniques like Apache Kafka.

With the dataset up and ready to go, the data engineering team will use tools like Apache Spark to process the structured data from its database dumps and from the website clickstream to generate high-value data that shows the shopping and sales trends over time. The team will also process the social media feeds and extract data that pertains to rain gear and winter gear as well as any associated purchases that the same feeds indicate. This architecture will enable the data engineering team to generate high-value data on a scheduled basis (e.g., daily) on sales trends, inventory and supply, website-browsing trends, and social media trends around rain and winter gear. This data will then be loaded into the data warehouse and refreshed on a periodic basis (e.g., daily).

The data stored in the data warehouse is very high-value structured data. The business analysts will use this high-value data to build dashboards that show the sales trends quarter over quarter or month over month, so the sales teams can understand the trend of their sales and set projections for the upcoming time period. The business analysts can also slice and dice the data by factors like region, salespeople coverage, partners, and other attributes, so the leadership team can understand the growth drivers and make data-informed decisions about the company’s expansion

