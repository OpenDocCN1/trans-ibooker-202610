
Now that we have looked at different modernization strategies for existing applications and we know how and when to apply them. It is time to think about other prerequisites for our target platform.

## Kubernetes as the new application server?

The word platform in the Enterprise Java world normally refers to the application server. Application server follow a guard-railed software development approach with standardized APIs. The vertical layers are defined by what is commonly referred to as technical layers of a 3-tier system. Presentation on top of business on top of data access and/or integration. Horizontally to this we usually find business components or domains. While the vertical layers usually are well separated and decoupled, it is comparably common to find shared classes and violated access rules between the horizontal components. If this happens frequently across the code base, we talk about entangled designs that turn into unmaintainable monoliths over time. But no matter how entangled the application code is, it still profits from the standard application server functionalities addressing non-functional and functional requirements like security, isolation, fault tolerance, transaction management, configuration management, et cetera.

If we fast forward to distributed architectures of today, where applications consist of many small services, we observe two things. There is no longer a shortcut to a good component design, and the standard application server features are not available anymore to our components.

The first observation leads to a mandatory requirement. Distributed services have to be well designed, loosely coupled, and strongly encapsulated components. We will talk more about design principles and approaches for modernizing monoliths in [Chapter 5](#). The second observation holds a list of missing functionalities in cloud-native runtimes. If an application server isn’t providing support for commonly used functionalities like we mentioned above anymore there are only two places left. One can be the microservices framework of choice (e.g. Quarkus), another one could be additional frameworks or products on top of Kubernetes.

additional UIs or CLIs can make it easier for DevOps teams to share responsibility.

## Basic Invocation

Applications running inside containers are accessed through Ingress controllers. Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource. Traditionally, this can be compared with Apache httpd based load balancers. Other alternatives include projects like [HAProxy](HAProxy) or Nginx. You can use the routing capabilities to do rolling deployments as the basis for a sophisticated CI/CD strategy. For one-time jobs, such as batch processes, Kubernetes provides job and cron-job functionality.

## Elasticity

Kubernetes’ ReplicaSets control scaling of pods. It is a way to reconcile a desired state: you tell Kubernetes what state the system should be in, so it can figure out how to reach the outcome. A ReplicaSet controls the number of replicas, or exact copies, of a container that should be running at any time. What sounds like a largely static operation can be automated. The Horizontal Pod Autoscaler scales the number of pods based on observed CPU utilization. It is possible to use a custom metric or almost any other application-provided metric as input.

## Logging

One of the more challenging aspects of a distributed application is the correlation of logs from each active part. An area where the difference to traditional application servers becomes very visible because it used to be so simple and isn’t in the new world. Storing them individually, per container, is not recommended because you lose sight of the bigger picture and have a hard time debugging side effects and root causes for issues. There are various approaches to this, with most of them using the [ELK](ELK) ([Elasticsearch](Elasticsearch), [Logstash](Logstash), [Kibana](Kibana)) stack or a variant extensively. In those stacks, Elasticsearch is the object store, where all logs are stored. Logstash gathers logs from nodes and feeds them to Elasticsearch. Kibana is the web UI for Elasticsearch, which is used to search the aggregated log files from various sources.

## Monitoring

Monitoring in a distributed application is an essential ingredient to make sure all of the bits and pieces continue working. In contrast to logging, monitoring is an active observation often paired with alerting rather than simply recording events. [Prometheus](https://prometheus.io/) is the defacto standard for storing the generated information. Essentially, it is a complete open source monitoring system that includes a time-series database. Prometheus’s web UI gives you access to metric querying, alerting, and visualizations and helps you gain insights into your systems.

## Build and Deployment Pipelines

CI/CD (Continuous Integration/Continuous Delivery) aren’t anything new to enterprise Java applications or distributed applications. As a good software development practice, every production code should follow a strict and automated release cycle. With a potentially large number of services that compose an application, the automation should at least aim for 100% coverage. Traditionally a job for the open-source tool [Jenkins](https://www.jenkins.io/), modern container platforms have moved away from a centralized build system and embrace a distributed approach to CI/CD. One example is [Tekton](https://tekton.dev/). The goal is to create reliable software releases through build, test, and deployment. We dig deeper into this in [Chapter 4](#).

## Resilience and Fault Tolerance

Psychologists define resilience as the process of adapting well in the face of adversity, trauma, tragedy, threats, or significant sources of stress. In distributed applications, it is the concept of recovering from failure or load scenarios without human interaction. Kubernetes provides resilience options for the cluster itself, but only sparsely supports application resiliency and fault tolerance. For example, application-level resiliency can be facilitated through PersistentVolumes that support replicated volumes or with ReplicaSets ensuring a consistent number of pod replicas across the cluster. On an application-level, there is resilience and fault-tolerance support through Istio or various frameworks like [Cloudstate]. You want to use features such as retry rules, circuit breaker, and pool ejection.

> **NOTE**
> 
> [Istio] is an open-source service mesh that layers transparently onto existing distributed applications. It is also a platform, including APIs that integrate into any logging platform, telemetry, or policy system.

## Security

Authentication or Authorization between services is not part of Kubernetes itself. There are two ways to implement it. Using Istio, each service is provided with a strong identity representing its role and enables interoperability across clusters and clouds. It secures service-to-service communication, as well as providing a key management to automate key and certificate generation, distribution, rotation, and revocation. A more application-centric alternative can be to use a single-sign-on component like [Keycloak] or relying on [Eclipse MicroProfile JSON-Web-Token] (JWT).

## Tracing

Tracing gives you a way to follow request paths and events throughout the system across individual application parts by still allowing you to trace-back to an origin. You can find different approaches across the community today. Independent of languages, frameworks, or technologies you intend to use, Istio can enable distributed tracing. There are other commercial and open-source projects available helping with distributed tracing across your application components. [Zipkin] and [Jaeger] are two possible solutions.

## Define your target platform

It’s important to note that the nine elements mentioned above are focused on application development and do not capture all the necessities of a modern container platform. Just looking at this narrow focus leaves important areas unaddressed. A container platform needs to provide
