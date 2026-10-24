Elasticsearch, which is used to search the aggregated log files from various sources.

# Monitoring

Monitoring in a distributed application is an essential ingredient to make sure all of the bits and pieces continue working. In contrast to logging, monitoring is an active observation often paired with alerting rather than simply recording events. [Prometheus](https://prometheus.io/) is the defacto standard for storing the generated information. Essentially, it is a complete open source monitoring system that includes a time-series database. Prometheus’s web UI gives you access to metric querying, alerting, and visualizations and helps you gain insights into your systems.

# Build and Deployment Pipelines

CI/CD (Continuous Integration/Continuous Delivery) aren’t anything new to enterprise Java applications or distributed applications. As a good software development practice, every production code should follow a strict and automated release cycle. With a potentially large number of services that compose an application, the automation should at least aim for 100% coverage. Traditionally a job for the open-source tool [Jenkins](https://www.jenkins.io/), modern container platforms have moved away from a centralized build system and embrace a distributed approach to CI/CD. One example is [Tekton](https://tekton.dev/). The goal is to create reliable software releases through build, test, and deployment. We dig deeper into this in [Chapter 4](#).

# Resilience and Fault Tolerance

Psychologists define resilience as the process of adapting well in the face of adversity, trauma, tragedy, threats, or significant sources of stress. In distributed applications, it is the concept of recovering from failure or load scenarios without human interaction. Kubernetes provides resilience options for the cluster itself, but only sparsely supports application resiliency and fault tolerance. For example, application-level resiliency can be facilitated through PersistentVolumes that support replicated volumes or with ReplicaSets ensuring a consistent number of pod replicas across the cluster. On an application-level, there is resilience and fault-tolerance