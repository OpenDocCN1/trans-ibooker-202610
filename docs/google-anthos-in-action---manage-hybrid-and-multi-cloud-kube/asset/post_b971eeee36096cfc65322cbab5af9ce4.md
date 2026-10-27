
deployed in the site types described in the previous sections:

- Public Cloud Edge: Google edge locations
- Telco Edge: Owned by Telco providers
- Enterprise Edge: Owned by the final customer (such as in retail stores, factory floors, branch offices, stadiums, etc.)

### Figure 10.3: Google Distributed Cloud Edge High Level Architecture

![](img/fb3bebce7b5f593bca1a7f8a1371be06_308_0.png)

GDCE provides fleet management capabilities to manage all the hardware and software assets and supports both containerized workloads and virtual machines through Anthos on Bare Metal capabilities.

GDCE also provides users a VPN connection to GCP allowing users to interact with other applications running in a customer’s VPC and other GCP services. To service high performance and low latency workloads GDCE also provides a number of high network performance features such as SR-IOV and DPDK. Google Cloud operates and manages the underlying infrastructure extending the cloud experience to the customer premises.

Google provides the compute, network and storage hardware, those are shipped to the target location and managed by Google. The Google operations team will do the management and monitoring of the Google gear remotely.

The customer needs to have a designated contact person (part of the customer personnel, not a Google person) at the target location with access to the gear and permissions to perform basic administrative tasks. Those include, for example, cold restart, replacing of parts, ability to run local diagnostics on the gear and common system administration tasks.

After the hardware is connected and operational, users can consume the service using standard Google API tools and use the service.

A GDCE Kubernetes cluster consists of a control plane and worker nodes. Worker nodes are organized into node pools. The control plane is hosted on GCP in a single compute region. Worker nodes are servers on GDCE racks, each worker node uses a full physical server. A rack or a group of racks with shared space, power, cooling and contiguous network fabric is defined to be a GDCE Zone.

## Google Distributed Cloud Edge Container Resource Model

GDCE has a different resource model compared to Anthos and other Anthos deployment options. Here you will find a list of the container resources used to describe, implement and manage the compute architecture of a GDCE implementation, described schematically in the Figure 10.4.

### Figure 10.4: Google Distributed Cloud Edge Container Resource Model

![](img/fb3bebce7b5f593bca1a7f8a1371be06_310_0.png)

### Zone

Zone represents a set of machines sharing a network fabric or a single fault domain. A zone can represent one rack or a number of racks placed in the same location. The GDCE Zone resource is different from GCE Compute Zones. Listing the GCE Compute Zones will not return the GDCE Zones. Zone represents a single failure domain, to deploy fault-tolerant applications with high availability and help protect against failures, users need to deploy applications across multiple Zones linked to the same region.

### Machine

A machine represents a physical server, the machine resource metadata includes which rack it is on and other properties and tags. Machines are read-only resources for users. At deployment a machine is assigned to a specific region based on the physical location of the GDCE system. A machine is only able to be part of a Kubernetes cluster deployed in its designated region.

### Cluster

A cluster consists of a control plane and zero or more NodePools. A cluster is homed in a specific region and can connect NodePools only from that region, if a user tries to connect NodePool with machines homed in a different region from the cluster the operation will fail.

### Node Pool

NodePool is a logical grouping of machines in a GDCE Zone. NodePool is used to add worker nodes to clusters.

### VPN Connection

GDCE supports setting up a VPN connection to a GCP project allowing workloads running on a GDCE Kubernetes cluster to connect directly to GCP resources. At least one NodePool should be created in the cluster before establishing a VPN connection.

## Google Distributed Cloud Edge Network Resource Model

In addition to Kubernetes Cluster Resources, APIs and the default Kubernetes Pod Network, GDCE also allows customers to provision additional networks in a GDCE Zone and connect them with customer networks for different purposes. For example, in Network Functions use case, a customer might create an OA&M network and a Signal network, each with different multiple subnets that connect to the secondary interfaces of the Network Function pods.

The Figure 10.5 diagram describes the resources and their relationships for the network model in GDCE. From the high-level, there are 5 types of resources related to the edge network configuration.

- **Network**: a virtual network in a GDCE zone with private address space, which may contain one or more subnetworks. A network is isolated from other networks of the same GDCE Zone.
- **Subnetwork**: a layer 2 (VLAN) subnet in a GDCE network. A subnetwork has its own broadcast domain and customer-assigned CIDR. Subnetworks within a network can reach each other. Subnetworks of different networks in a GDCE Zone can not reach each other.
- **Interconnect**: represents a bundled logical link of one or more physical links between GDCE and a customer network. An Interconnect can only be created at GDCE site turnup time. Multiple Interconnects are typically configured to provide high-availability.
- **InterconnectAttachment**: a virtual link provisioned on top of an Interconnect based on customer requests, to provide an isolated connection between a GDCE network and a customer network (e.g. a VRF). Packets flowing through an InterconnectAttachment will be untagged, or tagged with a customer-specified VLAN id.
- **Router**: A virtual routing instance to configure routing functionalities for a network in a GDCE zone, e.g. customers can use it to configure a BGP peering session over an InterconnectAttachment between a GDCE network and customer network, or over a pod subnet, so that certain pods can advertise prefixes to GDCE. By default, the routes received from subnetworks will be readvertised.

### Figure 10.5: Google Distributed Cloud Edge Network Resource Model

![](img/fb3bebce7b5f593bca1a7f8a1371be06_313_0.png)

They are similar to the Google Cloud network abstractions with a few differences:

- All network resources created for GDCE are local to the GDCE Zone. A GDCE network doesn't have direct connectivity to a GCP VPC. Networks in different GDCE zones also don't have direct connectivity to each other, unless being intentionally connected by the customer.
- GDCE subnetworks support VLAN, and are thus isolated with each other in layer 2.

System Administrators should create and maintain all Network Resources. App Developer / Owner only has view access to network resources.

## 10.2.2 Anthos capabilities for Telco and Edge workloads

Besides the specific characteristics of GDCE, there are Anthos capabilities, available in the standard product, that help the deployment, execution and management of edge and telco workloads. Some of these capabilities have been designed specifically for this purpose, and are mainly provided with Anthos on Bare Metal, that is the most suitable version for these workloads and is also the core of GDCE, while others, such as Anthos Config Management, are provided for general purpose but can be adapted for edge and telco applications.

### Multiple Network Interfaces for pods

A common Containerized Network Functions (CNFs) requirement is to have additional network interfaces provisioned to Pods on top of the default k8s interface, this is often needed to keep separation between data plane and management/control plane, for performance or security reasons, or to isolate network flows for other reasons.

Anthos provides this capability on Anthos on Bare Metal, using a specific implementation of the Multus CNI Plugin.

### Architecture

This capability allows a single pod to connect to multiple networks. The default kubernetes interface will be seen in the pod as eth0, while additional interfaces, created through multus CNI, will be seen by default as net1, net2 ... (can be configured).

The CNI used for the additional interfaces can be:

- Ipvlan
- Macvlan
- Bridge
- SR-IOV
