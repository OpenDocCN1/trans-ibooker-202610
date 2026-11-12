
The initial aim of SOA was to make IT landscapes more flexible and provide real-time communication between applications. Prior to SOA, most enterprise application communication took place via direct client/server calls, such as remote procedure calls (RPC).³

The drawback of direct application-to-application communication is that it involves coupling with the underlying application runtime environment. Applications that want to communicate, for example, via RPC methods require the same client/server libraries and protocols. This doesn’t allow for great application diversity because not all libraries and protocols may be available for all languages. Another drawback is compatibility. Versioning is more difficult to manage; if an application method changes, the other application also needs to change immediately because otherwise it might break. The last drawback is scalability: as the number of applications increases, the number of interfaces quickly becomes unmanageable. Typically, the same problems are seen when building point-to-point interfaces between applications.

These drawbacks are the main reasons why engineers and architects started working on a new software architecture based on higher-level abstractions and the key concepts of application frontends, services, service repositories, and service buses. Applications and their application-specific protocols in this model are hidden and expose their functionality and data via services using standard web protocols, such as HTTP. The communication and integration between applications is handled via service buses. The implementation of technologies and methodologies to facilitate this form of communication between (enterprise) applications is known as enterprise application integration (EAI).

## Enterprise Application Integration

In the early 2000s, with use of the web growing, the enterprise service bus (ESB) emerged as a new enterprise application integration platform, part of SOA, to handle integration between different applications and software packages. It added new features above and beyond traditional message-oriented middleware (MOM)⁴ and client/server models, such as:

- **Orchestrating services**
  Composing several fine-grained services into a single higher-order composite service

- **Rule-based routing**
  Receiving and distributing messages based on specified conditions

- **Transformation to standard message formats**
  Transforming one specific data format to another (this also includes data-interchange format transformations, such as SOAP/XML to JSON)

- **Mediating for flexibility**
  Supporting multiple versions of the same interface for compatibility

- **Adapting for connectivity**
  Supporting a wide range of systems for communicating with the backend application and transforming data from the application format to the bus format

The ESB promotes agility and flexibility because it enables companies to more quickly deliver services using buses and adapters. It turns traditional applications into modern applications with easy interfaces. The ESB also decouples applications by providing abstractions and taking care of mediation. The adapter connects the applications and the ESB, and handles the protocol conversions between them.

To facilitate the implementation of the ESB, many software vendors began making their applications and platforms ready for SOA by modernizing their application designs. Standards-based service interfaces emerged based on World Wide Web Consortium (W3C) standards, such as Simple Object Access Protocol (SOAP) with Extensible Markup Language (XML) as the default messaging format.

## What Are WSDL, XSD, and Service Repositories?

XML-based communication is typically supported by WSDL, XSD, and a service repository. Web Services Description Language (WSDL), also written in XML, is a standard used to describe a web service and its operations. It specifies the methods, location, data types, and transport protocol. It eases the work of integration and software development because a developer can develop a web service completely by just having the WSDL provided.

WSDL documents are associated with another XML-based document, called an XSD (XML Schema Definition) document. XSD documents describe the schema, which is a definition of how XML documents are structured. They can be used to ensure that a given XML document is valid and follows the rules laid out in the schema.

XML documents are generally stored in a service repo. This is typically an XML-based registry that publishes information about what web services are available and accessible. The service repo also keeps track of versions, docs, and policies.

SOAP became popular because it uses the Hypertext Transfer Protocol (HTTP) for network communication and has a similar solid structure to HyperText Markup Language (HTML), which is used to build websites. These same HTML and HTTP standards were and still are the foundation for today's internet communication. Based on these communication standards, the ESB acts as a transformation and integration broker, providing universal connectivity between the service providers and service consumers.

![](img/999fd6f5dfec4455873f6d26acc75ff0_173_0.png)

> ESBs, within SOA, can become complex monoliths. I’ll come back to this in “Parallels with Enterprise Data Warehousing Architecture” on page 155, in reference to making the comparison with enterprise data warehousing.

Besides transforming message formats and giving applications a modern communication jacket, the ESB also provides variations of communication in cases where more complex collaboration is required. Two of them are service orchestration and service choreography. I’ll discuss these in detail in the next sections because they make SOA more complex and challenging.

## Service Orchestration

Within SOA, the roles of the service provider and service consumer are sometimes obscured when multiple services are combined, which may be done for efficiency, maintainability, or coordination. Integration is often complex and can occur in different ways. The first pattern we’ll look at is *service aggregation*, in which multiple services are combined, wrapped together, and exposed as a single, larger service. This can be useful to reduce the pain of management and satisfy the needs of (one or multiple) service consumers. The aggregator in this scenario is responsible for coordinating, invoking, and combining the different services.

Service aggregation requires *service orchestration*, the coordination and execution of tasks to combine and integrate different services.5 This is commonly done for technical reasons, such as combining, splitting, transforming, inspecting, and discarding data. The service orchestration process can be short-running, making fairly straightforward combinations, in cases where a simple service flow across services is needed.⁶ For example, in order to get all of a customer’s contracts, another customer’s product service has to be called first. It can also be long-running, combining services into a more complex flow that requires decision logic and different execution paths. Although service orchestration isn’t explicit about the duration, people predominantly use the term in the context of long(er)-running services. Service orchestration requires some disambiguation because it can be done for different reasons.

## Should You Orchestrate Using the ESB or on the Consuming Side?

There are two approaches to combining and integrating services. The first is to use the aggregation services of the ESB, which allow consumers to retrieve all the data with a single call. The ESB plays the role of the orchestrator and does all the heavy lifting that comes with it. Consequently, in this scenario the ESB becomes the single point of failure.

The alternative approach is to let the consuming application play the role of orchestrator. In this case, the application must manage the order of calls and combine (integrate) the results.

Both approaches have pros and cons. The benefit of letting the ESB take care of the orchestration is that it promotes reusability of orchestration or business logic and makes data consumption easier. One drawback is that the ESB has to offer adequate performance in combining all the logic. A far bigger drawback is that the role of the service provider becomes more obscure because service aggregation acts as a facade over the different services. Aggregated services typically combine different services from different owners, so no one true owner can guarantee the integrity of the aggregation service.

Having the consuming application handle the orchestration has the advantage of flexibility, but it’s less efficient because every consumer needs to implement this itself.

Another common reason for orchestration is business process management (BPM). The processes in this situation contain business logic and are typically long-running. The coordination needed to manage the series of steps requires the process status (state) of every step to be stored, and thus persisted in a database. Long-running business processes are interruptible and can wait for other processes (or human tasks) to finish. They are, in general, asynchronous, so for these types of processes

³ RPC is similar to the client/server model; it’s a pattern that lets one program use and request application functionality from another program located on another system on the network. Rather than using messages, RPC uses an interface description language (IDL) for communication between the client and server. The IDL interpreter must be implemented on both sides.

⁴ Middleware, or message-oriented middleware, is an umbrella term for technologies and products that support sending and receiving messages between distributed systems. The ESB can be considered middleware: it’s used to route messages and support reliable processing of these messages.

5 Some people also use the term *service composition*. Service composability principles encourage designing services in such a way that they can be easily reused in multiple solutions, meaning that existing services can be used to create new services. While service composition denotes the fact that services are combined, it doesn’t elaborate on how this is done.

⁶ Service flows, also called *microflows*, are noninterruptible and generally run in a single thread and in only one transaction. They’re short in duration and typically use synchronous services only. The process state usually isn’t persisted.
