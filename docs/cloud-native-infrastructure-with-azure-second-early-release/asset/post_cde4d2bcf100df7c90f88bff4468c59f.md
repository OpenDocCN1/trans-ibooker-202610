
## Deployments

Kubernetes is primarily a declarative-syntax-focused orchestrator. This means that in order to roll out new features, you need to tell Kubernetes what you need to do and it’s up to Kubernetes to figure out how to perform that operation in a safe manner. One of the objects that Kubernetes offers in order to make the release of new versions of applications smoother is `Deployment`. If you manually go on updating Pods, you will need to restart the Pods, which will cause downtime. While a ReplicaSet knows how to maintain the desired number of Pods, it won’t do a zero-downtime upgrade. Here is where the `Deployment` object comes into the picture, as it helps roll out changes to Pods with zero downtime by keeping a predefined number of Pods active all the time before a new updated Pod is rolled out.

## Services

To expose an application that is running inside a Pod, Kubernetes offers an object called `Service`. Since Kubernetes is a very dynamic system, it is necessary to ensure that applications are talking to correct backends. Pods are short-lived processes in the Kubernetes world, as they are frequently created or destroyed. Pods are coupled with a unique IP address, which means that if you rely on just the IP address of Pods, you will most likely end up with a broken service when a Pod dies, as the Pod will get a different IP address after restart, even though you have ReplicaSets running. The `Service` object offers an abstraction by defining a logical set of Pods and a policy by which to access them. Each service gets a stable IP address and a DNS name that can be used to access the Pods. You can declaratively define the services that front your Pods and use the `Label-Selector` to access the service.

## Namespaces

Since multiple teams and projects are deployed in a production-grade environment, it becomes necessary to organize the Kubernetes objects. In a simple sense, *namespaces* are virtual clusters separated by logical partitioning; that is, you can group your resources, such as deployments, Pods, and so on, based on logical partitions. Some people like to think of namespaces as directories to separate names. Every object in your cluster has a name that is unique to that particular type of resource, and similarly every object has a UID which is unique across the whole cluster.
Namespaces also allow you to divide the cluster resources among multiple users by setting *resource quotas*.

## Labels and Selectors

As you start using Kubernetes and creating objects, you’ll realize the need to identify or mark your Kubernetes resources in order to group them into logical entities. Kubernetes offers *labels* to identify metadata for objects, which easily allows you to group and operate the resources. Labels are key-value pairs that can be attached directly to objects like Pods, namespaces, daemon sets, and so on. You can add labels at any time and modify them as you may like. To find or identify your Kubernetes resources, you can query the labels using *label selectors*. For example, a label for a type of application tier can be:

```
"tier" : "frontend", "tier" : "backend", "tier" : "midtier"
```

## Annotations

*Annotations* are also key-value pairs, but unlike labels, which are used to identify objects, annotations are used to hold nonidentifying information about the object itself. For example, build, release, or image information such as timestamps, release IDs, the Git branch, PR numbers, image hashes, and the registry address can be recorded in an annotation.

## Ingress Controller

For your services to receive traffic from the internet, you need to expose HTTP and HTTPS endpoints from the outside to Kubernetes services running on Pods. An *ingress* allows you to expose your services running inside the cluster to the outside world by offering load balancing with Secure Sockets Layer/Transport Layer Security (SSL/TLS) terminations using name-based virtual hosting. To support an ingress, you should first choose an *ingress controller*, which is similar to a reverse proxy, to accept incoming connections for HTTP and HTTPS.

## StatefulSets

To manage and scale your stateful workloads on Kubernetes, you need to ensure that the Pods are stable (i.e., a stable network and stable storage). *StatefulSets* ensure the ordering of the Pods and maintain their uniqueness (unlike ReplicaSets). A StatefulSet is a controller that helps you deploy groups of Pods that remain resilient to restarts and reschedules. Each Pod in a StatefulSet has unique naming conventions whose ordinal value starts at 0 and has a stable network ID associated with it (unlike a ReplicaSet, in which the naming convention is random in nature).

