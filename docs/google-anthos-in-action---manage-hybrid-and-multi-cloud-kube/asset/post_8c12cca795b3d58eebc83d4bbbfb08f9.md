
## 21.2.1 Cluster Architecture

In this section, we will discuss the architecture of a bare metal cluster. Much of the architecture you know from the Anthos on VMware deployment is the same for bare metal, however, the bare metal option includes a few architecture differences from the VMware model.

| Customer Provided & Managed | Google Provided, Customer Managed |
|---|---|
| Anthos | Anthos |
| Red Hat Linux, CentOS, Ubuntu | Ubuntu, COS |
| Physical, Virtual, OpenStack | Virtual, VMware |
| Anthos on bare metal | Anthos on VMware |

### Cluster Roles

An Anthos Bare Metal installation has two kinds of clusters:

- **User Clusters**: Where applications are deployed; they include control plane nodes and worker nodes where containerized application instances run.
- **Admin Cluster**: A cluster that manages one or more user clusters; it is used to install, update, upgrade and delete user clusters through an Anthos on bare metal specific operator configured through 2 custom resources: `Cluster` and `Node Pool`.

An admin cluster includes only control plane nodes where the components used to manage the installation run. It also hosts some security-sensitive data, including SSH keys to access nodes' OS and GCP service account keys.

Unlike Anthos on VMware, user cluster control plane nodes are *decoupled* from the admin cluster.

### High Availability

It is possible to run the user or admin cluster control plane in high availability (HA) mode so that a control plane node failure does not affect cluster operations. This mode requires three or more control plane nodes.

If high availability is not required, it is possible to run a single control plane node in each cluster, but this should only be used for non-production workloads.

Along with the control plane, you need to consider high availability for the worker nodes as well. For applications with high availability a minimum of two worker nodes are needed. As with the control plane, you should never run production workloads without HA for the worker nodes.

### Deployment Models

This is where you will start to see the differences between how Anthos deploys on VMware and on bare metal. Anthos on Bare Metal provides a few different deployment models, offering flexibility to meet different organization requirements:

#### Standalone cluster deployment

In a standalone cluster deployment model, a single cluster serves both as the admin cluster and the user cluster. Since this model doesn’t require a separate admin cluster, you save three nodes in an HA setup. This can be helpful in scenarios where each cluster is managed independently or a single cluster for each deployment location is required—for example, an edge use case or an isolated network.

From a security perspective, you need to consider that user workloads will run on the same cluster that the control plane is running on. You will need to carefully consider securing your cluster to protect information like the node SSH keys and GCP service account keys. Implementing RBAC policies, OPA policies, network policies, and proper auditing will help to secure the cluster.

To provide more flexibility for these types of deployments, starting with Anthos version 1.8, Google reduced the minimum number of supported nodes per cluster from 2 to 1 and introduced, for standalone clusters, the new edge profile, which further reduces the hardware requirements.

**Figure 21.3. Standalone cluster deployment**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_754_0.png)

#### Multi-cluster deployment

This is the same deployment model that Anthos on VMware uses. In a multi-cluster deployment model you have a single admin cluster managing multiple user clusters; this deployment model is useful if you need to centrally manage a fleet of clusters deployed in the same datacenter.

**Figure 21.4. Multi-cluster deployment**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_756_0.png)

As you can see, Anthos on bare metal added greater deployment flexibility than the VMware deployment model. The added flexibility doesn't stop there, though; in the next section we will discuss the updates to the networking architecture.

### Networking Architecture

It makes sense that the networking architecture for the bare metal option would differ from the VMware model. Remember, in the VMware deployment, you can use an external load balancer or you can use the bundled load balancer, Seesaw. If you use the Seesaw option, Anthos will deploy a pre-configured virtual machine, or machines in HA mode, to your VMware environment.

In the bare metal deployment, there are no appliances or VM images from Google for any component. Don’t worry, though: Google has this covered using other components like HAProxy and MetalLB.

### Load Balancing

Anthos on Bare Metal requires L4 load balancing to expose the control plane endpoint, ingress endpoint, and applications using the LoadBalancer type Service. The load balancer’s responsibility is to route and balance the traffic to the appropriate nodes.

Anthos on Bare Metal can provide the needed load balancing capabilities through a bundled L4 load balancer (bundled load balancer mode) or, alternatively, you can use an external load balancing solution provided and configured by the customer (manual load balancer mode).

Whichever option you choose for L4 load balancing, during the installation an Envoy-based Istio ingress gateway is deployed, and it is exposed through a LoadBalancer type Service using the L4 load balancer. This Envoy deployment is used to provide application proxy capabilities to the applications, exposing them through standard Kubernetes Ingress objects.

#### Bundled load balancer mode

To begin with, let’s discuss the load balancing for the control plane. If you choose bundled load balancing, Anthos on bare metal deploys L4 load balancers during cluster installation, removing the requirement for you to provide an external load balancer. The load balancers can run on a dedicated pool of worker nodes or they can be located on the same nodes as the control plane; in either case the load balancer nodes must be in the same subnet.

Starting with Anthos 1.9, Google changed how the L4 load balancers are deployed. Previously, the HAProxy container image was deployed to the node(s) as a standard Docker container (i.e., not Kubernetes-controlled) running as a systemd service. Starting with 1.9, the Keepalived and HAProxy containers have been updated to run as static Kubernetes pods on the load-balancer nodes.

HAProxy is only used for load balancing the control plane. To provide L4 to the data plane, Anthos deploys a popular open-source solution called MetalLB. MetalLB will service requests in the cluster for any service that is deployed using the LoadBalancer type.

To recap the bundled load balancer components:

- **Control plane load balancing:** The control plane virtual IP address (VIP), routing traffic to the Kubernetes API server running on control plane nodes, is exposed through an HAProxy load balancer running as a Kubernetes pod on the load balancer nodes, together with a containerized keepalived service that manages HAProxy high availability.
- **Data plane load balancing:** The LoadBalancer type Service objects created for the applications and the Istio ingress gateway deployed with Anthos on bare metal are exposed through an Anthos-managed MetalLB deployment running on the load balancer nodes. IP addresses for LoadBalancer type Services can be automatically assigned from a predefined pool and are part of the same subnet where load balancer nodes are deployed.

Both control plane load balancing components (HAProxy + keepalived) and data plane load balancing components (MetalLB) run together on designated nodes (cluster control plane nodes or dedicated load balancer worker nodes).

The following picture shows an architecture example of a user cluster deployed in a single subnet with bundled load balancers running on control plane nodes:

**Figure 21.6. Load balancers running on control plane nodes**
