
## Exam Essentials

**Understand hybrid solutions.** Hybrid solutions are a combination of resources in the cloud and the ones that you run in an on-premise or other private data center. Hybrid solutions can be, but are not entirely limited to, security, networking, compute, and data.

**Understand Azure Policy.** This a feature that allows an administrator to enforce restrictions on a management group, subscription, or resource group. The restrictions can be aligned with a compliance standard or just something your enterprise requires, for example, requiring TLS 1.2 on all endpoints that expose an HTTP interface.

**Know how to use tags.** You can use these as another means of organizing your provisioned Azure resources. When a resource group doesn’t quite meet your requirements, or even within the same resource group, use tags as an additional means for marking your resources, whether the resource is a development, test, or production environment.

**Know how to use Security Center.** This is the place where you manage most of your security configurations and monitor your Azure resources. You receive reports of compliance issues, compliance scores, and instructions on how to configure the resources that are not compliant.

**Know how to use Service Bus.** Service Bus is a messaging-based storage queue used for decoupling IT solutions from each other. A sender places a message into a queue that can be monitored for duplicates and stored and processed in the received order. There can be multiple receivers that process messages in the same queue in parallel, or by using a Service Bus topic, multiple receivers can receive and process the messages for their own purpose.

**Know how to use Event Grid.** Use this product for the ingress of telemetric, streaming, or huge big data message processing. Your Event Hub can have a single partition, which a producer adds messages into, with a single consumer group and a single receiver to process them. You can have a maximum of 32 partitions and a consumer for each one, which makes the processing of such data extremely parallel, fast, and efficient for these high-level throughput operations.

## Key Terms

Advanced Message Queueing Protocol (AMQP)    disaster recovery (DR)
allOf    disaster recovery as a service (DRaaS)
authentication (AuthN)    integration and testing (I&T)
authorization (AuthZ)    label

## Review Questions

Many questions can have more than a single answer. Please select all choices that are true.

+   1. Which of the following can you not create a hybrid solution with using Azure products?
+      A. Azure Active Directory
+      B. Azure Virtual Network
+      C. Azure Policy
+      D. Azure Virtual Machine
+   2. Which networking products are helpful for creating a hybrid networking solution?
+      A. API Management
+      B. Network Watcher
+      C. Hybrid Connection Manager
+      D. Single-sign on
+   3. Which of the following are helpful for implementing and monitoring compliance standards?
+      A. Azure Compliance Standards
+      B. Azure Policy
+      C. Security Center
+      D. Azure Blueprint
+   4. Which of the following are helpful for grouping resources that help with management and overview?
+      A. Resource groups
+      B. Event Grid domains
+      C. Subscriptions
+      D. Tags
+   5. True or False? Reliability means that a system can recover quickly from a transient outage. Resiliency is the outcome of a reliable system.
+      A. True
+      B. False
+   6. True or False? GDPR and PIPA are data privacy compliance standards.
+      A. True
+      B. False
+   7. Which of the following features are configurable in Security Center?
+      A. Policy and compliance analysis
+      B. Resource security hygiene
C. Multifactor authentication (MFA)
D. Threat detection

8. True or False? Security Center automatically enforces compliance standards once an Azure product configuration is flagged as noncompliant to its associated compliance standard.
A. True
B. False

9. If you wanted to ingest streaming data or telemetry information into a data source and you wanted to queue that data using a messaging service, which one would you use?
A. Event Grid
B. Event Hub
C. Service Bus
D. Azure Storage Queue

10. If you need to process messages that arrive in a queue in the order they are received, which Azure messaging service would you choose?
A. Azure Storage Queue
B. Event Hub
C. Event Grid
D. Service Bus

## Chapter
7

## Developing
for the Cloud

EXAM AZ-303 OBJECTIVES COVERED IN THIS CHAPTER:

+   - Implement Management and Security Solutions
+     - Manage security for applications
+   - Implement Solutions for Apps
+     - Implement an application infrastructure

EXAM AZ-304 OBJECTIVES COVERED IN THIS CHAPTER:

+   - Design Identity and Security
+     - Design authentication
+     - Design security for applications
+   - Design Business Continuity
+     - Design for high availability
+   - Design Infrastructure
+     - Design an application architecture
+     - Design a compute solution
+   - Design Data Storage
+     - Design a solution for databases

![](img/95043b3df926bcdadadb082b49262037_552_0.png)

![](img/95043b3df926bcdadadb082b49262037_553_0.png)

At the start of Chapter 5, I told you to take for granted that you have an application. That application could be a simple snippet of code or a complicated enterprise system that uses inheritance, interfaces, and third-party functional libraries. No matter how well you designed, implemented, and configured the security, networking, compute, and data stores, if your code is bad, the other components don’t matter so much. Well, from a security perspective, that’s not 100% true, because security always matters, but I think you get what I am trying to say here. In short, there are coding patterns that you can follow that will increase the probability of success in the cloud. This chapter shares some of those patterns and provides some coding examples. It also covers IDEs, source code repositories, and more about coding for security.

Don’t expect many, if any, questions about coding on the exam. This chapter is present more for making you the best Azure Solutions Architect Expert than it is for helping you prepare for the exam. As Ian Stewart once said, “There are 10 kinds of people in the world: those who understand binary numerals and those who don’t.” In this chapter, you will learn some coding patterns and tools for creating and managing source code.

## Architectural Styles, Principles, and Patterns

In this section, you will find information about the following topics:

+   - Architectural styles
+   - Design principles
+   - Cloud design patterns

Patterns have been called out in a few chapters in various phases of IT solution design and configuration.

+   - Architectural styles such as multitiered applications
+   - Enterprise data warehouse (EDW) designs such as snowflake and star schemas
+   - Design principles such as partitioning

There are design patterns that should be implemented into your code when running in the cloud. This chapter provides guidelines and suggestions for implementing all the

best-practice styles, principles, and patterns into the solutions that you plan on moving or creating on the Azure platform.

## Architectural Styles

The most common or traditional architecture style is called *multitiered* (aka *n-tier*), which is illustrated in Figure 7.1. In this design, you will see the existence of a client that connects to a web server, which is one of the tiers. The web server then can make connections to an application server and/or a database server.

FIGURE 7.1 A multitiered architecture

![](img/95043b3df926bcdadadb082b49262037_554_0.png)

Table 7.1 lists some of the other common architectural styles.

| Architectural Style | Description | Azure Product |
|---|---|---|
| Multitiered (n-tier) | A division of application roles by physical machines | Azure Virtual Machines |
| Big Data | Unstructured data collection and analysis | Azure Cosmos DB
Azure Synapse Analytics
Data Lake Analytics |
