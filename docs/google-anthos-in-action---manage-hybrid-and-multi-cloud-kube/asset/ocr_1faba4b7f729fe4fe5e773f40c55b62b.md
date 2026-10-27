servers, and on AWS. In addition to the supported Anthos clusters, you can also register any [conformant](conformant) Kubernetes cluster to the Anthos platform. For example, you can register [EKS](EKS) clusters running in AWS and [AKS](AKS) clusters running in Azure to the Anthos platform. There are currently six types of Anthos clusters.

+   - Anthos clusters on GCP (GKE)
+   - Anthos clusters on VMWare (GKE on-prem)
+   - Anthos clusters on bare metalAnthos clusters on AWS (GKE on AWS)
+   - Anthos clusters on Azure (GKE on Azure)
+   - Anthos attached clusters (conformant Kubernetes clusters)

**Anthos multi-cluster networking** - Anthos multi-cluster networking deals with environments with multiple clusters where services need to communicate across cluster boundaries. This section can be divided into two subsections:

+   - **Anthos multi-cluster networking with GKE on GCP** - In GKE on GCP, you can either have a flat network architecture (using a single or shared VPC) or a multiple network (multiple VPC) model. In a flat network architecture using VPC-native GKE clusters, VPC networking automatically allows for Pod to Pod connectivity between multiple clusters. Clusters can be in any region. No additional configuration is required for Pod to Pod connectivity between clusters. In a multiple VPC architecture, you need additional configuration to connect Pods and Services between multiple clusters. For example, you can use special gateways or ingress model to communicate between clusters.
+   - **Anthos multi-cluster networking in non-GCP environments** - In all non-GCP clusters, every cluster and specifically Pod IP address range is isolated from other clusters. This means that there is no direct connectivity between Pods in multiple clusters. In order to connect multiple clusters, special gateways or ingress must be used. Anthos Service Mesh can be used to deploy such gateways. These are often called “east-west gateways” and are deployed in all clusters participating in a multi-cluster mesh. In addition Anthos Service Mesh also provides multi-cluster service discovery.

**Service layer networking** - The top layer of Anthos networking is service layer networking. This layer addresses how services discover