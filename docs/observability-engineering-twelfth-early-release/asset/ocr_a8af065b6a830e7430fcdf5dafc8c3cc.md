value pairs to each event, you will create a wide, rich view of what was happening in all the parts of your system that are typically hidden and impossible to reason about.

For example, in a distributed, cloud native system, you may find it difficult to debug an issue occurring across multiple hosts by using logs or other noncorrelated signals. But by utilizing a combination of signals, you could systematically drill down into anomalous behavior starting from the high level of your service metrics, iterating until you discover which hosts are most relevant. Sharding logs by host means you no longer need centralized retention and indexing of all logs as you would for a monolith. To understand the relationships among subcomponents and services in a monolith or distributed system, you previously may have needed to keep the relationships of all system components in your head.

Instead, if you can use distributed tracing to break down and visualize each individual step, you need to understand the complexity of your dependencies only as they impact execution of a specific request. Understanding which calling and receiving code paths are associated with which versions of each service allows you to find reverse compatibility problems and any changes that broke compatibility.

Observability provides a shared context that enables teams to debug problems in a cohesive and rational way, regardless of a system’s complexity, rather than requiring one person’s mind to retain the entire state of the system.

# Observability Empowers DevOps and SRE Practices

It’s the job of DevOps and SRE teams to understand production systems and tame complexity. So it’s natural for them to care about the observability of the systems they build and run. SRE focuses on managing services according to service level objectives (SLOs) and error budgets (see Chapter 12 and Chapter 13). DevOps focuses on managing services through