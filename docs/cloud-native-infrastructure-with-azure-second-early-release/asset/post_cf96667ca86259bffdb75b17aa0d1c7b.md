
In this chapter, we will dive into the major components and the underlying concepts of Kubernetes. Though this chapter doesn’t aim to teach Kubernetes in full, as there are already a wide array of books¹ that cover the topic at length, we want to build a strong foundation. This hands-on approach will help you better understand the nitty-gritty of the overall environment. Let’s start with a discussion of how the Kubernetes cluster works.

## Kubernetes Components

The Kubernetes cluster contains two types of node components:

*Control plane*

This is the governing component of the Kubernetes cluster. It ensures that a couple of important services (e.g., scheduling, starting up new Pods²) are always running. The core purpose of the control plane node is to make sure the cluster is always in a healthy and correct state.

*Worker nodes*

These are the compute instances that run your workload on your Kubernetes cluster which hosts all your containers.

Figure 4-1 depicts the high-level components of Kubernetes. The lines signify the connections, such as worker nodes accepting a connection talking to the load balancer, which distributes traffic.

![](img/8d47d1b5dbca55e784ae602145a894c7_109_0.png)

Figure 4-1. Kubernetes components

Now let’s take a detailed look at each component.

## Control Plane

The control plane is primarily responsible for making global decisions about the Kubernetes cluster, such as detecting cluster state, scheduling Pods on nodes, and managing the lifecycle of the Pods. The Kubernetes control plane has several components, which we describe in the following subsections.

### kube-apiserver (API server)

The API server is the frontend of the Kubernetes control plane and the only component with direct access to the entire Kubernetes cluster. As you saw in Figure 4-1, the API server serves as the central point for all the interactions between the worker nodes and the controller nodes. The services running in a Kubernetes cluster use the API server to communicate with one another. You can run multiple instances of the API server because it’s designed to be scalable horizontally.

### Kube scheduler

The Kube scheduler is responsible for determining which worker node will run a Pod (or basic unit of work; we will explain Pods in more detail later in this chapter). The Kube scheduler talks to the API server, which determines which worker nodes are available to run the scheduled Pod in the best possible way. The scheduler looks for newly created Pods which are yet to be assigned to a node, and then finds feasible nodes as potential candidates and scores each of them based on different factors, such as node resource capacity and hardware requirement, to ensure that the correct scheduling decision is made. The node with the highest score is chosen to run the Pod. The scheduler also notifies the API server about this decision in a process referred to as binding.

### Kube controller manager

Kubernetes has a core built-in feature that implements the self-healing capabilities in the cluster. This feature is called the Kube controller manager and it runs as a daemon. The controller manager executes a control loop called the reconciliation loop, which is a nonterminating loop that is responsible for the following:

- Determining whether a node has gone down, and if so, taking action. This is done by the Node controller.
- Maintaining the correct number of Pods. This is done by the Replication controller.
- Joining the endpoint objects (i.e., services and Pods). This is done by the Endpoint controller.
- Ensuring that default accounts and endpoints are created for new namespaces. This is done by the Service Account and Token controllers.

The reconciliation loop is the driving force behind the self-healing capability of Kubernetes. Kubernetes determines the state of the cluster and its objects by continuously running the following steps in a loop:

- Fetch the user-declared state (the desired state).
- Observe the state of the cluster.
- Compare the observed and desired states to find differences.
- Take actions based on the observed state.

### Etcd

Kubernetes uses etcd as a datastore. Etcd is a key-value store that is responsible for persisting all the Kubernetes objects. It was originally created by the CoreOS team and is now managed by CNCF. It is usually spun up in a highly available setup, and the etcd nodes are hosted on separate instances.

## Worker Nodes

A Kubernetes cluster contains a set of worker machines called worker nodes that run the containerized applications. The control plane manages the worker nodes and the Pods in the cluster. Some components run on all Kubernetes worker nodes; they are discussed in the following subsections.

### Kubelet

Kubelet is the daemon agent that runs on every node to ensure that containers are always in a running state in a Pod and are healthy. Kubelet reports to the API server about the currently available resources (CPU, memory, disk) on the worker nodes so that the API server can use the controller manager to observe the Pods’ state. Since kubelet is the agent that runs on the worker nodes, the worker nodes handle basic housekeeping tasks such as restarting containers if required and consistently conducting health checks.

### Kube-proxy

Kube-proxy is the networking component that runs on each node. Kube-proxy watches all Kubernetes services³ in the cluster and ensures that when a request to a particular service is made, it gets routed to the particular virtual IP endpoint. Kube-proxy is responsible for implementing a kind of virtual IP for services.

Now that you’ve got the basics of Kubernetes components under your belt, let’s dig a bit deeper and learn more about the Kubernetes API server.

## Kubernetes API Server Objects

The API server is responsible for all the communication inside and outside a Kubernetes cluster, and it exposes a RESTful HTTP API. The API server, at a fundamental level, allows you to query and manipulate Kubernetes objects. In simple terms, Kubernetes objects are stateful entities that represent the overall state of your cluster (Figure 4-2). To start working with these objects, we need to understand the fundamentals of each.

![](img/8d47d1b5dbca55e784ae602145a894c7_112_0.png)

Figure 4-2. API server interaction with cluster objects

### Pods

In Kubernetes, Pods are the smallest basic atomic unit. A Pod is a group of one or more containers that get deployed on the worker nodes. Kubernetes is responsible for managing the containers running inside a Pod. Containers inside a Pod will always end up on the same worker node and are tightly coupled. Since the containers inside a Pod are co-located, they run in the same context (i.e., they share the network and storage). This shared context is characteristic of Linux namespaces, cgroups, and any other aspects which maintain isolation (as we explained in Chapter 3). Pods also get a unique IP address.

In typical scenarios, a single container is run inside a Pod, but in some instances, multiple containers need to work together in a Pod. This latter setup is usually referred to as a sidecar container. One of the most common examples of running a sidecar container is running a logging container for your application that will ship your logs to external storage, such as an ELK (Elasticsearch, Logstash, and Kibana) server, in case your application Pod crashes or a Pod is deleted. Pods are also smart in that if a process in a container dies, Kubernetes will instantly restart it based on the health checks defined at the application level.

Another characteristic of Pods is that they allow horizontal scaling by replication implemented through ReplicaSets. This means that if you want your application to scale horizontally, you should create more Pods by using ReplicaSets.

Pods are also ephemeral in nature, which means that if a Pod gets killed, it will be moved and restarted on a different host. This is also accomplished by using ReplicaSets.

### ReplicaSets

Reliability is a chief characteristic of Kubernetes, and since no one would be running a single instance of a Pod, redundancy becomes important. A ReplicaSet is a Kubernetes object that ensures that a stable set of replica Pods are running to maintain a self-healing cluster. All of this is achieved by the reconciliation loop, which keeps running in the background to observe the overall state of the Kubernetes cluster. ReplicaSets use the reconciliation loop and ensure that if any of the Pods crash or get restarted, a new Pod will be started in order to maintain the desired state of replication. In general, you should not directly deal with ReplicaSets, but rather, use the Deployment object, which ensures zero-downtime updates to your application and has a declarative approach toward managing and operating Kubernetes.
