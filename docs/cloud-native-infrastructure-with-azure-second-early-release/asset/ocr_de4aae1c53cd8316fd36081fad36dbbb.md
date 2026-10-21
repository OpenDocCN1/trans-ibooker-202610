# Calico

Calico is an open source network security and policy solution for containers, virtual machines, and bare metal installations under Windows and Linux. Calico provides network security policy enforcement as well as the ability to implement zero-trust networks. Calico supports multiple data planes, including a state-of-the-art pure Linux extended Berkeley Packet Filter (eBPF) data plane, standard Linux networking data plane, and Windows HNS data plane. Calico runs on top of the host data plane to perform network policy enforcement. It supports the standard Linux networking stack.

# Why Would You Use Calico?

Managing network connectivity and policy becomes exponentially more difficult as your infrastructure grows. Furthermore, it is exceptionally difficult to ensure that the intended policy is correct and is applied correctly, and that feedback on the policy is applied. This is where Calico shines. It allows you to define your network policy in an abstracted, scalable manager and then ensures that the policy is being applied without sacrificing network performance.

# Basic Architecture

Calico has a client/data store architecture that simplifies operation of the platform. It includes the following key components (also depicted in Figure 8-1):

Felix

The network programmatic daemon that installs network access control lists (ACLs), routing information, and interface management and state reporting

BIRD