service interacts with the control plane using NLB.

Each control plane stores configuration in a local etcd database. These databases are replicated and set up in a [stacked high availability topology](stacked high availability topology).

One control plane manages one or more AWSNodePools.

# Figure 12.11 Anthos on AWS architecture

![](bbox=[0.125, 0.285, 0.865, 0.815])

The following VPC resources are required when creating Anthos on AWS clusters in a Dedicated VPC.