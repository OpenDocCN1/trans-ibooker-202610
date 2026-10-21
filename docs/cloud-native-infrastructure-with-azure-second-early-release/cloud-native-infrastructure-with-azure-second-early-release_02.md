## 第 1 章. 引言：为什么云原生？

### 早期发布读者须知

使用 Early Release 电子书，您可以在官方发布这些标题之前很久就利用这些技术——在作者撰写时获取书籍的最原始和未编辑内容。

这是最终书籍的第 1 章。请注意，GitHub 仓库将在稍后激活。

如果您对如何改进本书的内容和/或示例有任何评论，或者如果您注意到本章中缺少材料，请联系编辑 rfernando@oreilly.com。

尽管使用云提供了一种解决应用程序可扩展性、可用性和可靠性问题的方法，但它并不是万灵药。我们不能将应用程序放入云中并期望它们永远正常运行，也不能将应用程序打包到容器中以将其转变为可在云中顺畅运行的微服务。为了充分利用云所提供的优势，我们需要以云为先导来开发基础设施和服务。

要真正理解云原生之旅及其重要性，我们需要先回顾互联网早期的基础设施和服务世界是什么样子的。所以，让我们踏上这段旅程。

## 通往云的旅程

在互联网早期，整体的 Web 应用程序基础设施是使用物理服务器托管的，在应用程序可以从中提供服务之前，需要采购和准备这些服务器。IT 团队必须亲自购买服务器并在本地设置它们，安装相关的服务器操作系统，准备环境，然后在其上部署应用程序。这种方法存在许多问题：例如，您会有资源利用率低的服务器（因为您永远无法完全使用它们），运行多个应用程序很困难，并且设置和维护成本很高。*虚拟化* 被开发出来以允许更有效地利用物理服务器。虚拟化在物理硬件之上创建了一个抽象层，允许底层资源（如处理器、内存和存储）被划分和共享。

虚拟化解决了资源利用率和多租户的许多问题，但您仍然需要拥有硬件来部署应用程序，并且仍然需要维护运行数据中心的全部开销。这催生了基础设施即服务（IaaS）的需求，其中服务器由负责应用程序底层基础设施的第三方拥有。这是云计算时代的开始，它使公司能够专注于其应用程序和底层环境，而无需担心硬件、开销或配置问题。IaaS 之后是平台即服务（PaaS），它通过分离底层软件环境和运行时进一步减少了繁琐工作。这意味着开发人员只需专注于编写应用程序和定义依赖项，而服务平台将完全负责托管、运行、管理和公开应用程序。PaaS 为软件即服务（SaaS）的出现铺平了道路，SaaS 俗称“按需软件”，以按使用付费的方式向消费者提供应用程序即服务。

随着云计算的日益普及，利用云更高效地使用云并发挥云基础设施及其各种产品全部潜力的云原生技术理念也随之兴起。这催生了云原生基础设施和云



native application development. Cloud native infrastructure creates an abstraction on the cloud provider’s underlying infrastructure and exposes the infrastructure with APIs. This infrastructure management philosophy makes it very easy to scale and reduce underlying complexity, which indirectly improves availability, resiliency, and maintainability. Similarly, cloud native applications fortify the bridge between the application and infrastructure by incorporating supporting features such as health checks, telemetry and metrics, resiliency, a microservices environment, and self-healing.

Let’s now take a look at challenges in the cloud computing environment.

### Challenges in the Cloud

Public cloud providers have become a dominant enterprise solution for a lot of growing industry needs and business requirements. They give you advantages such as higher availability and scalability along with the flexibility to design your applications in a way which utilizes cloud services. When cloud solutions were first introduced, numerous challenges including security, effective cost management, compliance, and performance concerned potential customers. Those early challenges are now a thing of the past for the majority of cloud consumers, as they have been overcome with advances in both cloud provider technologies and the way enterprises deploy solutions on the cloud.

Even though we have come a long way, this does not mean the cloud is perfect. There are still challenges in the cloud landscape, but they look very different from the ones we faced back when the cloud was still new. Customers today now have to consider the following challenges:

Too many choices

There are plenty of cloud providers out there with an extensive array of services to choose from. This means you need to hire expert architects and engineering teams who know how to operate the services and use them according to your business use case. Not only is hiring these engineers difficult, but finding engineers who are experts in a specific field requires a significant time investment.

### Rapid growth and development of cloud services and technologies

A huge number of new cloud services are being released by cloud provider giants like Amazon, Microsoft, and Google. This results in a greater need to train engineers in these new services and a greater need to maintain these services in the future as the applications grow. Often, the level of investments in these services also indirectly causes vendor lock-in, resulting in a spiral of future constraints in application design.

### Multiple generations of technology

As we have moved into the cloud era, we have also lifted and shifted our application stacks from various generations of infrastructure solutions, ranging from virtual machines to containers to serverless technologies. This migration of applications requires a significant amount of effort to understand the underlying technologies and support them in the future.

