# Network Functions

# Introduction to Network Functions Virtualization (NFV)

Traditionally, Network Operators used to implement network functions on dedicated, proprietary hardware appliances. From the mid 2010s, the concept of Network Functions Virtualization (NFV) emerged as telcos looked at virtualizing their networking functionality, following the same pattern that led to virtualization of IT Servers, to consolidate many network equipment types onto industry standard high volume servers, switches and storage in order to reduce costs and increase efficiency, agility and resilience.

A trend started to transform Network Appliances in Virtual Network Functions (VNF), Virtual Machines deployed on industry standard x86 servers through an hypervisor.

The 3 Main working domains of a NFV Architecture are:

+   - Virtualized Network Functions (VNFs): x86 compliant virtual machine versions of the network appliances.
+   - Network Function Virtualized Infrastructure (NFVI): All hardware (servers, storage, network gear) and software components (Virtualization sw) that build up the environment hosting the Virtual Network Functions.
+   - Management and Orchestration (MANO): Lifecycle Management and Orchestration of physical or software resources that support the infrastructure virtualization and the lifecycle management of the Virtual Network Functions.

Figure 10.1: High Level NFV Architecture Framework