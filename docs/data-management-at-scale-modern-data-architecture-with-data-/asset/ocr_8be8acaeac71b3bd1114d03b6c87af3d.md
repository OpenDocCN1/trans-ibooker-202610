Here's where things should stand at the end of this stage:

+   - The new governance and operating models, as discussed in Chapter 8, are defined. These include new roles and responsibilities.
+   - Dashboards have been created to show how well your domains and their corresponding metadata are represented.
+   - Domain teams own the data life cycle, from start to end (from the applications that create the original data to the data products that are consumed by other teams). They own all the data models, and determine what data is suitable for sharing with others.
+   - Domain teams use DataOps best practices for managing their data products. This could, for example, mean that each team uses its own individual Git repository, from which they provision their CI/CD and data pipelines through code. (There is more guidance on this in “Data-Driven Culture” on page 365.)
+   - Data product development is supported with tooling and data modeling best practices. Guidance (such as standards for identifiers, reference data, layering and structuring data in separate tables for providing coarse-grained security) is published in wikis for better collaboration.
+   - Development teams have visibility into pipelines and how they impact downstream consumers. Developers are supported by data quality services, which proactively alert users when problems occur.
+   - Manual processes and pipelines are replaced with templates and services. Instead of hardcoding locations and parameters, developers store their information as source code in version control. These repositories may include other artifacts as well, such as test and deployment scripts, libraries/packages, configuration information, and so on.
+   - Additional ingestion patterns have been added, such as real-time or event-driven ingestion.
+   - Data access policies, which specify who can access what data products, are stored as code or configurations. At this stage, still expect your central team to be in full control of provisioning data access.
+   - You still use one single landing zone. There’s limited variation in the services you offer. All domain teams use the same blueprint configuration(s).
+   - There’s a consistent metamodel that uses a data product and data domain. This metamodel is managed in a data governance service and ensures that all domain teams know what data products are owned by what domains.

While onboarding new use cases and domains into your architecture, it’s likely that you will run into migration or legacy scenarios. For example, you may encounter situations in which consumers demand historical data from several years back. When