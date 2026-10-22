4. Deployment options give operators the flexibility to install and scale a running Pulsar cluster based on use case and workload. It was also designed with multi-tenancy in mind, making a big efficiency difference in large deployments. Instead of installing a separate Pulsar instance per application, many applications (tenants) can use one Pulsar instance with guardrails to prevent resource contention. Finally, built-in storage tiering creates automated alternatives for storage persistence as data ages, and lower cost storage can be utilized.

Pulsar’s highest level of abstraction is an instance that consists of one or more clusters. We call the local logical administration domain a cluster and deploy in a Kubernetes cluster and where we’ll concentrate our attention. Clusters can share meta-data and configuration, allowing producers and consumers to see a single system regardless of location. Each cluster is made of several parts acting in concert that primarily consume either compute or storage which we will note.

![](bbox=[0.12, 0.48, 0.86, 0.83])

Figure 7-4. Apache Pulsar™ Architecture

# Broker (Compute)