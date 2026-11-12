
comparing data across synchronous boundaries, such as a query via a synchronous request-response API. If you can't risk nonconvergence, then you should ensure that your application or service is consuming directly from the source data products and computing its own results.

The SLAs of your upstream data products are extremely important. Make sure that your service consuming services can tolerate an input data product outage for the period of time specified in the SLA. For instance, if it's possible that your source data product may be unavailable (or not producing data) for say, 5-minutes, then you should ensure your consumer service can handle *at the least* a 5-minute outage (though 15 to 20 minutes would be far safer).

Late-arriving data is not strictly a problem in event-driven data products. Batch-computed data sets can be late as well—a processing job may simply take too long due to resource starvation, failures, or greater-than-usual input data volumes. Down-stream batch jobs that rely on the upstream data sets will either themselves be delayed or produce incomplete results and need to be rerun later. Event streams simply bring consideration for temporal ordering to the fore.

## Summary

Eventual consistency can be a bit tricky to manage, but by and large it's about becoming aware of how it can be introduced and figuring out if it needs to be mitigated at all. Event streams offer each consumer the means to converge on the same final state. While each consumer must uphold its own end of the bargain by correctly processing and integrating the data into its own domain, this responsibility remains the same regardless of whether the data is sourced by event stream, batches of data at rest, or via a request-response API.

In the event-driven space, a lagging consumer service will often be inconsistent with another service or data set that is fully up to date. Comparing data between them will likely show inconsistencies, which is only to be expected, as both are operating within their own frame of relative time. There are many solutions to deal with this issue. One is to simply source data from other contexts via their own event streams. If this is not possible, then plan for inconsistencies and have retry and mitigation strategies in place to deal with them.

Eventual-consistency issues can also arise when data in one data product references data in another that may not yet have been produced, such as any data product where events would be joined by a primary or foreign key. While the consumers may all be up to date with the latest written events, they have not converged to the same state as the upstream producers of the data.

Finally, out-of-order data can introduce additional complications, particularly when many parallel producers write events to the same stream with slightly skewed time-stamps. Consumers can independently handle out-of-order events using late-arrival logic, but they must be cognizant that they may end up interpreting data differently than a neighbor. A best practice is to tie the implementation of late-arrival logic to the SLA of the data product so that your consumers are resilient to any availability or consistency issues within the guarantees of the SLA.

In our next and final chapter, we'll review what we've covered across this book and wrap up with what to look out for in the future of data mesh.

## Bringing It All Together

Data mesh is founded on four principles, based on hard-learned lessons of dealing with difficult-to-use data, often provided as no more than a form of exhaust emitted by operational applications. Mechanisms for obtaining and accessing data outside of its original source are typically added as an afterthought, bolted onto the source system in an ad hoc manner.

Data mesh promotes data to a product with the same rigor, ownership, and feature management of any other product in your business. The free-for-all, “figure it out yourself” data access is replaced with purpose-built, maintained, and supported modes. It is as much a social shift as it is a technological shift and requires both top-down and bottom-up buy-in. We reevaluate how we own, create, discover, and access data, and then build out the processes, governance, and technology required to make it work.

Event streams provide the number one option for making data mesh a reality. They provide a singular mechanism for both historical and real-time data communications, forming the data mesh’s basic building blocks for consumers to use, mix, and match to their needs.

Data mesh is based on four main principles: domain ownership, data as a product, federated governance, and self-service platform. Together, these principles help us focus on communicating important business data across the entire organization safely, effectively, and at-scale:

- Domain ownership

Those who know the data best are charged with the responsibility of making it readily available for their peers and colleagues to use and access as they see fit. Domain owners consult with prospective consumers to source data requirements and ensure that business needs are met. They remain responsible for protecting the internal domain model from overexposure and unacceptable levels of coupling by external parties so that both the internal model and the external data product can independently evolve and change.

### Data as a product

Important business data needs to be readily and reliably available as building block primitives for your applications, regardless of the runtime, environment, or codebase of your application. We can accomplish this goal by focusing on creating data as a product, treating data as a first-class citizen, complete with dedicated ownership, minimum quality guarantees, SLAs, and scalable mechanisms for clean and reliable access.

Data products act as the basic building blocks for composing your business services, enabling consumers to access and use the data for their own use cases.

### Federated governance

Federated governance concerns itself with maintaining stability and order, balancing individual autonomy and top-down centralized control.

A governance team, drawn from those participating in the data mesh, is empowered with addressing the needs of domain owners, data product creators, data product users, and infrastructure providers. Like any form of effective governance, the governance team needs participation, representation, debate, and collaborative action to get meaningful work done. The governance team guides the requirements for the self-service platform, focusing on increasing discoverability, ease of use, and intercompatibility with other data products.

Successful federated governance results in data products that are easy to build, manage, and use. Data product owners should have reasonable options for building and managing their data product in the self-service platform. They should also be provided with guardrails to support necessary nonfunctional business requirements, like encryption, access controls, and automated data retention management.

### Self-service platform

Just as every data mesh implementation will be unique unto itself, so will its self-service platform. The main goals of creating a self-service platform include making it easy to:

- Browse, discover, and search through available data products
- Grant, restrict, and manage access controls
- Requisition compute, storage, and serving components, including event streams
- Manage the data product life cycle, including prototyping, publishing, deprecating, and deleting

Building a self-service platform requires working closely with data product owners and the federated governance team. Your best chance for success involves starting with a minimum viable product consisting of technologies and control systems you’re already using. Treat your self-service data platform like any other product, iteratively adding and testing new capabilities as usage patterns and requirements become clearer.

### Event Streams for Data Mesh

An event is an immutable and self-contained record of something that happened. An event stream is a continuously updating data structure of events, with each stream representing a selection of important business facts.

Event streams are the ideal mechanism for serving data products, as they provide a simple yet powerful way of reliably communicating important business data across an organization. Event streams are fast, scalable, and efficient, letting consumers know when new data is available as it happens and enabling real-time reaction across your business for any use case.

State events form the best option for building data products, though you do have some leeway in your options. You can include information such as _why_ the state changed, combining some of the aspects of a delta event with the state event. But remain cautious about overexposing specific business logic transitions from the source domain in your data products because they introduce strong coupling seams.

The Kappa architecture works hand-in-hand with state events. You can source the entire history of the data product from just the event stream instead of splitting historical and real-time data access in two, as with the Lambda architecture. Historical state can be accessed by consuming the event stream from an earlier point in time. You can use compaction to eliminate older, no-longer-relevant state events, making it fast, easy, and inexpensive for consumers to access the data.

Simplicity is key for making a data mesh work, and event streams provide a simple yet powerful solution for making data readily available.

### Integrating with Existing Systems

Data mesh implementations are rarely (if ever) greenfield developments. The pains that data mesh resolves are incurred largely by the growth of an organization and the demands for important business data by multiple disparate use cases. Thus, when building a data mesh, you’re going to need to contend with all of your existing systems and iteratively work toward improving how data is created, shared, and used across your organization.
