
## FinOps for the Container World

To support the adoption of microservice architectures, containers have gained popularity. Over the last few years, the number of container environments being run by organizations has rapidly increased. Managing a single running container instance is quite simple, but running hundreds, thousands, or tens of thousands of containers across many server instances becomes difficult. Thus, along came orchestration solutions like Kubernetes, which enable DevOps teams to maintain the configuration and orchestrate the deployment and management of fleets of containers.

Since containers and container orchestrators are becoming a popular choice for many teams, it's vital to understand the fundamental impact of these containerized workloads on FinOps practices.

The most important reason this impacts FinOps so much is that most container environments are complex shared environments. Shared resources like containers—which run on shared cloud compute instances—cause challenges with cost allocation, cost visibility, and resource optimization. In the containerized world, traditional FinOps cost allocation doesn't work the way it does with just virtual machines (VMs). You can't simply allocate the cost of a resource to a tag or label, because each cloud or on-premises resource may be running a constantly shifting set of multiple containers, each supporting a different application. They also may be attached to different cost centers around the organization. Not to worry, though. In this chapter we'll look at changes and adjustments that will allow you to successfully operate your FinOps practice in the container world.

## Containers 101

Let’s quickly run through the basics for anyone not familiar with containers. It will also be helpful to create a common understanding of these components throughout this chapter.

Containers are, quite simply, a way to package software. All of the requirements and settings are baked into a deployable image. Container orchestration tools like Kubernetes help engineers deploy containers to servers in a manageable and maintainable way.

There are a few key terms we will use throughout this chapter:

*Image*
A snapshot of a container with the software that needs to be run.

*Container*
An instance of a container image; you can have multiple copies of the same image running at the same time.

*Server instance/node*
A server (e.g., EC2 instance, VM, physical server).

*Pod*
This is a Kubernetes concept. A pod consists of at least one container but can also contain a group of containers and treats them as a single block of resources that can be scheduled and scaled on the cluster.

*Container orchestration*
An orchestrator manages the cluster of server instances and also maintains the lifecycle of containers/pods. Part of the container orchestrator is the scheduler, which schedules a container/pod to run on a server instance. Examples include Kubernetes or AWS Elastic Container Service (ECS).

*Cluster*
A group of server instances, managed by container orchestration.

*Namespace*
Another Kubernetes concept, a namespace is a mechanism for isolating groups of resources within a single cluster where pods/containers can be deployed separately from other namespaces.

A Kubernetes cluster (see Figure 23-1) consists of a number of nodes (server instances) that run containers inside pods. Each pod can be made up of a varying number of containers. The nodes themselves support namespaces used to isolate groups of pods.

![](img/4d4736bc293fd892d1da81802d0f9520_385_0.png)

Figure 23-1. Basic view of a Kubernetes cluster

## The Move to Container Orchestration

Engineers make the decision to move to containers for a multitude of reasons including availability, reliability, scalability, and ease of management. Remember, FinOps is about making sure that you are doing what’s best for the business, continuously balancing between cost and technical needs. Cost is not the only driver for the migration to containers, but having the ability to cost out an application in a container model and a VM model (with all attendant costs and implications on both sides) is helpful context to provide the business.

When you change from a large number of individual cloud server instances to a cluster orchestration solution like Kubernetes, you usually end up with a smaller number of larger servers. By packing multiple container workloads into these larger servers (also known as bin packing), you can gain efficiency through better resource utilization by running many containers isolated on the same node.

An analogy might help you picture this idea: each server instance in a cluster is like a game of Tetris. Containers are the blocks of different shapes and sizes, and the container orchestrator is trying to place as many of these “blocks” (containers) into the servers as possible.

Platforms like Kubernetes allow you to set different resource guarantees, allowing you to pack more containers onto a server instance, which we’ll cover in more depth later in the chapter.

You lose visibility into the individual costs of each container when running multiple containers on a single server instance, since the cloud service provider will provide billing data only for the usage of the underlying server instance. When a server instance is running multiple containers, you need more information in order to divide up the usage costs.

In other words, each cloud resource that's shared by multiple workloads needs to have supplemental data, so it can show how much of the cloud resource was used by each workload. Containers do not consume servers equally. Some containers can be configured to use more of a server instance than others. Some containers run for weeks or months. But research shows that most containers often run for short periods, less than 12 hours on average, and some might only run for a few seconds. With different sized containers coming and going at different rates, taking infrequent snapshots of your cluster usage won't provide sampling sufficient for the detailed cost allocation you need. Detailed data that tracks all the containers as they come and go will help you identify how the individual server costs are to be divided, and how much of the server was used by a container for service A versus a container for service B. You will see what this data looks like in “Container Proportions” on page 357.

One of the difficulties collecting this data, though, is the sheer volume of it. The cloud billing data files are already extremely large, but you may have many times the number of containers as VMs, and each one may shift around from resource to resource many times, creating a new line of data each time. It is a difficult challenge that requires some specialized tooling if you aim to solve it in a detailed and granular way.

FinOps practitioners need to prepare teams for the impact of shared resources on cost visibility and cost allocation processes. It’s vital to have a clear understanding of the requirements needed to maintain successful FinOps practices in the container world, and then to be sure they get implemented by the DevOps team.

## The Container FinOps Lifecycle

When you look at the challenges that containerization poses for FinOps—cost visibility, cost showback/chargeback, and cost optimization—you quickly realize that you’re encountering the same difficulties you faced as you moved into the cloud. Containers represent another layer of virtualization, also called containerization, on top of cloud virtualization. While this might feel like you have moved backward, keep in mind that you have already accumulated all the knowledge, processes, and practices necessary to solve these challenges. You simply need to base your approach on how you solved for the cloud in the first place.

Therefore, you can divide the containerization FinOps challenges into the same inform, optimize, and operate lifecycle that you applied to the broader cloud FinOps.

## Container Inform Phase

Your first focus should be to generate reporting that enables you to determine the cost of individual containers on the clusters that teams are operating. In other words, you need to build visibility into your container spending.

## Cost Allocation

You will be charged by a cloud service provider for every server instance that makes up a cluster. When containers are deployed into a cluster, they consume some amount of the cluster's resource capacity, such as CPU, memory, and storage. Even if the processes inside the container don't consume all of what was provisioned for that container, in some cases it blocks other containers from using this capacity, and therefore you have to pay for it. It's just like when you provision a server with your cloud service provider. You pay for that server resource, whether you use it or not. The network traffic costs from and to your services is a challenge on its own and should be included as well. Network costs attributed to a single node or server instance may be supporting a number of containers, and the related costs will need to be split up by each individual container's usage. Network becomes a form of shared costs that require an equitable distribution model, ideally by evaluating the actual network traffic volume of each container and allocating the network costs based on these amounts.

To allocate the individual costs of a container that runs on a cluster, you'll need some way to determine how much of the underlying server the individual container consumed. We'll cover this more in the next section.

You also need to take into account the satellite costs of a running cluster. Management nodes, data stores used to track cluster states, software licensing, backups, and disaster recovery costs are part of your cost allocation strategies. These overhead costs are all part of running clusters and must be taken into account in your cost allocation strategy.

## Container Proportions

The biggest issue with governing clusters is that you're sharing underlying resources, so you can't get complete visibility into allocation by using your cloud bill alone. Remember, a cloud service provider can only observe metrics provided by the VM hypervisors, not operating systems or processes, unless you're using their managed service offerings. This lack of visibility means providers are unable to track your containers and how much of the server they use. So for your FinOps team to divide
