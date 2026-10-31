
# GitOps Overview
With `GitOps`, your desired state for your infrastructure is defined in code, and stored in source control. When changes are made to this desired state, some tooling ensures that this updated desired state is applied to the running system. The idea is giving developers a simplified workflow for working with their applications.

If you’ve used infrastructure configuration tools like `Chef` or `Puppet`, this model is familiar for managing infrastructure. When using `Chef Server` or `Puppet Master`, you had a centralized system capable of pushing out changes dynamically when they were made. The shift with `GitOps` is that this tooling is making use of capabilities inside `Kubernetes` to help manage applications, rather than just infrastructure.

Tools like `Flux`^([3](#ch07.xhtml#idm46534869771928)) are making embracing these ideas much easier. It’s worth noting of course that while tools can make it easier for you to change the way you work, they can’t force you into adopting new working approaches. Put differently, just because you have `Flux` (or another `GitOps` tool), it doesn’t mean you’re embracing the ideas of desired state management or infrastructure as code.

If you’re in the world of `Kubernetes`, adopting a tool like `Flux` and the workflows they promote may well speed up the introduction of concepts like desired state management and infrastructure as code. Just make sure you don’t lose sight of the goals of the underlying concepts and get blinded by all the new technology in this space!

# Deployment Options
When it comes to the approaches and tooling we can use for our microservice workloads, we have *loads* of options. But, we should look at these options in terms of the principles we outlined above. We want our microservices to run in an isolated fashion, and ideally be deployed in a way that avoids downtime. We want the tooling we pick to allow us to embrace a culture of automation, define our infrastructure and application configuration in code, and ideally also manage desired state for us.

Let’s briefly summarize the various deployment options we’re going to look at, before looking at how well they deliver on these ideas.

**Physical Machine**
A microservice instance is deployed directly on to a physical machine, with no virtualisation.

**Virtual Machine**
A microservice instance is deployed on to a virtual machine.

**Container**
A microservice instance runs as a separate container on a virtual or physical machine. That container runtime may be managed by a container orchestration tool like `Kubernetes`.

**Application Container**
A microservice instance is run inside an application container which manages other application instances, typically on the same runtime.

**Platform As A Service (`PAAS`)**
A more highly-abstracted platform is used to deploy microservice instances, often abstracting away all concepts of the underlying servers used to run your microservices. Examples include `Heroku`, `Google App Engine` or `AWS Beanstalk`.

**Function As A Service (`FAAS`)**
A microservice instance is deployed as one or more functions, which are run and managed by an underlying platform like `AWS Lambda` or `Azure Cloud Functions`. Arguably, `FAAS` is a specific type of `PAAS`, but it deserves exploration in its own right given the recent popularity of the idea and the questions it raises about the mapping from a microservice to a deployed artifact.

## Physical Machines
An increasingly rare option, you may find yourself deploying microservices *directly* on to physical machines. By “directly”, I mean that there are no layers of virtualization or containerization between you and the underlying hardware. This has become less and less common for a few reasons. Firstly, deploying directly on to physical hardware can lead to lower utilization across your estate. If I have a single instance of a microservice running on a physical machine, and I only use half the CPU, memory, or IO provided by the hardware, then the remaining resources are wasted. This problem has lead to the virtualization of most computing infrastructure, allowing you to co-exist multiple virtual machines on the same physical machine. It gives you much higher utilization of your infrastructure, which has some obvious benefits in terms of cost effectiveness.

If you have direct access to physical hardware without the option for virtualization, the temptation is to then pack multiple microservices on the same machine - this of course violates the principle we talked about regarding having an *isolated execution environment* for your services. You could use tools like `Puppet` or `Chef` to configure the machine - helping implement infrastructure as code. The problem is that if you are only working at the level of a single physical machine, implementing concepts like desired state management, zero-downtime deployment etc require us to work at a higher-level of abstraction, using some sort of management layer on top. These types of systems are more commonly used in conjunction with virtual machines, something we’ll explore more in a moment.

In general, directly deploying microservices onto physical machines is something I almost never see nowadays, and you’ll likely need to have some very specific requirements (or constraints) in your situation to justify this over the increased flexibility that either virtualization or containerization may bring.

## Virtual Machines
Virtualization has transformed data centres, by allowing us to chunk up existing physical machines into smaller, virtual machines. Traditional virtualization like `VMWare` or that used by the main cloud providers managed virtual machine infrastructure (such as `AWS`’s `EC2` service) has yielded huge benefits in increasing the utilization of computing infrastructure, whilst at the same time reducing the overhead of host management.

Fundamentally, virtualization allows you to split up an underlying machine into multiple smaller “virtual” machines which act just like normal servers to the software running inside the virtual machines. You can assign portions of the underlying CPU, memory, IO and storage capability to each virtual machine, which in our context allow you to cram many more isolated execution environments for your microservice instances on to a single physical machine.

As each virtual machine gives the software running on its own operating system and set of resources, we have a very good degree of isolation between instances. Each microservice instance can fully configure the operating system in the VM to their own local needs. We still have the issue though that that if the underlying hardware running these virtual machines fails, then we can lose multiple microservice instances. There are ways to help solve that particular problem, including things like desired state management, which we discussed earlier.

### Cost Of Virtualization
As you pack more and more virtual machines onto the same underlying hardware, you will find that you get diminishing returns in terms of the computing resources available to the VMs themselves. Why is this?

Think of our physical machine as a sock drawer. If we put lots of wooden dividers into our drawer, can we store more socks or fewer? The answer is fewer: the dividers themselves take up room too! Our drawer might be easier to deal with and organize, and perhaps we could decide to put T-shirts in one of the spaces now rather than just socks, but more dividers means less overall space.

In the world of virtualization, we have a similar overhead as our sock drawer dividers. To understand where this overhead comes from, let’s look at how most virtualization is done. [Figure 7-14](#ch07.xhtml#a61-virtualisation-comparison) shows a comparison of two types of virtualization. On the left, we see the various layers involved in what is called *type 2 virtualization*, and on the right we see *container-based virtualization*, which we’ll explore more shortly.
