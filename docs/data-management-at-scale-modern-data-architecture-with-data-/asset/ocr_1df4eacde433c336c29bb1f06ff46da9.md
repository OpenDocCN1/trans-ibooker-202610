+   • Keep domain logic in the domain.
+   • Build services for consumers using the right level of granularity.¹⁰
+   • Simplify and use modern community standards.
+   • Allow service orchestration (combining multiple API endpoints into one) only within the boundaries of the domain (bounded context).
+   • Use domain identifiers consistently so that domains can identify the interconnections between resources.

# Resource-Oriented Architecture

Within software engineering there's a pattern called *resource-oriented architecture* (ROA). ROA is a specific set of guidelines for a RESTful architecture. Data is modeled into collections of homogeneous subject areas or resources, so what logically belongs together is grouped together into a collection or resource. Each resource is an entity that can be identified using a URI. For example, the resource *customers* is used to return all relevant information about customers, which might include names, addresses, and contact information. The data resources in this pattern are noun-oriented and should be self-discoverable and self-explainable as well. (SOA, by contrast, is typically verb-oriented.)

Following these guidelines will help make your API architecture scalable. It allows domains to stay decoupled while leveraging other services without performing too much repeatable work. To facilitate this way of working, a more lightweight integration service must be considered.

# API Gateway

With RESTful APIs and the modernization trends in applications, communication and service integration between applications started to change. REST, introduced in “Resources and Operations” on page 146, is an architectural pattern used in modern web-based applications to communicate statelessness. It’s based on simplicity: resources—simple represented chunks of related information—are identified by uniform resource identifiers (URIs) and can be connected with hypermedia links.¹¹ It also uses HTTP with uniform interface methods. RESTful APIs became popular due

¹⁰ Filipe Ximenes and Flávio Juvenal have written a blog post about how RESTful APIs can represent resources that are correctly formatted, linked, and versioned. ¹¹ HATEOAS (Hypermedia as the Engine of Application State) is used to link REST resources. You can find a tutorial on this on the REST API Tutorial website.