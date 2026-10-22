

我们用来构建系统的平台和技术提供了多种基于标准和专有的监控工具，可用于这些目的。监控仪表盘可用于检查每个系统组件的持续健康状况和行为。这些仪表盘使用高度可定制且开放的工具（如 Grafana）构建，可以显示系统指标，并在发生需要操作员注意的各种阈值或事件时发送警报。用于这种复杂的监控能力的术语是 *observability*。

工程师可以利用各种 API，例如 Java 的 MBeans、AWS CloudWatch 和 Python 的 AppMetrics，来为其系统捕获自定义指标——一个典型例子是请求响应时间。使用这些 API，监控仪表盘可以定制以提供实时图表和图形，从而深入了解系统的行为。这样的洞察对于确保持续运营并突出可能需要优化或复制的系统部分非常宝贵。

扩展系统不可避免地意味着添加新的系统组件——硬件和软件。随着组件数量的增长，我们有更多需要监控和管理的活动部件。这永远不是没有成本的。它增加了系统运营的复杂性，以及开发所需的监控代码和可观测性平台演进方面的成本。

控制扩展时可管理性的成本和复杂性的唯一方法是通过自动化。这就是 *Devops* 出现的场景。*Devops* 是一套将软件开发和系统运营相结合的实践和工具。Devops 缩短了新功能开发生命周期，并自动化了系统的持续测试、部署、管理、升级和监控。它是任何成功的可扩展系统的不可或缺的一部分。

## Summary and Further Reading

能够快速且经济高效地扩展应用程序应该是当代面向互联网应用程序软件架构的决定性质量。我们有两种基本方法来实现可扩展性，即增加系统容量，通常通过复制实现，以及系统组件的性能优化。

像任何软件架构质量属性一样，可扩展性不能孤立实现。它不可避免地涉及需要根据应用程序需求进行调整的复杂权衡。我将在本书的其余部分讨论这些基本权衡，事实上从下一章开始，当我描述实现可扩展性的具体架构方法时就会开始。

## References

[1] Ian Gorton, Paul Greenfield, Alex Szalay, and Roy Williams. 2008. Data-Intensive Computing in the 21st Century. Computer 41, 4 (April 2008), 30–32.

[2] Rachel Potvin and Josh Levenberg. 2016. Why Google stores billions of lines of code in a single repository. Commun. ACM 59, 7 (July 2016), 78–87.

[3] Ian Gorton (2017). Chapter 2. Hyperscalability – The Changing Face of Software Architecture. 10.1016/B978-0-12-805467-3.00002-8.

## Chapter 2. Distributed Systems Architectures: An Introduction

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 2nd chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at vwilson@oreilly.com.

In this chapter I’ll broadly cover some of the fundamental approaches to scaling a software system. You can regard this as a 30,000 feet view of the content that is covered in Parts 2, 3 and 4 of this book. I’ll take you on a tour of the main architectural approaches used for scaling a system, and give pointers to later chapters where these issues are dealt with in depth. You can regard this as a “Why” we need these architectural tactics, and the remainder of the book explaining the “How”.

The type of systems this book is oriented towards are the internet-facing systems we all utilize every day. I’ll let you name your favorite. These systems accept requests from users through Web and mobile interfaces, store and retrieve data based on user requests or events (e.g. a GPS-based system), and have some intelligent features such as providing recommendations or notifications based on previous user interactions.

I’ll start with a simple system design and show how it can be scaled. In the process, I’ll introduce several concepts that will be covered in much more detail later in this book. Hence this chapter just gives a broad overview of these concepts and how they aid in scalability – truly a whirlwind tour!

## Basic System Architecture

Virtually all massive scale systems start off small and grow due to their success. It’s common, and sensible, to start with a development framework such as Ruby on Rails or Django or equivalent, which promotes rapid development to get a system quickly up and running. A typical, very simple software architecture for ‘starter’ systems which closely resembles what you get with rapid development frameworks is shown in Figure 2-1. This comprises a client tier, application service tier, and a database tier. If you use Rails or equivalent, you also get a framework which hardwires a Model-View-Controller (MVC) pattern for Web application processing and an Object-Relational Mapper (ORM) that generates SQL queries.

With this architecture, users submit requests to the application from their mobile app or Web browser. The magic of Internet networking (see Chapter 3) delivers these requests to the application service which is running on a machine hosted in some corporate or commercial cloud data center. Communications uses a standard application-level network protocol, typically HTTP.

The application service runs code that supports an application programming interface (API) that clients use to format data and send HTTP requests to. Upon receipt of a request, the service executes the code associated with the requested API. In the process, it may read from or write to a database or some other external system, depending on the semantics of the API. When the request is complete, the service sends the results to the client to display in their app or browser.

![](img/936a72197720a5158b16541313af61c9_31_0.png)

Figure 2-1. Basic Multi-Tier Distributed Systems Architecture

Many, if not most systems conceptually look exactly like this. The application service code exploits a server execution environment that enables multiple requests from multiple users to be processed simultaneously. There’s a myriad of these application server technologies – JEE and Spring for Java, Flask for Python¹ – that are widely used in this scenario.

This approach leads to what is generally known as a monolithic architecture.² Monoliths tend to grow in complexity as the application becomes more feature rich. All API handlers are built into the same server code body. This eventually makes it hard to modify and test rapidly, and the execution footprint can become extremely heavyweight as all the API implementations run in the same application service.

Still, if request loads stay relatively low, this application architecture can suffice. The service has the capacity to process requests with consistently low latency. But if request loads keep growing, this means latencies will increase as the service has insufficient CPU/memory capacity for the concurrent request volume and hence requests will take longer to process. In these circumstances, our single server is overloaded and has become a bottleneck.

In this case, the first strategy for scaling is usually to ‘scale up’ the application service hardware. For example, if your application is running on AWS, you might upgrade your server from a modest t3.xlarge instance with 4 (virtual) CPUs and 16GBs of memory to a t3.2xlarge instance which doubles the number of CPUs and memory available for the application.³

Scale up is simple. It gets many real-world applications a long way to supporting larger workloads. It obviously just costs more money for hardware, but that’s scaling for you.

It’s inevitable however that for many applications the load will grow to a level which will swamp a single server node, no matter how many CPUs and how much memory you have. That’s when you need a new strategy – namely scaling out, or horizontal scaling, that I touched on in Chapter 1.

## Scale Out

Scaling out relies on the ability to replicate a service in the architecture and run multiple copies on multiple server nodes. Requests from clients are distributed across the replicas so that in theory, if we have N replicas, each server node processes {requests/N}. This simple strategy increases an application’s capacity and hence scalability.

To successfully scale out an application, you need two fundamental elements in our design. As illustrated in Figure 2-2, these are:

- *Load balancer*

All user requests are sent to a load balancer, which chooses a service replica target to process the request. Various strategies exist for choosing a target service, all with the core aim of keeping each resource equally busy. The load balancer also relays the responses from the service back to the client. Most load balancers belong to a class of Internet components known as reverse proxies,⁴ which control access to server resources for client requests. As an intermediary, reverse proxies add an extra network hop for a request, and hence need to be extremely low latency to minimize the overheads they introduce. There are many off-the-shelf load balancing solutions as well as cloud-provider specific

