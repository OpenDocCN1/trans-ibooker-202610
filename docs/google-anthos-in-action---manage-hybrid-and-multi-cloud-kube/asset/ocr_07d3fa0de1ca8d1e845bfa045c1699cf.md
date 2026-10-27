only resources for users. At deployment a machine is assigned to a specific region based on the physical location of the GDCE system. A machine is only able to be part of a Kubernetes cluster deployed in its designated region.

# Cluster

A cluster consisting of a control plane and zero or more NodePools. A cluster is homed in a specific region and can connect NodePools only from that region, if a user tries to connect NodePool with machines homed in a different region from the cluster the operation will fail.

# Node Pool

NodePool is a logical grouping of machines in a GDCE Zone. NodePool is used to add worker nodes to clusters.

# VPN Connection

GDCE supports setting up a VPN connection to a GCP project allowing workloads running on a GDCE Kubernetes cluster to connect directly to GCP resources. At least one NodePool should be created in the cluster before establishing a VPN connection.

# Google Distributed Cloud Edge Network Resource Model

In addition to Kubernetes Cluster Resources, APIs and the default Kubernetes Pod Network, GDCE also allows customers to provision additional networks in a GDCE Zone and connect them with customer networks for different purposes. For example, in Network Functions use case, a customer might create an OA&M network and a Signal network, each with different multiple subnets that connect to the secondary interfaces of the Network Function pods.

The Figure 10.5 diagram describes the resources and their relationships for the network model in GDCE. From the high-level, there are 5 types of resources related to the edge network configuration.