

might want to implement a principle of routing master identification numbers back to the underlying domain.

## 应用键与代理键

Data engineers have two choices for unique identifiers: you can use a unique, existing business identifier or application key, or a unique system-generated surrogate key that is immutable and will never be duplicated. The benefit of using a surrogate key is that it has no semantic meaning. MDM assigns a surrogate key to every master record to help you recognize groups of master data.

When using application keys, you have to remember that a key that is unique in the source system may not be unique across all systems. For example, take a system that uses seed identity keys starting from 1 and going up. You might say this is a bad practice—clearly, if there are other systems in the organization that use the same approach, the identifiers will not be unique across systems—but there are many systems that do actually work like this! Using a random GUID removes this problem. Thus, a business identifier by itself is often not enough to uniquely identify an object when we’re bringing data together from multiple systems.

Now imagine the following situation: you ingest data from an operational application into your MDM system, and the MDM system generates a new random GUID for it. Then, someone decides to wipe all data from the MDM system and ingest the data again. A new GUID would be generated for each record. What happens if the data that was in the system was already being used and referenced out in the business? MDM can address this problem by using deterministic GUIDs, where the same input data from the same location will yield the same GUID every time.

When distributing master identifiers, it’s recommended that you not extrapolate the MDM master identifiers to all source systems, which can create consistency issues between domains. Thus, only applications or systems that are subject to master data management should obtain the master identifiers from the hub. Systems that aren’t subject to MDM should use their own local (domain) identifiers.

## MDM 域和数据产品

When integrating, cleansing, and harmonizing data and storing it in a master data store, you generally create new MDM domains. These domains create uniform sets of data for certain subject areas. Well-known examples are customers, products, geographical locations, finance and risk data, and employee data. Master data from these MDM domains is expected to find its way (back) to other domains. It’s distributed as data products, in the usual manner (as described in Chapter 4). A general recommendation is to distinguish regular data products from master data products because master data products use regular data products as input for master data management. You could do this, for example, by using a data catalog.

Note that you might not need to distribute master data products to all domains. You might have a set of domains within the boundaries of a certain scope that need to conform to using master data, while domains outside that scope don't.

## 域级 MDM

When you look for overlapping data, you'll likely discover various degrees of overlap. Some data is generic and spans many domains. Other data might span only two or three domains. You can draw distinctions about the amount of data overlap and its importance by implementing domain-level MDM. To do this, you create partial views of your master data within specific scopes. To distinguish the importance and amount of overlap, I recommend using the following classifications:

- **企业数据**：具有企业级范围和适用性的数据。它可以在多个域、所有域甚至企业外部使用。企业数据既包括主数据也包括参考数据，其一致性非常重要。
- **域数据**：由部分但非所有域共享的数据。对重叠域的管理很重要，但没有企业重要性，也没有任何监管重要性。域数据通常在几个域内管理和维护，而不是在全球层面。
- **本地数据**：仅在单个本地域或系统内使用的数据。它不会被任何其他域共享或使用。

Figure 10-3 shows an example of managing master data within the boundaries of a domain. As you can see, the overlap with a data product is significant. Instead of only using ETL, MDM and ETL are used for creating new data products.

With an approach like this, you can support your domains by providing MDM services to them on an as-a-service basis. We'll look at this next.

![](img/999fd6f5dfec4455873f6d26acc75ff0_325_0.png)

Figure 10-3. Mastering and distributing data within the domain itself (this form of MDM is aligned with domain data, while the previous example you saw is aligned with enterprise data)

## MDM 与即服务的数据质量

To successfully implement MDM within your enterprise and domains, I recommend offering MDM capabilities to the domains as a service. MDM solutions are often complex and can be hard to implement. Abstracting away the infrastructure and providing MDM services to your domains in this way can simplify usage tremendously. If you’re using a central solution, I recommend segregating the domains (or groups of domains) from one another. If all master and reference tables are stored centrally, you can distinguish them using, for example, a catalog and metadata, and classify which datasets are enterprise and which are domain data.

The same applies for data quality services, such as profiling, matching, standardization, and validation functionality. With this model, quality measures and controls become transparent, and domains don’t have to implement these solutions themselves. A recommendation that applies here is to make it part of your data pipeline solution. So, bake it into your data product architecture as a standard service or component.

Master and reference data management are ways of achieving higher levels of consistency and reuse in your organization, but there are different approaches as well. In the next section, I’ll discuss how these approaches may overlap in integration and data distribution, as seen in MDM.

## MDM 与数据策展

Master data management overlaps with the discipline of _数据策展_: the process of collecting data from diverse sources and integrating it so that it becomes more valuable than its independent parts. We’ve already touched upon the creation of new data in Chapter 2 when discussing constructor domains, which are data-consuming domains that also act as providers by creating and distributing new data products. This can be seen as a form of data curation.

Data curation removes repeatable integration work from other teams, which can be important for enterprises. Within large organizations this process is typically combined with DataOps, a methodology to create and deliver automated, integrated, and trusted data pipelines. Although data curation overlaps with MDM, it’s important to clearly show the differences and formulate some strong principles around each of them. In the next sections, I’ll highlight some different approaches to creating and managing curated data.

![](img/999fd6f5dfec4455873f6d26acc75ff0_326_0.png)

> DataOps 与 MDM 之间存在大量重叠，但它们针对不同的角色（DataOps 的数据工程师和 MDM 的数据管家）。DataOps 的目标是构建自动化管道，通过代码解决数据质量问题。MDM 提供一种操作流，当发现无法（或可能不应）通过代码管理或自动化的问题时，会介入人工处理。

Both MDM and curated data creation aim to deliver value and make data consumers’ lives easier. They share techniques like ETL, data cleansing, data science, and meta-data management (capturing lineage, describing data models, etc.).

As you’ve learned, at its core MDM consists of the processes used to collect, link, cleanse, integrate, match, enrich, and distribute (master) data. During these processes, duplicates are typically removed and inconsistencies in the data are corrected. MDM also has a strong focus on entity consolidation (creating one entity out of several similar or identical entities) and cluster reduction (creating a single golden record). The results MDM produces are authoritative sources of master and reference data. Unique data is typically detected centrally and the output persisted in a new physical location, since it includes many improvements. Most importantly, _不会使用不同的上下文创建新数据_. Facts, such as quantitative information, remain in their own context, although improvements are made for correctness and consistency.

