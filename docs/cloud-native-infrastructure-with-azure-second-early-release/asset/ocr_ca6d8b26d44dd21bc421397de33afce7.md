Computing Foundation’s (CNCF) graduated scheduling and orchestration platform.

Container orchestrators generally provide the following functionality:

+   - Autoscaling of cluster instances depending on instance load
+   - Provisioning and deployment of instances
+   - Basic monitoring functionality
+   - Service discovery

Other CNCF container orchestrators include:

+   - Azure Kubernetes Service (AKS)
+   - Azure Service Fabric
+   - Amazon Elastic Container Service (ECS)
+   - Docker Swarm
+   - Apache Mesos
+   - HashiCorp Nomad

Good container orchestrators make it simple to build and maintain compute clusters. At the time of this writing, there is no standard configuration for container orchestrators.

# Container Software

Container daemons, located just below the orchestrators, provide the software to execute a daemon. Depending on what orchestrator you are using, this may be transparent to you. Essentially, container daemons manage the lifecycle of the container runtime. In many cases, this will be as simple as starting or stopping the container or as complicated as interacting with plug-ins like Cilium (which we’ll discuss in Chapter 8).

Common container software platforms include: