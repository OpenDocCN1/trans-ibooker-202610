
metadata by scanning. After scanning, it asks data experts and data engineers to provide additional information and correlate entities by creating relationships. After the Graphmarts are created, users can add Data Layers to their Graphmarts for data cleansing, transformation, linking, and access control, dynamically enhancing the in-memory graph in an iterative manner.

Figure 9-4 shows how Graphmarts and Data Layers are managed. The neat part is that you can map metadata and sources into layers and build another series of layers on top of this. For example, your first layers could be “source layers” that directly take your sources and metadata as input. The metadata for building your first layers could come from ontologies (OWL files) you provide, or metadata that is discovered by scanning. Your second layers expand the first layers by connecting entities and making improvements using SPARQL. These are the Data Layers. Then, you could add another series of layers for making projections that are specifically designed for data consumers. These layers are also called Data Layers, but they’re specially designed for consumers using semantic layers. Between all of the layers, there are relationships. The recipes that describe the sequence of transformation steps that are applied are stored as ontologies in the RDF format. They include references to other ontologies describing the contents of each layer.

![](img/999fd6f5dfec4455873f6d26acc75ff0_305_0.png)

Figure 9-4. A screenshot of how Anzo visualizes and connects entities within a graph: the models represent datasets that you can execute queries against

The advanced aspect of this vision is that you can also set up knowledge graphs in a distributed model, with some parts maintained by domains and another (central) part maintained on a central or enterprise level. This central part forms the core of your model. Of course, when you implement a central model, it’s important to set enterprise-level standards for the most important metadata objects and how they relate to each other.

When you implement a distributed design, your domains can extend the central metadata model without compromising the core model. For example, a marketing domain team might add their own domain-specific entities or properties to this model for the context and data they need. They might even make their models publicly available for other users or consumers. Everything can easily be extended, combined, and queried together, as long the underlying data repositories are accessible and the relationships to the resource (data) locations are embedded.

## Data fabric for metadata management

You can expand the vision of using the semantic web standards for metadata management even further, and link it all the way to your enterprise metamodel. Your metamodel then becomes a semantic layer that sits on top of your knowledge graph, in which all your important metadata is stored. You can use this semantic layer for navigating, searching, and answering complex queries. In other words, you can query your metadata just the way you would query your regular data using a knowledge graph. With this approach, for example, central data privacy classifications can be linked to conceptual terms, and since you have the underlying links to the application data endpoints, you know what data attributes are sensitive.

The knowledge graph, seen as a giant semantic web of entities and their attributes, allows you to find the closest-matching entities based on semantic similarities between them. You can also allow domains to extend their domain-specific knowledge graphs with domain-specific vocabularies, taxonomies, or ontologies, as shown in Figure 9-4. Although the ontology-based approach is difficult to implement, its impact can be huge: it can greatly improve the overall consistency of the data landscape and boost data consumption and productivity; it can completely change the way users interact with complex data representations; it may automatically generate ontologies, classifications, graph relationships, or embeddings for natural language processing, recommendation, and search algorithms. So, it is important to prepare yourself for this data fabric development.

The technologies behind the data fabric utilize the same open standards that drive the web. Internal knowledge and the meaning associated with data thus becomes accessible to everyone. Big tech companies often integrate this technology with artificial intelligence, machine learning, and NLP. You can even connect this vision to the disciplines of data integration or data security: for example, you could integrate and transform data automatically based on the semantic context and relationships provided, or dynamically change data access based on process descriptions documented in one of the knowledge graphs.

A data fabric, as an intelligent and metadata driven–architecture, can be a great solution for managing metadata at scale. When using a knowledge graph–based approach for metadata management, I recommend taking it step by step. Don’t try to create enormous charts with all connections that only a small group of technical people will understand. Build it up slowly, and make the model accessible for business users, too, so everybody benefits from new insights. In the end state, you can apply advanced algorithms and additional tools to the knowledge graphs to enable intelligent data consumption, improve overall quality, and detect hidden patterns.

## Metalake solution design

For designing a knowledge graph architecture, consider mirroring the patterns that we discussed in earlier chapters. Metadata, just like regular data, often needs to have its quality validated and be cleansed and integrated with other metadata before it can land in your knowledge graph for metadata management purposes. To get started quickly, reuse blueprints and services from your data product architecture. Figure 9-5 clearly shows the overlap with some of the patterns from Chapter 4. Layered on top of these are several services from the API architecture for real-time communication between frontend services and the underlying services in which metadata is stored.

![](img/999fd6f5dfec4455873f6d26acc75ff0_307_0.png)

Figure 9-5. A metalake solution design showing how a knowledge graph can be used to fuel a data marketplace—the marketplace frontend services are there to visualize end-to-end data management insights

The relationship between your data catalog and knowledge graph is strong; the majority of the metadata is managed by your data catalog. The difference between these two is that your knowledge graph is more flexible, easier to adapt, and broader: it covers a wider range of metadata and can integrate metadata from many different places. The data catalog, on the other hand, is scoped to finding and searching for data for the purposes of data use and data management.

![](img/999fd6f5dfec4455873f6d26acc75ff0_308_0.png)

I expect future-generation data catalogs to look different. At some point, data catalogs will become open and interoperable knowledge graph platforms with many surrounding services for visualization, workflow management, integration, and so on.

Once you've successfully designed your knowledge graph and started to use it for managing your data management metadata, you can also use it to monitor the maturity of your enterprise architecture. For example, using the business capability model for Oceanic Airlines from Chapter 2, Figure 9-6 shows how you might indicate the data management maturity level of each business capability within your organization.

| Section | Item 1 | Item 2 | Item 3 | Item 4 | Item 5 | Item 6 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Customer and service management | ✅ Ticket and sales | ❌ Customer management | ✅ Discount and loyalty | ✅ Bookings and commissions | ✅ Advertising marketing | ✅ Reservation and planning |
| Staff and personnel management | ✅ Recruitment | ✅ Personnel management | ⚠️ Incident handling | ✅ Pilot management | ✅ Baggage handling and lost items | ✅ Purser management |
| Airflight management | ✅ Airplane maintenance | ✅ Fuel optimization | ✅ Flight optimization planning | ⚠️ Logistics and planning | ✅ Engines and spares | ✅ Flight plan management |
| Supporting service management | ✅ Assets and lease | ⚠️ Finance | ✅ Information technology | ✅ Emission and fees | ✅ Income and taxes | ✅ Regulatory procedures management |

Figure 9-6. Business capability model showing the maturity of each business capability through the lens of data management
