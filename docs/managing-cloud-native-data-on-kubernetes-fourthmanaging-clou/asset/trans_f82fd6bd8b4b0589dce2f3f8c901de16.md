

Kubernetes 仍显示出它诞生于无状态世界的痕迹，但我对 Kubernetes 上有状态工作负载的未来感到兴奋。我们正处于非常“跨越鸿沟”的时刻，并且有状态工作负载的拐点才刚刚到来。随着更先进的 Operator 的出现，我们将不再在各自为政中工作，反复解决相同的问题。然后我们就可以利用集体的才华和技能来解决更大、更高层次的问题。

## Summary

在本章中，你了解了扩展 Kubernetes 控制平面的几种方式，尤其是 Operator 和自定义资源。Operator 模式提供了关键突破，使我们能够通过自动化简化 Kubernetes 中的数据库操作。虽然你绝对应该使用 Operator 在 Kubernetes 中运行分布式数据库，但在开始编写自己的 Operator 之前请仔细思考。如果构建 Operator 对你来说是正确的选择，有大量资源和框架可以帮助你。Kubernetes 本身在使编写 Operator 变得更容易方面肯定还有改进的空间，正如你从本章采访的专家那里学到的那样。

虽然我们在过去几章中主要关注在 Kubernetes 上运行数据库，让我们看看如何将迄今为止学到的知识应用到另一种类型的数据基础设施：流式传输和消息传递。

## Chapter 6. Integrating Data Infrastructure in a Kubernetes Stack

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 6th chapter of the final book. Please note that this book’s code examples are available at https://github.com/data-on-k8s-book.

In this book we are pointing toward a future of modern, cloud native applications that run on Kubernetes. We’ve noted that historically, data has been one of the hardest parts of making this a reality. In previous chapters, we’ve introduced the primitives Kubernetes provides for managing compute, network, and storage resources, and considered how databases can be deployed on Kubernetes using these resources. We’ve also examined the automation of infrastructure using controllers and the operator pattern.

Now it’s time to expand our focus to consider how data infrastructure fits in your overall application architecture in Kubernetes. In this chapter we’ll explore how to assemble the building blocks discussed in previous chapters into integrated data infrastructure stacks that are easy to deploy and tailor to the unique needs of each application. These stacks represent a step toward the vision of the virtual data center we introduced in Chapter 1. To learn the considerations involved in building and using these larger assemblies, let’s

take an in-depth look at K8ssandra, an open-source project that provides an integrated data stack based on Apache Cassandra.

## K8ssandra: Production-ready Cassandra on Kubernetes

To set the context, let’s consider some of the practical challenges of moving application workloads into Kubernetes. As organizations have begun to migrate existing applications to Kubernetes and create new cloud native applications in Kubernetes, modernizing the data tier is a step that is often deferred. Whatever the causes of these delays - a belief that Kubernetes is not ready for stateful workloads, a lack of development resources, or other factors - the result has been mismatched architectures in which applications are running in Kubernetes with databases and other data infrastructure running externally. This leads to a division of focus for developers and SREs that can limit productivity. It’s also common to see distinct toolsets for monitoring applications and database infrastructure, which increases cloud computing costs.

This adoption challenge became evident in the Cassandra community. Despite the growing collaboration and consensus around building a single Cassandra operator discussed in Chapter 5, developers were still confronted with some key questions about how the database and operator would fit in the larger application context:

- How can we have an integrated view of the health of our entire stack, including both applications and data?
- How can we tailor the automation of installation, upgrades, and other operational tasks in a Kubernetes-native way that fits the way we manage our data centers?

To help address these questions, John Sanda and a team of engineers at DataStax launched an open-source project called K8ssandra with the goal of providing a production-ready deployment of Cassandra that embodies

best practices for running Cassandra in Kubernetes. K8ssandra provides custom resources that help manage tasks including cluster deployment, upgrades, scaling up and down, data backup and restore, and more. You can read more about the motivations for the project in the article [Why K8ssandra?](Why K8ssandra?)

## K8ssandra Architecture

K8ssandra is deployed in units known as clusters, which is similar terminology to that used by Kubernetes and Cassandra. A K8ssandra cluster includes a Cassandra cluster along with additional components depicted in Figure 6-1 to provide a full data management ecosystem. Let’s consider these in roughly clockwise order starting from the top center.

- *Cass Operator* is a Kubernetes operator first introduced in Chapter 5. manages the lifecycle of Cassandra nodes on Kubernetes, including provisioning new nodes and storage, and scaling up and down.
- *Cassandra Reaper* manages the details of repairing Cassandra nodes in order to maintain high data consistency.
- *Cassandra Medusa* provides backup and restore for data stored in Cassandra
- *Prometheus and Grafana* are used for the collection and visualization of metrics
- *Stargate* is a data gateway that provides API access to client applications as an alternative to CQL.
- *K8ssandra Operator* orchestrates all of the other components, including multi-cluster support for managing Cassandra clusters that span multiple Kubernetes clusters.

![](img/cb55ccac39ceccbc2806f8db5c8fad69_192_0.png)

Figure 6-1. K8ssandra Architecture

In the following sections, we’ll take a look at each component of the K8ssandra project to understand the role that it plays within the architecture and its relationship to other components.

## Installing the K8ssandra Operator

Let’s dive in with some hands-on experience of installing K8ssandra. To get a basic installation of K8ssandra running that fully demonstrates the power of the operator, you’ll need a Kubernetes cluster with several worker nodes. To make the deployment simpler, the K8ssandra team has provided scripts to automate the process of creating Kubernetes clusters and then deploying the operator to these clusters. These scripts use Kind clusters for simplicity, so you’ll want to make sure you have this installed before starting. Instructions are also available for installing on various clouds on the K8ssandra website.

### K8SSANDRA 2.0 RELEASE STATUS

This chapter focuses on the K8ssandra 2.0 release, including the K8ssandra Operator. At the time of writing, K8ssandra 2.0 is still in Beta status. The instructions we provide here are based on an installation guide in the K8ssandra Operator repository. As K8ssandra 2.0 moves toward a full general availability (GA) release, the instructions on the Getting Started section of the K8ssandra website will be updated to reference the new version.

First, start by cloning the K8ssandra operator repository from GitHub:

```
git clone https://github.com/k8ssandra/k8ssandra-operator.git
```

Next, you’ll want to use the provided Makefile to create a Kubernetes cluster and deploy the K8ssandra Operator into it. (This assumes you have make installed)

```
cd k8ssandra-operator
make single-up
```

If you examine the Makefile, you’ll notice the operator is installed using Kustomize, which we discussed in Chapter 4. The target you just executed creates a Kind cluster with four worker nodes and changes your current context to point to that cluster, as you can see by running the following:

```
% kubectl config current-context
kind-k8ssandra-0
% kubectl get nodes
NAME                          STATUS   ROLES                       AGE
VERSION
k8ssandra-0-control-plane     Ready    control-plane,master        6m45s
v1.22.4
k8ssandra-0-worker            Ready    <none>                      6m13s
v1.22.4
k8ssandra-0-worker2           Ready    <none>                      6m13s
v1.22.4
k8ssandra-0-worker3           Ready    <none>                      6m13s
v1.22.4
k8ssandra-0-worker4           Ready    <none>                      6m13s
v1.22.4
```
