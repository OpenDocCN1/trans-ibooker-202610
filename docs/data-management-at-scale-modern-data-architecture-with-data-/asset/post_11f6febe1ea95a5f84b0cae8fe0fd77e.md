
demand for MDM are security, fraud detection, and regulations such as the GDPR and the CCPA. Inefficiencies in managing master data can result in failures to detect fraud or penalties from regulators.

![](img/999fd6f5dfec4455873f6d26acc75ff0_315_0.png)

> It’s easy to fall into the trap of enterprise data unification: widening your scope and mastering too much data requires more effort to integrate, govern, and coordinate data dramatically. Use metadata to decide what to include. Analyzing lineage, data models, and data contracts can reveal overlap and common areas of interest between domains and thus determine which data should be subject to MDM. In general, data elements that are used within only a single domain shouldn’t be within MDM’s scope. Data elements shared by numerous domains, however, are candidates.

Before demonstrating how to apply MDM within a large ecosystem, it is important to say that MDM can’t—or shouldn’t—be solved with technologies that primarily focus on data integration (ETL). It’s considerably more involved than that. A modern MDM stack comprises many aspects, including the following:

- Data quality management, such as detection handling, correction handling, and follow-up management
- Automated match and merge rule management for handling data duplication
- Location/localization handling for managing geographic limitations and implications
- Data enrichment management for supplementing missing or incomplete data
- Consent management, such as data legislation and usage management
- Data governance management, such as workflow and life cycle management for the approval process and decisions made on data quality, rules, localization, data access, and so on
- Data source management and integration features, such as updating your source system data with newly created MDM records

## Demystifying Master Data Management

Most organizations have systems that share data elements with commonalities. Take “customer” and “product,” for example: chances are high that multiple versions of the same master data are stored in different parts of the organization, across many applications and systems. Such overlap isn’t only inefficient but can result in inconsistencies as well. The customer’s name might be spelled differently; addresses can differ; each customer might have their own unique ID number in each system. Each copy of the data can belong to a different department or domain. If customers’ data changes or needs to be combined, how will you recognize who the same customers are, despite these differences? This is where MDM comes in. MDM focuses on detecting and resolving data inconsistencies by distributing mastered versions of data across applications and systems.

1 In practice, you’ll find numerous data elements (for example, “customer”) don’t even have unique and coherent identifiers that can easily be stitched together. MDM can be a solution for this problem as well.

MDM can be implemented in many different ways using different approaches and scopes. In the following sections, we’ll look at some possible implementation styles and data integration options and then move on to designing an MDM solution.

### Master Data Management Styles

To manage your master and reference data successfully, you need to design and implement an MDM solution. Lyn Robison, writing for Gartner, identifies four MDM styles: consolidation, registry, centralized, and coexistence (see Figure 10-1).

![](img/999fd6f5dfec4455873f6d26acc75ff0_316_0.png)

Figure 10-1. An abstract comparison of the four master data management implementation styles from Gartner

Let’s take a closer look at each of these:

### Consolidation style

The consolidation style has a great deal of overlap with data warehousing: it consolidates master data and other data into a single repository or hub, called the master data store or MDM hub. The hub uses data integration patterns to continuously pull together master data from the operational systems to improve quality and manage governance. Data from this consolidated repository is available for consumption by downstream applications. This style is typically implemented for analytics, business intelligence (BI), and reporting. No effort is made to clean up or improve data in the golden source systems. Improvements made to the data are limited to the hub, so only consuming applications benefit.

### Registry style

The easiest MDM style to implement is the registry or repository style. The registry is typically a simple cross-reference table that provides direct insights into objects that are deduplicated and what relationships have been found between them. It uses an internal reference system and local identifiers (such as customer identifiers) to link back to the original data sources.

This style has no impact on the golden source applications. Each source system remains in control of its own data and remains the golden source. Since there’s no distribution of data from the registry back to the source systems, there’s also no direct impact on data quality.

### Centralized style

In the centralized style, the master data store serves as a central repository for master data for all operational and analytical systems. It collects, links, cleans, matches, and enriches data, then immediately publishes the enhanced data back to the respective (transactional) source systems.

The biggest concern about this style is that it requires intrusion into the golden source systems for two-way synchronization: applications need to conform to use the master data repository as their backend. This conformity pattern can be difficult to implement because application designs can’t always be changed. Off-the-shelf products, for example, usually can’t be modified. Another concern is tight coupling: if you want to change the centralized repository, for whatever reason, you’re forced to change all the other systems too. Although commercial MDM vendors will tell you that this is the best approach for well-managed and governed master data, I advise you to seriously consider any of the other approaches.

### Coexistence style

The coexistence style is used in situations where the master data can’t be used centrally and must be distributed in multiple locations throughout the enterprise. In this style, improvements find their way back to the original systems, so updates are performed within the golden source systems. Coexistence is the most complex implementation style because complex data integration patterns must be implemented for distribution and to ensure data consistency in the golden source systems and master data store.

These four implementation styles aren't mutually exclusive. You can start, for example, with the registry style and move to coexistence later. You can also use different styles for different datasets. You might choose to manage a reporting structure with the consolidated style but customer data with coexistence. When implementing a style, you can also blend different product implementations. It's important to note that the implementation style shouldn't be driven by technology, but rather by your organizational and data needs. That said, not all MDM vendors allow you to switch between or combine the different styles easily. You have to be cautious about which vendor you choose and consider which style(s) you might want to adopt.

## Data Integration

Data integration is a foundational piece of enabling MDM. Many MDM implementations must support both batch and real-time processing; in transactional systems processing typically happens in real time, while analytical systems usually focus on batches for larger quantities of data. MDM must rest on the same foundation we've used so far for distributing and integrating data. In this model, MDM simultaneously plays the role of data consumer and provider by collecting data from and distributing it to sources.

When distributing MDM data, you must balance between consistency of the data and latency. Depending on the situation you're dealing with, different implementation and integration designs are possible.

If the use cases are operational or require low latency, use APIs or events. An approach for microservices is to use state stores: instead of moving millions of records at once, you use an event-driven architecture to expose the MDM records as a streaming database to consuming applications. In this way, the microservices of a domain can be a conglomerate of services developed by the domain and a read-oriented microservice that is responsible for the MDM data.

![](img/999fd6f5dfec4455873f6d26acc75ff0_318_0.png)

Using MDM for critical and time-sensitive operational paths can be complex. For example, if an order management system receives an order and looks for a customer in the MDM system, you need to ensure that the critical matching and merging process happens within milliseconds.

One option for implementing an MDM state store is using a NoSQL database, which is constantly injected with data from the source systems. Within the database, the state store keeps track of what newly created data can be linked to global identification numbers. To streamline this process and improve the quality, you can extend the state store with machine learning capabilities. Having an MDM state store that holds the full transaction log of all events (for example, creates, updates,
