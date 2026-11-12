
Finally, a prospective consumer can discover what data products are available in the metadata spreadsheet and choose those necessary for their use case. Then they can either create a new sink connector to write the data into their own database (6a), or they can simply consume the data natively using a stream-processing framework like Kafka Streams or Flink (6b).

The MVP self-service data platform makes it easy to get started with the practice of data mesh without over-investing in tools, systems, or processes that you may not need. You can begin practicing publishing data products, and arguing and negotiating roles and responsibilities, metadata requirements, service levels, connection interfaces, and schema types. You’ll get a taste for the social changes that data mesh brings as well as identify the people and teams that are amenable to change and those who are going to have a much more difficult time with it.

The most important thing when getting started with a self-service platform is to internalize and practice the principle that “You aren’t gonna need it”. Build new functionality as you need it, guided by the pain points you and your teammates are experiencing. Express the issues you encounter to the federated governance team so that together, you can come to a decision about the next set of features and improvements to make to the self-service platform.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_116_0.png)

> Focus on resolving a selection of major pain points when building out your data mesh MVP. Then you can evangelize the successful pain point resolution as a way to build trust in your data mesh platform and gather feedback from others who may have their own adjoining pain points.

There are some deficiencies with the MVP that we will look at solving in the next section. First, since registration is a voluntary process, it’s possible that the metadata spreadsheet doesn’t accurately represent what’s available. Second, there are no permissions—anyone can read or write to any event stream, which is bad practice. Third, you can’t really tell who is using your data products. And fourth, there is no enforcement of security or data quality standards. We’ll look at resolving each of these issues with further evolution of the self-service platform to Level 2 and Level 3.

## Level 2: The Expanded Platform

The expanded platform (EP) addresses a number of the shortcomings of the MVP, such as a lack of user and service identity, a lack of permissions, and a fairly low-effort data catalog that doesn’t necessarily reflect reality.

It’s best to think about the EP as focused on two main outcomes. The first is to make it easy for data product owners and prospective data product consumers to use the data mesh. For data product owners, this means making it possible for them to publish their event streams as data products, while for consumers it means making it easy for them to find data products, register as consumers, and set up any connectors they may need to pipe the data into their own service space.

The second main outcome is to implement basic service identity and controls. At a minimum, services need unique identities for the assignment of permissions. Though seemingly simple, we also must consider that the self-service platform will typically span many subplatform components—code repositories, event brokers, and compute resources, just to name three common inclusions.

The components and features of the extended platform include:

- Full-featured metadata catalog
- Data product management service and UI
- Service and user identities
- Basic access controls
- Stream processing for building data products

Start talking to your infrastructure and platform engineering people. You’re going to need to figure out how to leverage existing systems and tools for usage in the data mesh self-service platform. Your engineering team likely already has a process for building, Dockerizing, testing, deploying, and monitoring applications. Don’t reinvent the wheel—copy what they already have to build and deploy data products and tweak it as needed.

First, let’s take a look at improving that shared spreadsheet “data catalog” with something a bit more robust and useful.

## Full-Featured Metadata Catalog

The first order of business is to replace the spreadsheet with a proper data catalog.

There are many options available for you to choose from. Popular open source software (OSS) projects include Apache Atlas and Amundsen, just to name two that I have successfully used in data mesh implementations. While you could also model and build your own data catalog using a relational database, these off-the-shelf, dedicated data catalog products include their own discovery UIs, well-formed APIs, and publishing, searching, and modeling functions.

Some important features of a full-feature metadata catalog include:

### Metadata modeling

Aside from data product metadata, you can create metadata for people, teams, applications, event brokers, and connectors. You can also create metadata models for the relationships between these elements, resulting in a traversable graph of relationships.

### Search functionality

A data catalog may also provide search functionality for you to query the metadata entries. Depending on the data catalog you select, you may have availability to SQL or SQL-like querying capabilities in addition to basic string matching. Search functionality is an essential component in making the platform self-service, so ensure you have evaluated the catalog’s capabilities before investing in it.

### Dependency identification through graph traversal

Identifying upstream and downstream dependencies via graph traversal is important for validating the integrity of your data mesh. For example, you could validate that all upstream data products and services meet a minimum service tier when adding a new dependent data product or consumer application, such that you don’t violate your own SLA. Similarly, you could use this same graph functionality to identify all existing consumers of your data, so that you could then notify them of upcoming changes.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_118_0.png)

Data catalogs have a tendency to be chronically out of date, usually because they’re treated like documentation—updated later, after the fact, if someone remembers to do it. It’s essential for a data catalog to be part of the operational workflow for creating, discovering, and using data products, updated automatically as new products are created, removed, and subscribed to.

Once you have selected a new full-featured data catalog, your next step is to ensure that it’s properly integrated into your data product management workflow. You can measure how successful you are in integrating your data catalog by the amount of manual interaction required to keep it up to date, with zero manual interactions being an indicator of success. Your data catalog should be populated and altered only via the confines of the data product management service. We’ll look at this in more detail in the next section.

## The Data Product Management Service and UI

The UI in the MVP is nonexistent. The data mesh user has to create their data products, manually add them to the spreadsheet, and hope that others have followed the same rules. It's likely that someone will forgo filling out the mandatory metadata, accidentally delete someone else's entry, or simply not register the data product formally. After all, you can still use the data even if it's not in the spreadsheet!

The easiest way to get a data product UI is to use a data catalog that already contains a UI and search elements. Bonus points if it also contains a way to set fine-grained permissions so that the majority of users have only read-only permissions (more on that in the next section). However, if you decide to craft your own UI, you're going to need to think about how your consumers can browse and search available data products as well as manage their own. The basic operations that it must support include:

### Browse data products

Users will need to browse through existing data products to see what's available. Figure 5-2 shows a sample of what the UI could look like, including the name, a description, and some metadata-related information. Clicking on a data product would provide you with more information about the product.

### Browse stream data products

| Name | Description | Domain | |
| :--- | :--- | :--- | :--- |
| Customer orders-Canada | Orders originating from Canada | ecom.customer.orders | View |
| Customer orders | Orders originating from USA | ecom.customer.orders | View |
| Item inventory | Global item inventory | ecom.inventory | View |

Figure 5-2. A data product browsing UI, including domain information, registered consumer list, and metadata

### View data product information

Each data product should have a detailed page that showcases the full range of data, including owners, description, schema, digital location, and any other information your governance team deems relevant. Figure 5-3 shows information that you could expect to see when viewing a data product. Note that there are some buttons that enable self-service actions, including messaging existing consumers, messaging the data product owner, and requesting registration as a consumer.
