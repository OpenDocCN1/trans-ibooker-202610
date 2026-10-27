
![](img/7a3ef505ef42a8f03ad37f9a013ac15a_97_0.png)

If you have heard the term *master data management* (MDM), this essentially refers to data about the data assets in the organization, including location, customer, products, and so on. This is different from the metadata that we saw in Chapter 2, which is data about the data in terms of describing the schema.

## Data producers

These are one segment of the users of the data lake. As the name suggests, data producers bring data into your data estate. This data could be raw (ingested from external and internal sources), enriched (prepared/cooked data that is in a tabular form), curated (summary/digest with high-value insights), or ad hoc datasets that are produced and consumed by data scientists. Data producers care deeply about ensuring that the data adheres to a certain level of quality depending on the type, and they want to ensure that they are aware of how the data needs to be locked down or opened up to the rest of the organization in terms of managing access. They need to adhere to the tools and processes set by the data officers and data engineers and not attempt to circumvent the guardrails.

## Data consumers

These are the other segment of users of the data lake. As with anything else, a product is valuable only when there are customers using it. Data is no exception. Whether they use dashboards, queryable tables, or ad hoc datasets, data consumers are anyone who consumes the data or insights. The data producers have control over who gets to use the data, and data consumers actually consume this data either as is or for further processing. For example, in the data curation process, the set of users or applications ends up consuming the enriched data and producing the curated data.

Figure 3-6 provides a very simplistic overview of some of the top-of-mind concerns for the various actors who interact with the data estate—in this case, the data lake.

A lot of tools and automations are available for these different actors to enable better data governance and follow best practices. Having said that, you can also conduct data governance with manual operations. Data governance can be categorized into the tasks that can be accomplished either with tools and automation or with manually enforced processes.

![](img/7a3ef505ef42a8f03ad37f9a013ac15a_98_0.png)

*Figure 3-6. Data governance components*

## Data Classification

Data governance always starts with the data officers, who work on defining the requirements and controls for the data that can be collected, stored, and processed.

*Data type* refers to the types of data or assets that the organization uses. The data in your data lake needs to be tagged with one or more of these types. The data type itself, also referred to as an *infotype*, has a singular meaning associated with it, such as data containing a first name or zip code. These infotypes are organized into data classes. For example, name and address fall into the personally identifiable information (PII) data class, and a credit card number falls into the financial information data class.

[Figure 3-7](#) illustrates this hierarchy with an example.

Policies are rules and restrictions that are applicable to the classes of data. For instance, your policy could be that consumers’ PII data must always be collected after the consumer consents to it once they clearly understand how your organization plans to use that information.

title or author. Similarly, data consumers can leverage the data catalog to look for the tables they have access to, either with the table name or with certain key fields. You might search for all the tables about employees, for example. The key thing to remember here is that one data catalog can hold data from various data sources, the data lake, the data warehouse, and any other storage systems. Figure 3-8 illustrates this concept.

![](img/7a3ef505ef42a8f03ad37f9a013ac15a_100_0.png)

*Figure 3-8. Data catalog*

Once key datasets and high-value insights are available to an organization, there may be other consumers either within the organization or, in some cases, even outside the organization. In other words, data is like a product that can be shared and monetized with a larger audience. *Data sharing* is an emerging capability of data governance where datasets in a data lake or data warehouse can be shared with an audience inside or outside an organization, with the consumer paying the data producer for the data access. Data catalogs make these scenarios much easier to implement.

## Data Access Management

When we talk about data discoverability via catalogs and data sharing, we must ensure that the right set of actors has access to the right data, and more importantly, that actors don’t have access to other data. A key part of data governance is access

management, where there is a set of capabilities that lets the actors manage data at various levels, from access to the data stored to access to the data through other applications like data sharing or through the warehouse.

I would like to explain this concept as a ringed approach to data access management:

- At the very inner core, there is access to the data itself, which is a data lake storage–level security model that helps you lock access to the storage itself.
- The next layer involves access to the computational engines running on top of the data lake storage, either via the ETL processes, the data warehouse, or the dashboards.
- The next layer involves boundaries at your cloud system level, which controls the visibility and accessibility of your cloud resources or your data across a network boundary, such as regional access restrictions on what data needs to stay in a region versus what can be transacted across regions.
- Finally, you can use comprehensive data security tools, such as Apache Ranger, to apply advanced rules and policies across the data in your multiple data stores (e.g., data tagged as PII must not have access to anyone other than the HR department).

Figure 3-9 illustrates this approach.

![](img/7a3ef505ef42a8f03ad37f9a013ac15a_101_0.png)

Figure 3-9. Approach to access management

## Data Quality and Observability

Given the criticality of data to running a business, the quality of data has grown to be as important, if not more important, than the quality of the code. Whether it is an outage that blocks critical news like COVID-19 data or a wrong award announcement like the embarrassing comment that La La Land won the Oscar in 2017, mistakes in data have a ripple effect downstream.

Organizations are increasingly relying on measuring and monitoring data quality as a best practice, and this is one growing area of investment in the data lake space. Although it is out of scope for this book, I recommend that you check out other resources on data quality and data observability for more in-depth coverage of the concepts and learn more about the available tool sets you can leverage for your data quality and observability solutions. I will provide an overview of the basic concepts and approaches to data observability in this section.

In *Data Quality Fundamentals* by Barr Moses, Lior Gavish, and Molly Vorwerck (O’Reilly), there is a succinct definition of the five pillars of data observability, or the attributes that need to be measured to ensure data quality in your data lake architecture. This is a great starting point for how to think about data observability.

Especially in a cloud data lake architecture, there is a disparate set of components that load data into the data lake storage and process data to generate high-value insights, and there is yet another set of components that draws the dashboards. One component is not quite aware of the others. Hence, data observability is of critical importance in building this common understanding across the components in a data lake by taking a data-centric approach.

Paraphrasing from the book, the five pillars of data observability are as follows:

*Freshness*

*Freshness* is an indicator of the recency of the data. When was the last time this data was refreshed? As an example, we talked in earlier chapters about a common pattern of having daily uploads of data from the operational datastore into the data lake. The freshness attribute indicates how fresh this update was, so if last night’s data upload failed, then you are clear that the reports are based on data that is two days old.

*Distribution*

*Distribution* is an indicator of the acceptable ranges or values of the data. This lets you define acceptable ranges for the data. Often, when the charts in your dashboards looked abnormal, you would wonder if the data was highlighting a genuine problem or if the data was just simply wrong or broken. Ensuring that you have acceptable ranges for the data helps you know that when the data goes above or below the acceptable ranges, you have a data problem rather than a real issue with the trends. As an example, when your recent sales data has
