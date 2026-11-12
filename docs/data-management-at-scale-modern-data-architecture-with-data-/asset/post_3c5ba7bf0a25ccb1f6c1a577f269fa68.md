
deletes) provides the ability to replay entire backlogs of data. In general, this simplifies managing the inevitable changes in data models because you’re working with a paradigm that allows you to replay all the data if needed, without the involvement of the source systems to play a role. Having said that, it’s important not to mix roles and responsibilities, and it may be better to keep this transaction log separate from your MDM solution (while ensuring that your MDM platform can work with an event streaming way of thinking).

For use cases that can tolerate higher latency, you can distribute master data using your data product architecture. However, it’s important to not make point-to-point connections between the MDM hub and providing and consuming applications. If you do this, there’s a risk of data inconsistency. Data consumers, for example, might get new records delivered from the MDM hub directly, while data products aren’t yet up-to-date and may not reflect the data used within the MDM hub. If the MDM hub always uses the data products, you’ll never have these inconsistencies, so a good principle is to feed your MDM environment with the same data products your consumers are using for their analytical workloads.

## Designing a Master Data Management Solution

The process of implementing MDM starts with identifying what unique and trustworthy data is created and maintained in what applications and systems. The golden datasets, from the golden source systems, are the basis. Since data is stored differently in each application, it’s important to understand each context and what business rules, formats, and reference ranges impact the master data there.

The next step is designing an MDM solution that harmonizes the data and makes it consistent. You can either go for a vendor-provided solution, or design one yourself. Some popular vendors in the MDM space are CluedIn, Profisee, Informatica, and Tamr.

The design and implementation of MDM has a strong relationship with data modeling and data integration: identifying overlapping data attributes, harmonizing and modeling the data through ETL processes into proper data models is a vital part of the overall design of the MDM solution. Depending on the style and solution type you choose, the amount of data modeling and data integration work involved can vary, as well as the integration patterns and architectures. The coexistence style, for example, relies more on real-time processing and uses API-based and event-driven architectures, while a consolidated approach can be engineered using only batch data from your data product architecture(s).

You’ll need to establish governance, making sure data owners and application owners are in agreement on the following items:

- What regulations must be applied (for example, certain laws don’t allow different customer datasets to be combined)
- The scope of data that will be mastered
- Rules for cleaning, matching, merging, linking, and consolidation²
- For what data usage classifications (see “Data Usage Classifications” on page 244) the central master data store can be used as an authoritative source and for what situations data consumers must fall back on their local (original) source systems
- Which data quality rules and processes to implement
- Whether improvements should be delivered back to the original source systems, and if so how quickly and with what data integration techniques (expect variations and combinations of batches, APIs, and streaming)
- Who owns the newly created data (domain teams or a central team)
- Which users are authorized to approve or reject proposed changes
- Master data definitions, which should be stored in central metadata repositories such as the data catalog

Once you’ve completed these steps, you can start to strategize about how to distribute data back to the original source systems and other applications.

### Domain-Oriented Master Data Management

Although many MDM solutions can manage both reference data and master data, I recommend clearly delineating between how to manage these two. Reference data is data used to define, classify, organize, group, or categorize other data (or value hierarchies, like relationships between product and geographic hierarchies). Master data, by contrast, is the core data that is absolutely essential for the enterprise. Each type is typically managed differently and comes with its own methods of data distribution.

### Reference Data

One approach for achieving data consistency between domains is asking your domains, when distributing data products, to conform themselves to centrally managed reference data. Currency codes, country codes, and product codes are common examples of reference data that should be provided. This data can be published, for example, in a master data store or central repository. When any of your domains distribute data products to other domains, they should use the identifiers from the enterprise reference data to classify the data. The mapping of local data to enterprise data allows other domains to quickly recognize master data within data products.

2 Enterprises rarely get the rules for matching and merging right the first time. You’ll also overfit and underfit with merging and linking. Having an agile MDM system that allows you to iterate on your rules is important.

![](img/999fd6f5dfec4455873f6d26acc75ff0_321_0.png)

Asking domains to conform their data products to enterprise reference data sounds like a contradiction to the data mesh approach. To some degree this might be true, but if regulation forces us to be consistent on certain data dimensions, then master data management is the only way to properly achieve this.

Of course, if domains are to map local reference data to centrally managed reference data, they need to know exactly what should be mapped to what—for example, if alpha-2 codes (e.g., US) and numeric values (e.g., 840) for country codes should always be mapped to the standard alpha-3 codes (e.g., USA).³

When asking your domains to conform their data products to enterprise reference data, there are different approaches you can follow. One option is to provide central services so domains can discover, understand, select, and incorporate enterprise reference values when building data products. For example, you might have a data catalog that references a central storage account in which all reference data is hosted. With this approach, domains perform the mapping activities themselves, using synchronized copies of enterprise reference data to look up the reference values before distributing any data products. This integration process can be anywhere from fully automated to manual.

An alternative is to offer central MDM services so domains can manage the local-to-enterprise mapping within the MDM solution. In this case, each time a data product is published, a postprocessing activity will run to check for inconsistencies and enrich the data product with enterprise reference data. This approach might take more work to implement up front, but it removes the need to build solutions for this within all of the respective domains. It also guarantees better quality because the mappings are validated on a central level.

You can use both approaches to adding reference data to data products side by side. That is, some domains might do the mapping themselves, while other domains rely on centrally managed services. In this case, a central governance body should oversee reference data management across the enterprise to ensure the goals of standardization, quality, and operational efficiency are met.

3 It's worth mentioning that many of these reference codes can be automatically rectified, even if they are in different formats, using external services. Many programming languages have this capability built in as well.

## Master Data

For master data, the approaches to data distribution and integration are different because the results may flow in different directions.

### Master identification numbers

The first important aspect of master data is master identification numbers,⁴ which link master data and data from your domains together. These data elements are critical for tracking down what data has been mastered and what belongs together. Identifying unique data and assigning master identifiers can only be done centrally, not locally within systems. It requires having the master data from the different systems come together in an MDM solution. Figure 10-2 shows how this works in practice.

![](img/999fd6f5dfec4455873f6d26acc75ff0_322_0.png)

Figure 10-2. Solution design for mastering and distributing data via an MDM

Identifying and maintaining these relationships is important not only for knowing what data has been mastered, but also what data can be quickly linked to other data. If local (domain) keys in an operational system change, the only element that binds everything together will be the master identifier, so you should also build feedback loops from your MDM solution to your local domains. One option is for domains to maintain the relationships between local identifiers and master identifiers in a lookup table, which may be stored within the MDM solution itself. For golden sources, you
