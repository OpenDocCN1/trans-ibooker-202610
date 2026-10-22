
## MongoDB Kubernetes Operator

MongoDB is the most popular document database, beloved by developers for its ease of use. The MongoDB Community Operator provides basic support for creating and managing MongoDB Replica Sets, scaling up and down, and upgrades. This operator is available on GitHub but not yet listed on Operator Hub, possibly because MongoDB also offers a separate operator for its enterprise version.

## Redis Operator

Redis is an in-memory key-value store that has a broad set of use cases. Application developers typically use Redis as an adjunct to other data infrastructure when ultra-low latency is required. It excels at things such as caching, counting and shared data structures. The Redis Operator covers the basic install and upgrade but also manages harder operations such as cluster failover and recovery.

As you can see, operators are available for many popular open source databases, although it’s unfortunate that some vendors have tended to think of Kubernetes operators primarily as a feature differentiator for paid enterprise versions.

## Building Operators

While there is broad consensus in the Kubernetes community that you should use operators for distributed data infrastructure whenever possible, there are a variety of opinions about who exactly should be building operators. If you don’t happen to work for a data infrastructure vendor, this can be a challenging question. The article “When to Use, and When to Avoid, the Operator Pattern” provides some excellent questions to consider, which we’ll summarize here:

- What is the scale of the deployment? If you’re only deploying a single instance of the database application, building and maintaining an operator might not be cost effective.
- Do you have the expertise in the database? The best operators tend to be built by companies that are running databases at scale in production, including vendors that are providing DBaaS solutions.
- Do you need higher levels of application awareness and automation, or would deployment with a Helm chart and standard Kubernetes resources be sufficient?
- Are you trying to make the operator manage resources that are external to Kubernetes? Consider a solution that runs closer to the resources being managed with an API you can access from your Kubernetes application.
- Have you considered security implications? Since operators are extensions of the Kubernetes control plane, you’ll want to carefully manage what resources your operator can access.

If you decide to write an operator, there are several great tools and resources available:

## OperatorSDK

The Operator Framework includes Operator SDK, a software development kit containing tools to build, test, and package operators. Operator SDK uses templates to auto-generate new operator projects and provides APIs and abstractions to simplify common aspects of building operators, especially interactions with the Kubernetes API. The SDK supports the creation of operators using Go, Ansible or Helm.

## Kubebuilder

Kubebuilder is a toolkit for building operators managed by the Kubernetes API Machinery SIG. Similar to Operator SDK, Kubebuilder provides tools for project generation, testing, and publishing controllers and operators. Both Kubebuilder and OperatorSDK are built on The Kubernetes controller-runtime, a set of Go libraries for building controllers. The blog post Kubebuilder vs Operator SDK provides a concise summary of the differences between these toolkits.

## Kudo

The Kubernetes Universal Declarative Operator, or Kudo for short, takes a declarative approach. Kudo is an operator that allows you to create operators declaratively using yaml files. This is an attractive approach for some developers as it eliminates the need to write Go. The blog post How to deploy your first app with Kudo operator on K8S provides a helpful introduction to using Kudo and discusses some of the pros and cons of the declarative approach.

Finally, the O’Reilly books Kubernetes Operators and Programming Kubernetes are great resources for understanding the operator ecosystem and getting into the details of writing operators and controllers in Go.

## CAN ONE OPERATOR RULE THEM ALL?

With Umair Mufti, Product Manager, PortWorx

The number of Kubernetes operators for databases has been continually growing. Database developers want their databases to run on Kubernetes, so to their credit, they are stepping up and developing operators to make it easy for others. This initiative is great, but the problem is that everyone is building operators their own way and solving similar problems with different implementations. As a result, there is no uniformity between operators for stateful workloads.

The question we have to reckon with is how specialized we expect end-users to be. Because of the popularity of cloud-native, microservice architectures, application developers now expect polyglot persistence: to run a relational database right next to a graph database or a key-value store. This gives cluster administrators the requirement to provide different types of databases while maintaining the operational simplicity of a single platform.

No Kubernetes admin wants to be maintaining 10 or 15 different operators on their cluster. The point of Kubernetes is the ease of operations when deploying applications, monitoring them on day two, and making lifecycle management simpler. As soon as you have the maintenance overhead of managing operator lifecycles, you are already broken. Multiply that 10 or 15 times over, and you are completely at odds with what Kubernetes is trying to provide. The only way out of this situation is to reduce the number of operators. Could there be a single operator for all our databases or stateful workloads? Let’s explore.

The operator pattern is simply a design pattern for running stateful workloads in Kubernetes, just as the Model View Controller framework is a pattern for user-facing applications. Various web frameworks such as Angular, Vue and React use the MVC pattern, but they all implement the pattern in different ways, and your code will vary based on the implementation you use. This is a familiar experience for developers using operators today. Each operator solves the problem of running a stateful workload in Kubernetes in a unique way, and it requires specialization to become proficient with each operator. The irony is that if you’re running Cassandra, Redis, or Postgres, a lot of the problems being addressed are very much the same: cluster membership, failure detection, backup and restore, and more.

Could we actually build “one operator to rule them all”? Well, not necessarily. Perhaps what we need is not literally one single operator, but a collection of higher-level interfaces that operators should adhere to, so they work with multiple data service types. This would enable administrators to choose an operator based on factors other than the vendor or project that created it. What if you could use an operator that would manage your Cassandra, ElasticSearch, and Kafka clusters? This is what we need to reduce the burden on operations teams and fully realize the benefits of managing stateful workloads on Kubernetes.

We need to build another layer of abstraction on top of the operator pattern. As a community, we can develop a common set of custom resources, and each controller can manage them in their own way. For example, we might define a “topology-aware StatefulSet” as a new CRD, or a cluster membership CRD that describes how a node joins a cluster. Instead of ElasticSearch developers and Cassandra developers creating separate definitions of a server group or topology, we could all agree that a distributed database has a concept of topology, agree on a CRD, and controllers can implement the specified behavior as needed.

The ideal end-state is a world with multiple implementations that adhere to a common standard. Kubernetes itself has a specification and each Kubernetes distribution has to provide certain APIs to be considered a valid distribution. Users can choose which operators to use in the same way, knowing that they can expect a baseline standard while applying other criteria.
