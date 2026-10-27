inspect the Host header and route the traffic to the appropriate Service inside the cluster.

# Figure 12.17 Anthos GKE hybrid multi-cluster networking: Anthos Service Mesh (ASM)

![](bbox=[0.159, 0.206, 0.813, 0.542])

For Distributed Services, we recommend using ASM, which provides Service Discovery and traffic routing functionality across multiple clusters.

# Hybrid and multi-cloud Connectivity

You can connect Services running in multiple Anthos clusters across multiple infrastructure environments as long as you can reach the Kubernetes API server and the external public load balancer of the target Anthos cluster.

There are three main ways to connect infrastructure environments.

+   1. [Cloud Interconnect](https://cloud.google.com/network-connectivity/docs/interconnect) - Cloud Interconnect extends on-premises network to Google's network through a highly available, low latency connection. You can use Dedicated Interconnect to connect directly to Google or use Partner Interconnect to connect to Google through a supported service