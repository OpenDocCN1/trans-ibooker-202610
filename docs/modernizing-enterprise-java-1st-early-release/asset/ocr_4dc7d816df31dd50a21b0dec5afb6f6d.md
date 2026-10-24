[Jaeger](Jaeger) is a popular open-source tool that connects to Kubernetes to provide observability. It uses distributed tracing to follow the path of a request through different microservices. It provides a visual representation of the call flows through a dashboard, and it is often also integrated with Service Mesh. Jaeger is very helpful to developers for monitoring distributed transactions, optimizing performance and latency, and performing root cause analysis.

# Logging

As we discussedm a single call in your microservices-based app, such as the Coolstore example, can invoke different services that interact with each other. It’s important to monitor and observe the app, but also to store relevant pieces of information in logs. Your application’s logging approach changes with modern apps. While in monoliths we usually rely on multiple log files stored in different paths on the disk, usually managed by the application server, distributed apps *stream* logs. As your app can scale up rapidly and move to different nodes or even clouds, it makes no sense to access the single instance to retrieve logs, a distributed log system is needed.

Kubernetes makes logging easy as well. By default, it provides the capability to access a pod’s logs by reading the application’s standard streams such as STDOUT (Standard Output) and STDERR (Standard Error). Thus, the app should not write logs into a certain path, but send logs to standard streams.

> **NOTE**
> 
> It is still possible to store logs in specific paths that can also be persistent in Kubernetes, but this is considered an anti-pattern.

Kubernetes also interacts with distributed logging systems such as [Elasticsearch](Elasticsearch), an open-source document oriented NoSQL database based on Apache Lucene⁷ to store logs and events. Elasticsearch usually comes with