## DaemonSets

In regular environments, we run a number of daemon services and agents on the host, including logging agents and monitoring agents. Kubernetes allows you to install such agents by running a copy of a Pod across a set of nodes in a cluster using *DaemonSets*. Just like ReplicaSets, DaemonSets are long-running processes which ensure that the desired state and observed state remain the same. Deleting a DaemonSet also deletes the Pods that it previously created.

## Jobs

Jobs in the Kubernetes world are short-lived entities, which can be small tasks such as running a standalone script, for example. Jobs eventually create Pods. Jobs are run until they are successfully terminated, which is the major difference between a Pod that controls a job and a regular Pod that will keep getting restarted and rescheduled if terminated. If a Job Pod fails before completion, the controller will create a new Pod based on the template.

## NOTE

This chapter is a crash course in Kubernetes and just scratches the surface of container orchestration. Other resources are available that can help you learn more about Kubernetes. Some of the ones we recommend are:

- *Managing Kubernetes* by Brendan Burns and Craig Tracey (O’Reilly, 2018)
- *Kubernetes: Up and Running, 2nd Edition* by Brendan Burns, Joe Beda, and Kelsey Hightower (O’Reilly 2019)
- “Introduction to Kubernetes,” a free course from LinuxFoundationX: https://www.edx.org/course/introduction-to-kubernetes
- *Cloud Native DevOps with Kubernetes* by John Arundel and Justin Domingus (O’Reilly, 2019)

Now that we have covered some basic terminology in the Kubernetes world, let’s take a look at the operational details for managing the cluster.

## Observe, Operate, and Manage Kubernetes Clusters with kubectl

One of the more common ways to interact with a container orchestrator is by using either a command-line tool or a graphical tool. In Kubernetes you can interact with the cluster in both ways, but the preferred way is the command line. Kubernetes offers kubectl as the CLI. Kubectl is widely used to administer the cluster. You can consider kubectl to be a Swiss Army knife of various Kubernetes functions that enable you to deploy and manage applications. If you are an administrator for your Kubernetes cluster, you would be using the kubectl command extensively to manage the cluster. Kubectl offers a variety of commands, including:

- Resource configuration commands which are declarative in nature
- Debugging commands for getting information about workloads
- Debugging commands for manipulating and interacting with Pods
- General cluster management commands

In this section, we will explore Kubernetes in depth by focusing on basic cluster commands for managing Kubernetes clusters, Pods, and other objects with the help of kubectl.

## General Cluster Information and Commands

The first step to interacting with the Kubernetes cluster is to learn how to gain insights on the cluster, infrastructure, and Kubernetes components that you’ll be working with. To see the worker nodes that run the workloads in your cluster, issue the following kubectl command:

```
$ ~ kubectl get nodes
NAME      STATUS   ROLES     AGE   VERSION
worker0   Ready    <none>    11d   v1.17.3
worker1   Ready    <none>    11d   v1.17.3
worker2   Ready    <none>    11d   v1.17.3
```

This will list your node resources and their status, along with version information. You can gain a bit more information by using the -o wide flag in the get nodes command as follows:

```
$ ~ kubectl get nodes -o wide
NAME      STATUS   ROLES     AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE          KERNEL-VERSION     CONTAINER-RUNTIME
worker0   Ready    <none>    11d   v1.17.3   10.240.0.20   <none>        Ubuntu 16.04.7 LTS   4.15.0-1092-azure   containerd://1.3.2
worker1   Ready    <none>    11d   v1.17.3   10.240.0.21   <none>        Ubuntu 16.04.7 LTS   4.15.0-1092-azure   containerd://1.3.2
worker2   Ready    <none>    11d   v1.17.3   10.240.0.22   <none>        Ubuntu 16.04.7 LTS   4.15.0-1092-azure   containerd://1.3.2
```

To get even more details specific to a resource or worker, you can use the describe command as follows:
