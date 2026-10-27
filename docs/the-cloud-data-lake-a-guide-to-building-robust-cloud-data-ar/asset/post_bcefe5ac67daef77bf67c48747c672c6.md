
With this in mind, there are two outcomes in Phase 2:

- Technical design choices are finalized for the cloud data lake.
- A project plan is in place with scope, dependencies, and timelines that can be communicated to your stakeholders.

![](img/7a3ef505ef42a8f03ad37f9a013ac15a_199_0.png)

## Timelines for Your Cloud Data Lake

I hear from my customers often about how long it will take to implement the cloud data lake once the architecture is finalized. This timeline largely depends on three factors: the complexity of your architecture, the implementation choices for the cloud services you are choosing, and the skill levels of your team. A good rule of thumb is, as you define your project deliverables, identify rough timelines for the efforts as T-shirt size estimates, as follows:

- XS is less than one week
- S is one to two weeks
- M is two to four weeks
- L is one to two months
- XL is more than two months

This will provide a rough, high-level timeline for your project. You can revisit this frequently to understand how your original estimates change as part of your regular project management processes, such as sprint planning.

## Finalize the Design Choices for the Cloud Data Lake

When it comes to a cloud data lake, there is a diverse set of options available, from architecture patterns, cloud providers, and specific services on the cloud. In this section, let’s take a closer look at how to make these choices with the decision framework.

### Picking your architecture

In “Design Considerations” on page 58, I walked you through an assessment of the various factors that go into deciding on a cloud data lake architecture. I encourage you to refresh your memory by reviewing Table 2-1.

The cost versus complexity comparison of the architectures shown in Figure 7-4 comes into play here as well.

![](img/7a3ef505ef42a8f03ad37f9a013ac15a_200_0.png)

*Figure 7-4. Cloud data lake architecture cost versus complexity*

Now that you have refreshed your memory on these concepts, you can make an assessment of the architecture that is the right fit for you. I will add a few more factors for you to keep in mind:

**Architectures could evolve over time**

Depending on your discoveries during Phase 1, you can address your customer and business needs in an all-or-nothing or a hybrid fashion. As an example, if you have requirements from the marketing team to support data science scenarios, and the sales team wants to support dashboards for the quarterly sales projections, one option is to implement a modern data warehouse architecture where your data scientists leverage tools that directly operate on data in the data lake while your business analysts from the sales team consume the data and the dashboards from the data warehouse. Alternatively, you can start with this modern data warehouse and eventually migrate the data warehouse scenarios to run directly on the data lake, evolving into a lakehouse architecture.

**Architectures can compose over one another**

Cloud data lake architectures are not mutually exclusive and can compose on top of one another. For instance, most customers I have talked to have an existing data warehouse that serves their business needs. The most common first step is to introduce a cloud data lake, which essentially helps minimize the cost (by storing past versions of the data) and supports new scenarios, such as machine learning. As a second step, the data warehouse moves to the cloud, either in the modern data lake or a lakehouse architecture, composing on top of this existing data lake. As your organization grows, so will the variety of the scenarios, and you can compose on top of the existing architectures with a data mesh.

**Architectures span across multiple cloud providers**

A note to always remember is the cloud data lake architecture does not enforce a unilateral implementation across a single set of components, enabling a multi-cloud or a hybrid cloud architecture. As an example, organizations that have an on-premises architecture have data analysts consuming their on-premises data warehouse. When they move to the cloud, they could start with a cloud data lake for data science scenarios while their business analysts continue to operate on premises in a hybrid architecture.

In summary, the architecture that you choose doesn’t need to be constrained by your implementation choices. You choose your architecture based on your customer and business needs and have flexibility in actual implementation.

### Picking your cloud provider

Once you’ve decided on an architecture, the next choice is to pick a cloud provider. Although there are multiple private cloud providers and hybrid cloud offerings, we’ll focus on the most common and popular choices: the public cloud providers. The most popular cloud providers for cloud data lake services are AWS Lake Formation that leverages Amazon S3, Azure Data Lake Storage, and cloud data lake offerings from Google Cloud, composed of Google Cloud Storage and BigQuery. The public cloud providers across AWS, Azure, and GCP have their own highly scalable distributed cloud data lake storage solutions, along with cloud native ingestion and orchestration services and managed data processing offerings that are compatible with open source offerings like Hadoop and Spark. These public cloud services have a healthy competition with one another on usability, price, and other data-related features, offering a rich set of services to cloud data lake customers.

To identify the right cloud services provider for you, there are a few factors to consider:

**Cost of your solution**

Identify the cloud services required to build your cloud data lake architecture, and ensure that the cost of the solution meets your budget.

**Cloud service capabilities**

While the cloud service providers have highly comparable features, they also have some unique nuanced capabilities. As an example, AWS Lake Formation provides a simplified interface to get started and orchestrate your cloud data lake, and Azure Data Lake Storage offers a truly non-siloed data store that supports multiple protocols, including Network File System (NFS) version 3.

**Integration with your other cloud services**

If you already have an implementation with a particular cloud provider, then that is the best starting point for your cloud data lake implementation. You could extend your current cloud implementation into the data services. As an example, if you already have a web application running on the cloud, your data platform could leverage ingesting logs or clickstream data from the web application to enable scenarios like customer dashboards or personalization features to your web application. This is a good starting point because you’re starting on familiar terrain and extending an existing implementation.

The best way to assess the capabilities of the cloud providers is to do a PoC implementation of your scenarios. This will help you understand the best fit for your needs while also informing you about the cost profile of these solutions. Pick a PoC that best illustrates the needs of your customers. As an example, when Klodars Corporation is evaluating cloud data lakes, it can pick two of its highest-priority problems:

- The sales dashboards are taking a long time to refresh, disrupting the executive review meetings.
- The marketing team needs to understand how to best target its social media campaigns based on analysis of existing trends.

When Klodars did a PoC on the three cloud providers, it found that AWS Lake Formation offered the simplest implementation for its cloud data lake; Azure Data Lake Storage offered the lowest cost as well as interoperability with its on-premises systems with NFS v3 support; and Google BigQuery offered the fastest performance on both structured and unstructured data while scaling better than the data warehouse solutions on AWS and Azure. Given that Klodars’s biggest pain point is around performance, it picked GCP for its cloud data lake implementation.

### Decision points for data lake migrations

If you have an existing on-premises infrastructure (either data lake or data warehouse) that you would like to migrate to a cloud data lake architecture, then you have a few choices to make. These can be bucketed under three major design choices:

**Lift and shift**

In this design choice, you are preserving your existing design and primarily changing the hosting to the cloud. The advantage of this approach is it requires minimal refactoring of your code and components, so you can get going on the cloud sooner. Most public cloud providers like AWS, Azure, and GCP offer pro-
