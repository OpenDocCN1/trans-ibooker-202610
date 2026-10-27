cluster nodes can fetch GKE on-prem components and call certain Google services. If you want external clients to call services in your GKE on-prem clusters, your network must support inbound traffic from the internet.

IP address architecture and allocation is discussed in the next section.

# Anthos on Prem cluster IP allocation

The following IP addresses are required for Anthos on Prem on VMWare clusters.

+   1. **Node IP -** DHCP or statically-assigned IP addresses for the nodes (virtual machines or VMs). Must be routable within the data center. You can manually assign static IPs. Node IP addressing depends upon the implementation of Load Balancer in the Anthos on Prem cluster. If the cluster is configured with integrated mode load balancing or bundled mode load balancing, you can use DHCP or statically-assigned IP addresses for the nodes. If the cluster is configured with manual mode load balancing, you must use static IP addresses for nodes. In this case, ensure enough IP addresses are set aside to account for cluster growth. Load balancing modes are discussed in detail in the next section.
+   2. **Pod IP CIDR -** Non-routable CIDR block for all Pods in the cluster. From this range, smaller /24 ranges are assigned per node. If you need an N node cluster, ensure this block is large enough to support N x /24 blocks.
+   3. **Services IP -** In Island Mode, similar to Pod CIDR block, so only used within the cluster. Any private CIDR block which does not overlap with the nodes, VIPs, or Pod CIDR block. You can share the same block among multiple clusters. The size of the block determines the number of services. In addition to your Services, a block of Service IP addresses are used for cluster controlplane Services. One Service IP is needed for the ingress service, and ten or more IPs for Kubernetes services like cluster DNS, etc.

# Egress traffic and controls