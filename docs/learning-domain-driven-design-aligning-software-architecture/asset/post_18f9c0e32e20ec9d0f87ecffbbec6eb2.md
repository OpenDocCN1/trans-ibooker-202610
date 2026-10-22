
## Chapter 16. Data Mesh

So far in this book, we have discussed models used to build operational systems. Operational systems implement real-time transactions that manipulate the system’s data and orchestrate its day-to-day interactions with its environment. These models are the online transactional processing (OLTP) data. Another type of data that deserves attention and proper modeling is online analytical processing (OLAP) data.

In this chapter, you will learn about the analytical data management architecture called data mesh. You will see how the data mesh–based architecture works and how it differs from the more traditional OLAP data management approaches. Ultimately, you will see how domain-driven design and data mesh accommodate each other. But first, let’s see what these analytical data models are and why we can’t just reuse the operational models for analytical use cases.

### Analytical Data Model Versus Transactional Data Model

They say knowledge is power. Analytical data is the knowledge that gives companies the power to leverage accumulated data to gain insights into how to optimize the business, better understand customers’ needs, and even make automated decisions by training machine learning (ML) models.

The analytical models (OLAP) and operational models (OLTP) serve different types of consumers, enable the implementation of different kinds of use cases, and are therefore designed following other design principles.

Operational models are built around the various entities from the system’s business domain, implementing their lifecycles and orchestrating their interactions with one another. These models, depicted in Figure 16-1, are serving operational systems and hence have to be optimized to support real-time business transactions.

![](img/b742a71dea5d5dd509847f82600a6ad1_356_0.png)

Figure 16-1. A relational database schema describing the relationships between entities in an operational model

Analytical models are designed to provide different insights into the operational systems. Instead of implementing real-time transactions, an analytical model aims to provide insights into the performance of business activities and, more importantly, how the business can optimize its operations to achieve greater value.

From a data structure perspective, OLAP models ignore the individual business entities and instead focus on business activities by modeling fact tables and dimension tables. We’ll take a closer look at each of these tables next.

### Fact Table

Facts represent business activities that have already happened. Facts are similar to the notion of domain events in the sense that both describe things that happened in the past. However, contrary to domain events, there is no stylistic requirement to name facts as verbs in the past tense. Still, facts represent activities of business processes. For example, a fact table `Fact_CustomerOnboardings` would contain a record for each new onboarded customer and `Fact_Sales` a record for each committed sale. Figure 16-2 shows an example of a fact table.

|  | Fact_SolvedCases |
|---|---|
| PK | CaseId |
| FK | AgentKey |
| FK | CategoryKey |
| FK | OpenedOnDateKey |
| FK | ClosedOnDateKey |
| FK | CustomerKey |

Figure 16-2. A fact table containing records for cases solved by a company's support desk

Also, similar to domain events, fact records are never deleted or modified: analytical data is append-only data: the only way to express that current data is outdated is to append a new record with the current state. Consider the fact table Fact_CaseStatus in Figure 16-3. It contains the measurements of the statuses of support requests through time. There is no explicit verb in the fact name, but the business process captured by the fact is the process of taking care of support cases.

| Fact_CaseStatus |  |  |  |  |  |
|---|---|---|---|---|---|
| CaseId | Timestamp | AgentKey | CategoryKey | CustomerKey | StatusKey |
| case-141408202228 | 2021-06-15 10:30:00 |  | 12 | 10060512 | 1 |
| case-141408202228 | 15/06/2021 11:00:00 | 285889 | 12 | 10060512 | 2 |
| case-141408202228 | 15/06/2021 11:30:00 | 285889 | 12 | 10060512 | 2 |
| case-141408202228 | 15/06/2021 12:00:00 | 285889 | 12 | 10060512 | 3 |
| case-141408202228 | 15/06/2021 12:30:00 | 285889 | 12 | 10060512 | 2 |
| case-141408202228 | 15/06/2021 13:00:00 | 285889 | 12 | 10060512 | 4 |

Figure 16-3. A fact table describing state changes during the lifecycle of a support case

Another significant difference between the OLAP and OLTP models is the granularity of the data. Operational systems require the most precise data to handle business transactions. For analytical models, aggregated data is more efficient in many use cases. For example, in the Fact_CaseStatus table shown in Figure 16-3, you can see that the snapshots are taken every 30 minutes. The data analysts working with the model decide what level of granularity will best suit their needs. Creating a fact record for each change of the measurement—for example, each change of a case’s data—would be wasteful in some cases and even technically impossible in others.

### Dimension Table

Another essential building block of an analytical model is a dimension. If a fact represents a business process or action (a verb), a dimension describes the fact (an adjective).

The dimensions are designed to describe the facts’ attributes and are referenced as a foreign key from a fact table to a dimension table. The attributes modeled as dimensions are any measurements or data that is repeated across different fact records and cannot fit in a single column. For example, the schema in Figure 16-4 augments the SolvedCases fact with its dimensions.

![](img/b742a71dea5d5dd509847f82600a6ad1_360_0.png)

Figure 16-4. The SolvedCases fact surrounded by its dimensions

The reason for the high normalization of the dimensions is the analytical system’s need to support flexible querying. That’s another difference between operational and analytical models. It’s possible to predict how an operational model will be queried to support the business requirements. The querying patterns of the analytical models are not predictable. The data analysts need flexible ways of looking at the data, and it’s hard to predict what queries will be executed in the future. As a result, the normalization supports dynamic querying and filtering, and grouping the facts data across the different dimensions.

### Analytical Models

The table structure depicted in Figure 16-5 is called the *star schema*. It is based on the many-to-one relationships between the facts and their dimensions: each dimension record is used by many facts; a fact’s foreign key points to a single dimension record.

![](img/b742a71dea5d5dd509847f82600a6ad1_361_0.png)

*Figure 16-5. The many-to-one relationship between facts and their dimensions*

Another predominant analytical model is the snowflake schema. The snowflake schema is based on the same building blocks: facts and dimensions. However, in the snowflake schema, the dimensions are multilevel: each dimension is further normalized into more fine-grained dimensions, as shown in Figure 16-6.

As a result of the additional normalization, the snowflake schema will use less space to store the dimension data and is easier to maintain.

However, querying the facts’ data will require joining more tables, and therefore, more computational resources are needed.

Both the star and snowflake schemas allow data analysts to analyze business performance, gaining insights into what can be optimized and built into business intelligence (BI) reports.

![](img/b742a71dea5d5dd509847f82600a6ad1_362_0.png)

Figure 16-6. Multilevel dimensions in the snowflake schema

### Analytical Data Management Platforms

Let’s shift the discussion from analytical modeling to data management architectures that support generating and serving analytical data. In this section, we will discuss two common analytical data architectures: data warehouse and data lake. You will learn the basic working principles of each architecture, how they differ from each other, and the challenges of each approach.
