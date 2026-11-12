
Here's where things should stand at the end of this stage:

- The new governance and operating models, as discussed in Chapter 8, are defined. These include new roles and responsibilities.
- Dashboards have been created to show how well your domains and their corresponding metadata are represented.
- Domain teams own the data life cycle, from start to end (from the applications that create the original data to the data products that are consumed by other teams). They own all the data models, and determine what data is suitable for sharing with others.
- Domain teams use DataOps best practices for managing their data products. This could, for example, mean that each team uses its own individual Git repository, from which they provision their CI/CD and data pipelines through code. (There is more guidance on this in “Data-Driven Culture” on page 365.)
- Data product development is supported with tooling and data modeling best practices. Guidance (such as standards for identifiers, reference data, layering and structuring data in separate tables for providing coarse-grained security) is published in wikis for better collaboration.
- Development teams have visibility into pipelines and how they impact downstream consumers. Developers are supported by data quality services, which proactively alert users when problems occur.
- Manual processes and pipelines are replaced with templates and services. Instead of hardcoding locations and parameters, developers store their information as source code in version control. These repositories may include other artifacts as well, such as test and deployment scripts, libraries/packages, configuration information, and so on.
- Additional ingestion patterns have been added, such as real-time or event-driven ingestion.
- Data access policies, which specify who can access what data products, are stored as code or configurations. At this stage, still expect your central team to be in full control of provisioning data access.
- You still use one single landing zone. There’s limited variation in the services you offer. All domain teams use the same blueprint configuration(s).
- There’s a consistent metamodel that uses a data product and data domain. This metamodel is managed in a data governance service and ensures that all domain teams know what data products are owned by what domains.

While onboarding new use cases and domains into your architecture, it’s likely that you will run into migration or legacy scenarios. For example, you may encounter situations in which consumers demand historical data from several years back. When building up your new data product architecture, you’ll learn that historical data is only available from the moment you onboarded your first data products into the new architecture. So, if you need historical data from before the onboarding period, you’ll have a gap. To solve this problem, make an extract or one-off copy of the historical data from your other environment(s). For example, if your data warehouse retains data from the past seven years, you can use that data to build a legacy data product, then combine that legacy data product with incoming data that feeds into the new architecture. This will give your domains the full picture. Note that combining historical data with new data isn’t always that easy, however; you’ll often need to match fields, delete duplicates, clean the data, or write business logic.

When scaling up further, you’ll need to interconnect domains by enabling them to exchange or directly share data products. For this, you need to set interoperability standards and implement query services. Consider popular file formats such as Parquet or Delta, and (serverless) SQL services to allow other domains to access and browse the data products.

## Optimization Phase: Professionalize Your Capabilities

After the foundation has been established, it’s time to iterate on prioritized business use cases and further professionalize your capabilities. One of the key objectives for this stage is to carry over all the supporting activities from the central team to your domain teams. Look for inefficiencies, and try to solve these with self-service and automation. To strengthen your organization, guide your teams so they become more efficient in managing their data and corresponding data pipelines. Allow them to self-onboard and self-subscribe to data products. Deploy services that allow for self-registration and maintenance of metadata. For example, deploy APIs that can be easily integrated with the CI/CD processes of your domains.

For the next iteration of your architecture, you will focus on real-time data processing, consumption-readiness, security, MDM, and distribution of curated data. Try to standardize the consuming side of your architecture with blueprints and services. Remember that data usage is diverse: many variations are possible. To remain in control, gradually expand by launching one new service at a time. For each new service, evaluate the need before handing it out to your domains. Figure 12-4 shows an example of an updated design.

![](img/999fd6f5dfec4455873f6d26acc75ff0_385_0.png)

Figure 12-4. As the adoption of your architecture grows, new services will be added

One key thing about the solution architecture is that many of the services don't yet support a federated way of working. Your central team must focus on closing these gaps by integrating services and delivering self-service and automation capabilities. They are the ones to arbitrate between deviation, tools, and technical requirements. They own all choices related to programming frameworks, ETL services, and storage-related services.

For managing data reusability concerns and consistency, look for data products that have the highest levels of usage. What you've most likely encountered is multiple domains complaining about data that is too hard to integrate and combine with data from other domains. So, look for repetitive harmonization and quality improvement activities that are being performed across teams. If you see many of these overlapping activities, the data products in question might be candidates for master data management. Alternatively, you can separate out generic (repeatable) integration logic and ask one data product team to take ownership of the data using a customer/supplier model. Consult Chapter 10 for recommended approaches.

As part of the data sharing experience, your data product teams should be able to describe which data products can be used for which purposes. Domains should be able to manage, in close collaboration with the central governance team, data access controls on data they own. Unfortunately, at the time of writing, I'm not aware of any out-of-the-box solutions that will give your teams a great experience. If you want to support your teams with easy-to-follow workflow processes and automated creation of policies, consider building a small data contract framework yourself that deploys consumer-oriented (secure) views using a workflow process. This framework may include pointers to business semantics, as well data quality and service level agreements. For more guidance, see Chapter 8.

At the end of this stage, here's where things should stand:

- Your architecture supports different patterns for data distribution and application integration. Preferably, guidance for data products, APIs, and events would be aligned.
- New data domains can be onboarded quickly via automation and provisioning through Infrastructure as Code.
- Standards on classifications and labels have been formalized, guidance and playbooks have been created, and teams are trained.
- Techniques such as natural language processing and machine learning are applied to support domains with metadata management.
- Governance and access control policies are automated. For this, consider setting up a small workflow, an application, and programmatic access APIs for registration.
- Data usage and processes for turning data into value are standardized. Updated blueprint templates are offered to your domain teams, from which they can choose what to use for individual use cases.
- Master data management services have been added to your architecture for ensuring end-to-end and cross-domain consistency.
- Interaction between stakeholders is optimized through data governance bodies. Teams come together on a periodic basis for triage and overall planning.
- Data products have multiple endpoints, also known as output ports, to cater for a large variety of use cases.
- Extra landing zones have been added for situations in which domain teams have contrasting requirements.

When scaling up further, it is important to have your structures for data governance clear. So, you need to move away from poorly defined data roles to a clear structure with well-aligned processes. Depending on your organization's size, you may have multiple governing bodies and data product teams that interact. Figure 12-5 shows an example of how this interaction could work.
