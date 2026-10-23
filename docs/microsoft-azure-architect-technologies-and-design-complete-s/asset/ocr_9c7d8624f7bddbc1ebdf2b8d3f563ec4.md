Both internal and public load balancers by default will route traffic equally to the Azure VMs configured into the backend pool. This is often referred to as round-robin. Using a five-tuple hash that consists of source IP, source port, destination IP, destination port, and the protocol, the load balancer routes the request to the same Azure VM for the entire transmission session. Any change in the hash values will result in the request being sent to a different server. This may be an issue if there is a need to maintain a session between the client and server. If this is the case, then Azure Load Balancer does support this using the feature called source IP affinity mode. This mode uses a hashed version of the source IP, destination IP, and/or the protocol to route requests to the same Azure VM for the life of the session.

# Internal Load Balancer

An internal load balancer (ILB) will not have a public IP address and is instead configured into an existing virtual network. As shown in Figure 3.69, traffic is load balanced across a group of Azure VMs, and the connectivity from those VMs to other servers in the VNet is managed using an ILB.

FIGURE 3.69 Internal load balancer flow

![](bbox=[0.13, 0.48, 0.88, 0.78])

The ILB could be managing access to a group of database servers, REST APIs, or some other kind of resource needing this added level of security and resiliency.

# Health Probes

Health probing is a feature that monitors the status of Azure VMs configured in the backend pool. There are three probe types, TCP, HTTP, and HTTPS. (HTTPS is available only in Standard SKU.) The TCP probe will start a three-way TCP handshake on the configured