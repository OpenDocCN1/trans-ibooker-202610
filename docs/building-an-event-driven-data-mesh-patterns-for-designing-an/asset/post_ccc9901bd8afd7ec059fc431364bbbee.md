
- Eliminating data inconsistencies between analytics and operational systems by using event streams as the single source of truth.

Data mesh relies on four interrelated principles. We’ll investigate each of these principles in brief, along with the role that event-driven data plays in them. The following chapters will dig into a deeper discussion of the principles, the relationships between them, and specifics for building a data mesh powered by event streams.

## Principle 1: Domain Ownership

Domain ownership is all about enforcing sovereignty over one’s own domain, without having to seek approval or permission from others to change or modify it. This principle ensures a definitive answer to the question of “Who is in charge of this data set?” by unequivocally answering, “the domain owner.” But with total ownership comes an important set of responsibilities: to export a selection of internal domain data in a mode suitable for use by those outside your domain boundaries.

In a data mesh, the owner of that data becomes responsible for making it readily available—it is no longer up to prospective users of the data to try to figure out the means to access it. For one, the team that owns the source data is the best qualified to determine what parts of the model should (and should not) be exposed to downstream consumers. Secondly, establishing data ownership at the source formalizes the boundaries of responsibility, simplifying the ownership issues that have plagued traditional centralized data teams.

But responsibility extends beyond modeling data for export to include handling on-call data issues, ensuring that the modes of access remain reliable, ensuring that evolution of the data model remains compatible with historical expectations (covered more in “Schema Evolution: Changing Your Schemas Through Time” on page 137), and ensuring that the agreed upon SLAs are met. This isn’t simply a change in who models the data for export but rather a total shift in ultimate responsibility. Moving ownership of data modeling and availability to the producer is a fundamental shift away from the centralization common to today’s data warehousing and data lake strategies. Instead of data engineers and data scientists trying to figure out how to get data from within a service’s boundary (e.g., smash and grab into an ETL job), the owner of the source data must strive to make the necessary data available through a well-supported and reliable medium.

## Domain-Driven Design in Brief

Domain-driven design (DDD), as created and articulated by Eric Evans in Domain-Driven Design (Addison-Wesley), is a software design approach applied to data and domain modeling. It focuses on modeling the structure and language of the software model to match that of the business domain. Much like data mesh, it too provides us with a common language to discuss software and data models. We'll borrow from DDD's common language to start the discussion on how to share data between systems. Distinguishing between data in here and data out there is essential for navigating the social changes and delegation of responsibility necessary for data mesh to succeed.

To begin with, we have the domain, as in domain-driven design. A domain is the area of interest or concern over which a person or team has control, including all of the entities, aggregates, business logic, and context that compose it. The domain, in a nutshell, consists of the things that we concern ourselves with, in a specific area, as we try to build a solution for a problem.

Next is the bounded context. A bounded context is defined as the boundary within a domain where the domain model applies. In practice, a bounded context also contains what’s known as a ubiquitous language, which describes every component within the boundary, including how it can be identified, its relations to other components, and how it’s modeled.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_40_0.png)

Ubiquitous language is ubiquitous only within the bounded context—in another bounded context, the same term may (and often does) mean something different. Subtly different meanings may cause confusion and inconsistent data interpretation and usage.

Entities are uniquely identifiable things or items that are defined within the domain. These are best thought of as objects with unique attributes that often change and evolve over time. For example, an ecommerce platform's entities include items for sale, coupons, deals, shipments, and warehouse-related matters like creating the package and managing inventory levels.

Finally, an aggregate is a cluster of entities that can be treated as a single unit. Further to our ecommerce example, aggregating the coupons, items, deals, shipping, and payment information forms the basis of an order. To compose this order aggregate, however, you need to apply domain-specific business logic to integrate each of the entities into the aggregate—the ordering and manner in which it’s applied is a concern solely for the domain’s aggregate owner.

Figure 2-1 shows a simplified automotive manufacturing domain, with a single bounded context containing entities, aggregates, and business logic. Each entity, aggregate, and process has a fully consistent meaning within the boundary, and any exposure of the inner data to the outside world is through the anti-corruption layer.

With these definitions, we can start discussing precisely what it is we’re trying to do: publish important entities and aggregates from within a domain’s bounded context for use in other domains. All data made accessible outside the domain will be through an anti-corruption layer, as we want to control what data is and isn't exposed to ensure that external parties do not couple directly on the internal model. This provides the domain owner with the ability to evolve and change the internal implementation without unduly affecting external data consumers.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_41_0.png)

_Figure 2-1. An example of an automotive manufacturing domain, including the anti-corruption layer_

But how do we know what data to select and share outside our domain?

## Selecting the Data to Expose from Your Domain

Deciding what data to expose from your domain can be a bit tricky. One of the best places to start is to identify the entities that are fundamental to your domain, as they are often good first candidates to evaluate. Given the ecommerce example of the previous section, we could expose data relating to the items available and the orders that have been created. Since this domain also maps to the physical world, it would be reasonable to also expose the current inventory to ensure that stock on hand is correctly displayed to our end users.

We would also likely expose select payment information as its own data set because this would provide important information for finance teams. We can link payments to orders via unique ID mappings, allowing downstream consumers to merge them together as they need, or simply use the data that is only relevant to their use cases.

However, the best way to determine which domain data is necessary for others to use is to simply ask them. Take a customer-focused approach and lean into the needs of those business processes that depend on your domain. And this brings us to the next data mesh principle.

## Principle 2: Data as a Product

This principle elevates data to the status of first-class citizen, the same as any other product created by the organization. This is a complete reenvisioning of how data is created, stored, and communicated to other domains. By applying the same product rigor of existing production products, we can formalize how data is built and communicated across domains.

The domain owners are responsible for identifying, extracting, and modeling their data to present as a data product for external access. After all, who better than the domain model experts to construct the data for export? And like any other product, prospective consumers are identified and consulted to ensure that the data product is built to serve their needs. But a data product isn’t just the data itself—it’s also the code that builds it, the infrastructure that stores it, and the ports (or modes) by which you can access it, as shown in Figure 2-2.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_42_0.png)

Figure 2-2. The code, data, infrastructure, and ports that comprise a data product

These four components are common to all data products. Every data product creator is going to need to identify and construct their data products with these in mind.

We’ll streamline the creation and management of data products, much like we do with microservices, to reduce the overhead and toil of each data product owner. We want it to be easy to build and support data products without getting bogged down in the operational overhead. We’ll cover self-service later in this chapter, but keep it in mind as we talk about serving data as a product.
