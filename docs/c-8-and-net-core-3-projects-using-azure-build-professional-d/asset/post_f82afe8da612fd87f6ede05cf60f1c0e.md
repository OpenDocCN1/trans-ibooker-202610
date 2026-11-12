
These days, this form of data exchange has changed into using message brokers: either maintained on-premises or in the cloud. I'll explain more about why this is the case a little later on.

At the time of writing, RabbitMQ and ActiveMQ are two well-known message brokers that are typically run on-premises. All of the big cloud providers offer a message broker of one form or another, including Azure, Google, and AWS (which use ActiveMQ).

Microservices are simply a type of distributed system, and they fit well with using containers. The idea is that you encapsulate the smallest level of functionality inside a self-contained process. The key thing here is that the process is self-contained – while there doesn't appear to be an official document that declares what microservices are, it would be wrong to call something a microservice if you simply had a process that had a direct dependency on another process.

It's worth considering the term dependency here. In my EDI example, clearly, a dependency exists in a logical process sense. The test here is whether you can turn one of these processes off and the other(s) continue (obviously with limited effect).

We've covered what creating a distributed system does for us, but let's consider the downsides to this approach, as there are many: you definitely should not create a distributed system unless you need one. I've compiled a (non-exhaustive) list of negatives here (each of these can be mitigated to an extent, but the purpose of this list is to illustrate that this isn't a silver bullet):

- **Data integrity**: In a system with a single data store, you can simply rely on the transactional model of your DBMS of choice to ensure that when you, for example, create a sales order, your stock is reduced. With a truly distributed system, these may be separate, independent services, and you can't execute these as a single transaction; the net effect of this is that you cannot guarantee that, at any specific given time, your data is correct.

- **System complexity**: Once you start creating many services, you'll find that you have a web of logical dependencies (similar to the physical dependencies you may have in a monolithic or n-tier system, as we mentioned previously).

- **Speed**: The fastest possible way to write a set of data to a database is to do so in a single transaction. Once you introduce a service bus into the middle of this process, you reduce its speed significantly.

- **Maintenance**: While one of the advantages of using this architectural style is that you can independently update individual components, it is also one of the downsides. If you have 50 different components, then you need to track 50 different versions.

In this chapter, we will do the following:

- Create a very simple sales order processing system. There will be no frontend, but we will feed it a list of sales orders.

- Wrap our system in a Docker container and add it to a Kubernetes cluster hosted on Azure Kubernetes Service (AKS).

- Demonstrate that, even though we may kill an instance of that service, or it may crash, Kubernetes can self-heal and our orders will continue to be processed.

I've mentioned both Docker and Kubernetes, but what exactly are these technologies? It's easiest to start by exploring Docker.

## Exploring Docker

Docker is a container engine: what that means is that you can encapsulate all the dependencies of the process (the executable) into a single entity. I often think that it helps to analyze the journey that ended with (or has currently reached) containers.

At one time, in the Microsoft development world (before .NET), we would write an application that still had dependencies on DLLs. Windows provided a registry that told the program where the DLL would be. When it was time to ship the product, you would have to create an installer that added your dependencies into the registry; however, because all the DLLs were held centrally, you then had an issue where another program could depend on the same DLL, but a different version of that DLL. Needless to say, this caused untold issues, and we simply didn't know what code was running on-site and were never confident that anything would install. Registry cleaners and install tools sprung up left, right, and center, and finally Microsoft moved away from the registry model.

When .NET was released, there were two ways to ship your dependencies: you could install them to the Global Assembly Cache (GAC) or just put them in the same directory as the executable. The GAC had its own issues (it was quite close to the registry model), but the package up dependencies that I needed and used worked quite well; however, what happens when you want to use a system dependency (.NET Framework, for example)? Well, again, Microsoft was quite good in that they would allow you to add this to an installer, but we are back to the principle that we're taking a piece of software and running it in a different environment than it was tested in. For example, what version of SQL Server did you write your data access against, and is that the version that's on the target machine?

What started to spring up was the principle of creating a virtual machine and then taking the entire virtual machine and simply copying it to the destination; that way, you can be absolutely sure what's running because it's essentially the same machine that the testing was done on. This was a good solution, but it (arguably) went too far. There are now a number of virtual machines with operating systems and licenses that need to be updated and maintained. What was needed was a container for the application where you could store the application and its dependencies, but the whole thing needed to be abstracted from the operating system.

That's essentially what Docker is – a container just like that.

## Kubernetes and orchestration

For all intents and purposes, Kubernetes came out around 2016. This was inspired by an internal Google product that they had been using for some years internally. To illustrate what Kubernetes allows us to do, I invite you to walk through a little scene.

You're in a queue at a supermarket. The tills are, unfortunately, very problematic and are constantly breaking down. There is a man that works for the supermarket who has the job of keeping the queue flowing: his name is Winston, and so each time a till breaks down, he redirects the next person in the queue to another till, fixes the till, and then reopens it.

Suddenly, the queue gets very big: 200,000 people all join the queue together. Winston has instructions that, should this happen, he should “scale out” and so he quickly builds another 500 tills to cope with the strain; it takes a few minutes, but the tills quickly come online and the queue flows again:

![](img/762492a8287f369cc1ac4621a212f193_364_0.png)

Kubernetes acts as our Winston: it can create new containers, upgrade existing containers with no downtime, and route traffic.

## Creating our microservice

We should consider what our microservice is, and what it is not: the most important thing here is that the service needs to be autonomous. If we create a service that needs other services in order to run, then it is not a microservice.

We shouldn't confuse this with creating a service that requires other services to function. For example, our sales order processing system must have sales orders, and these come from an external source. This is fine because we're not dependent on that external source: if we don't get a list of sales orders, then our service will still run; it just won't do anything.

Let's start by creating our new service!

Remember that what we mean here by *service* is a process that performs a task: it should not be confused with specific usages of the word service, such as a REST service. What we want is simply an independent process.

Bear in mind that, like Winston, our busy supermarket employee, Kubernetes can scale your application by *replicating* the container. You can only replicate a container or process if that process is written in such a way to allow it. What this means for us is that we need to communicate with our service using a mechanism that allows multiple processes to read from a single queue, which needs to be maintained.

If we return to our example involving Winston the super supermarket employee, each person in the queue approaches the till only when they are called; if they all try to approach the till together, then the till operator will not be able to serve them. Alternatively, imagine a situation where the till operator comes to the queue and scans the items of the person at the front, but then leaves that person there: the next available till operator may do the same. Winston creating new tills only works if there is a mechanism in place for the customers to approach the tills in an orderly fashion.

The same is true of our containers: if you design a microservice in the wrong way, then it simply can't be scaled.

## Queues

In order to get around this situation, we'll use Azure Service Bus to provide a queue for our feed into the system; because we don't need guaranteed delivery order, we can use storage queues rather than service bus queues.

It's worth bearing in mind that, in the case of sales order processing, the order that processes the messages is probably not too important; however, this is not always the case. You should carefully consider whether the order of delivery is critical in your own case.
