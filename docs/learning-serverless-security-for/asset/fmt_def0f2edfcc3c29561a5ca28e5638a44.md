
Unless my team had several other applications and scripts that would utilize this custom automated solution, going through the trouble of setting up this infrastructure for a one-off script didn’t make sense. With a serverless function service, I figured that I could focus more on the custom application logic that would run inside the cloud function resource. Reinventing the wheel wasn’t worthwhile since services like `AWS Lambda` already provided the necessary infrastructure and capabilities for these types of scenarios.

Therefore, I proceeded with leveraging various AWS services to build and deploy the serverless application. It took me less than a day to completely implement the architecture shown in [Figure 1-4](#ch01.html#fig_my_first_serverless_project).

![](assets/lsse_0104.png)

This serverless application used an event scheduler (`Amazon CloudWatch Events`, now known as `Amazon EventBridge`) that sent an event to a serverless function once every 24 hours. The serverless function ran the custom code that sent the cost control email reminder via an email service (`Amazon SES`) to each developer. This custom code had no more than 50 lines of code and ran for at most approximately 30 seconds per day.

You might be surprised that this specific serverless implementation *never* had a downtime in its lifetime. The proper use of serverless computing and managed services helped ensure that there was no *single point of failure* (`SPOF`) in the architecture. This eliminated the risk of downtime and allowed the system to operate smoothly without manual intervention. While not all serverless systems will have the same level of uptime and reliability, the benefits of a well-architected serverless solution are evident in this project.

### Note

From a security standpoint, there were also no publicly exposed API endpoints as the serverless function resource was invoked directly by the event scheduler service. The only authorized component that could invoke the cloud function was the event scheduler service. This setup minimized the attack surface and reduced the risk of security breaches by preventing any unauthorized interactions with the cloud function.

## Exploring Definitions and Serverless Use Cases

Now that you have a rough idea of what a real serverless implementation looks like, here’s an in-depth definition of serverless computing as well as a closer look at some of the related concepts that you’ll encounter throughout the rest of the book. While the term *serverless* has various definitions and interpretations along with concepts commonly associated with it, this book uses the following definitions:

### Serverless

Serverless, or serverless computing, is an *operational model* that allows developers to build and run applications without managing or scaling the underlying infrastructure. Serverless abstracts away the responsibilities of server management, scaling, patching, and resource provisioning.^[6](#ch01.html#id306) This model enables developers to focus on delivering business value by using a variety of interconnected tools and services that enable automatic scaling of applications based on demand. Cloud platforms like `AWS`, `Azure`, and `Google Cloud` define and implement serverless computing across various services used in serverless architectures,^[7](#ch01.html#id307) with slight variations in terminology, pricing models, and resource management practices.^[8](#ch01.html#id308)

### Function as a service (FaaS)

A serverless compute service that allows developers to run individual functions in response to events, without the need to provision or manage servers. Examples of FaaS services include `AWS Lambda`, `Azure Functions`, and `Google Cloud Run functions`.

### Note

Is *serverless* equivalent to the usage of serverless function services and resources? As you will see later in [“Myth 2: Serverless Is Equivalent to FaaS”](#ch01.html#ch01-myth-02), serverless is not equivalent to FaaS.

### Managed cloud service

A service that offloads part or all of the management and maintenance of IT resources to a cloud provider. Cloud resources created using a managed cloud service, while not completely immune to disruptions and downtime, are generally more fault-tolerant compared to resources set up manually by in-house IT staff.

### Pay-as-you-go (PAYG) model

A flexible pricing model that charges customers based on their actual usage of a service,^[9](#ch01.html#id313) rather than charging an up-front fixed fee or requiring a long-term contract.

### Event-driven architecture

A software architecture pattern that uses events to trigger and facilitate communication among decoupled components. In this architecture, components react to state changes or events emitted by other components of the system.

### Container

A portable, stand-alone package of software that includes all the application code, dependencies, configuration, and runtime needed to run in an isolated environment.

### Microservice

A small, decoupled, modular service designed to carry out a specific business function or execute a particular set of tasks within a larger application.

### Single point of failure (SPOF)

A component whose failure would cause a complete service disruption to the entire architecture or system.

With the definitions out of the way, you might want to know how to identify when serverless is the right choice. Understanding the context and specific needs of your application can help determine whether serverless is the optimal path forward.

Utilizing serverless solutions and strategies is generally a good idea in the following scenarios:

**Event-driven workloads**

Cloud resources are dynamically allocated and scaled based on incoming events, allowing the system to respond and scale in real time.

**Short-lived and bursty workloads**

The application may experience unpredictable and sudden spikes in demand.

**Rapid prototyping and development**

Updates can be made to specific components of a system without disrupting the entire application.

**Modular and decoupled architectures**

Modules and components of an application can be developed, tested, and deployed independently.

**Cost-conscious applications**

The right amount of resources needs to be provisioned at any given time.

**Resilient and fault-tolerant applications**

The impact of failures on the overall application is minimized and managed automatically to maintain high availability.

Not all projects need to perfectly fit these scenarios in order to use a serverless implementation. In some cases, you may consider using a serverless architecture for a project that doesn’t align fully with these use cases. For example, a project with a limited budget could benefit from a serverless implementation, especially when it is expected to deal with low usage or traffic. A team with limited infrastructure management experience can also utilize serverless architectures for projects to avoid the complexities of provisioning and maintaining servers.

My first serverless project aligned well with these use cases. The seamless integration of the serverless function service with the other cloud services used in the application reduced the overall complexity of the architecture. I was able to focus more on the application’s core functionality rather than spending time worrying about server maintenance and uptime.

When it comes to cost, not all serverless architectures will have the same level of cost efficiency. That’s because the overall cost depends on factors such as these:

* Design and complexity of the application
* Data storage needs
* Resource consumption patterns
* Network data transfer patterns
* Service configurations
