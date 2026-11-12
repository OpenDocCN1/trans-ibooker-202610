experts to describe data. Another approach is crowdsourcing: inviting communities of users or the public to provide context. Scanners and annotation tools may be used as well; these typically use algorithms to detect duplicates, classify or rate data, and predict what data belongs together.

# Integrated Views

*Integrated views* are predefined queries that re-create data, using the same SQL statements, at execution time. This approach to ensuring consistency is common in both MDM and EDW architectures. Integrated views are sometimes also called *virtual tables*, because it feels like you’re querying a table, but in fact it’s the query that is saved under the view.

The difference from the previous approach (providing semantic consistency through metadata) is that views can contain business logic. They can even create or generate new data elements. These changes aren’t only syntactic translations; they could be context transformations as well. Another difference is that data can be duplicated: views can be materialized, meaning that the results are copied and persisted. This is especially useful for improving query performance.

Views can also be combined with metadata. They can, for example, be generated from metadata and automatically change when the metadata changes.

# Reusable Components and Integration Logic

Another way of facilitating data collaboration and reusability is *code sharing*. Here, it’s not the curated data that is shared but the underlying code (templates, snippets, and scripts) used to generate the outputs. This code is stored in a central and open repository, including versioning, allowing DevOps teams to contribute and improve upon what has been published.

The benefit of this model is that business logic is applied only within domains, which allows teams to alter and optimize this logic as they see fit. In addition, the outputs regenerated as improvements from the community find their way to the central code repository. One drawback of this model is consistency, since allowing teams to modify the code can make comparing results between teams more difficult.

# Republicising Data Through Integration Hubs

Several database vendors advocate that data curation can be done by encapsulating the original sources. Domain teams, in this approach, extract and load data into an integration platform, and then republish or distribute it. During this process, the semantic context can be changed, so transformations and enhancements can be applied to existing datasets. New datasets can be created as well.