### Growing operational complexity

These rapidly growing technologies, combined with accelerated migration of workloads to the cloud, have given rise to operational complexity and an ever-growing list of factors to look out for, including storage systems, security models, governance models, and management platforms, among others.

### Evolution of business needs and technologies

New areas of technology and cultural change have also rapidly evolved the enterprise architecture. For instance, with the advent of the DevOps culture, a new application that once took weeks or months to develop can now be rolled out in a couple of minutes. More advanced areas, such as data science and machine learning, have also come into the picture, increasing business needs and overall engineering maturity.

So, despite the power of cloud computing, businesses had to wade through numerous complexities. Eventually, it became clear that businesses wanted to have the speed, scale, and margin of the cloud but none of the overhead. To accomplish this, it was necessary to embrace the cloud native approach of application building, which would help companies take full advantage of the cloud.

### Cloud Native Computing Foundation

As more companies are adopting cloud native technologies, creating software in-house, and closely partnering with other businesses to quickly bring products to market, strides are being made to improve the cloud native domain. One organization that is helping to lead the way in this effort is the Cloud Native Computing Foundation (CNCF). Their mission is as follows:

Cloud native technologies empower organizations to build and run scalable applications in modern, dynamic environments such as public, private, and hybrid clouds. Containers, service meshes, microservices, immutable infrastructure, and declarative APIs exemplify this approach.

These techniques enable loosely coupled systems that are resilient, manageable, and observable. Combined with robust automation, they allow engineers to make high-impact changes frequently and predictably with minimal toil.

The Cloud Native Computing Foundation seeks to drive the adoption of this paradigm by fostering and sustaining an ecosystem of open-source, vendor-neutral projects. We democratize state-of-the-art patterns to make these innovations accessible for everyone.

CNCF has compiled an interactive cloud native landscape (https://landscape.cncf.io) that shows the full extent of today’s cloud native solutions. The CNCF landscape acts like a map to cloud native technologies and provides guidelines for building successful cloud native applications. Though the cloud native landscape provides a lot of information on how to build services, it can be challenging, especially for beginners, to work their way through it as it includes a number of services that are used together. In this book, we have chosen the best of the available technology while following the CNCF guidelines to better navigate the cloud native world.

### Adopting a Cloud Native Infrastructure with Azure

Now that you understand the origins of cloud computing, the challenges of operating in the cloud, and how cloud native offerings aimed to change the way services were being developed and delivered, let’s discuss how this book can help you moving forward.

If you take a look at the cloud native movement and begin plotting its story from the beginning, the first technology that comes up is Kubernetes, and you’ll find plenty of people who have adopted it. On the surface, you might observe that many companies have Kubernetes in the middle of their stack. This gives the impression that Kubernetes is an essential tool that can solve all your problems and magically make your environment self-healable and fault tolerant. This misconception has pushed a lot of people to explore Kubernetes as a silver bullet without fully understanding the deeper meaning. It’s important to understand the need for such solutions and gain insight to the full ecosystem while looking at the bigger picture.

In the chapters that follow, we provide guidance on how to build cloud native environments following the guidelines suggested by CNCF. We wrote this book as a primer for engineers and enthusiasts who are just starting out with cloud native transformations and want to explore the overall need for cloud native architectures. You’ll learn practical ways to create cloud native infrastructures over Microsoft Azure by navigating the CNCF landscape. You’ll also learn the principles of cloud native infrastructure from a beginner’s perspective, and you’ll deploy mature solutions over Azure.¹ As Azure is one of the key players in the public cloud ecosystem, it provides a mature understanding of the infrastructure stack. We will take you through cloud infrastructure basics and explain how to become cloud native through various technologies while highlighting why these technologies are necessary. We will give an extensive overview of cloud native technologies and Azure services through a practical application, and we will showcase the advantages of being cloud based (with Azure) versus cloud native. Our end goal is to give you a book that showcases all the major cloud native technologies and their importance so that you can understand the logical reasoning behind the advantages of using these technologies.

### Summary

In this introductory chapter, we laid the foundation of what cloud computing is and how cloud native technologies help improve cloud adoption. You learned how the cloud became popular and how cloud computing evolved from physical hardware to a serverless environment.





你还了解了云计算所面临的挑战，以及适应云原生技术日益增长的需求。我们解释了云原生意味着什么，以及本书其余部分将如何遵循在 Azure 上实现云原生的路径。我们希望你会发现前方的旅程有趣，并且这项工作能帮助你更有效地采用云原生技术。

1 Azure 还提供了大量的架构指南，展示了在 Azure 上构建服务的最佳实践：请参阅 https://docs.microsoft.com/en-us/azure/architecture 和 https://azure.microsoft.com/pt-br/blog/azure-application-architecture-guide。
