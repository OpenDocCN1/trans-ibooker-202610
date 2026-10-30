
If the physical security of the ICS is adequate, then the primary threats are from the internet. In that case, a reasonable and common compensating control is to air-gap the networks used by the industrial control devices. This can provide adequate security if properly enforced, but keeping the air-gapped network safe requires careful diligence and technical controls to prevent foreign equipment from being connected. If external equipment must be connected periodically, say for maintenance purposes, consider using a firewall or proxy between the maintenance laptop and ICS network to provide an additional layer of security.

The classic example of compromising an air-gapped ICS was Stuxnet, which was used to cause considerable damage to Iran's uranium enrichment facility. It has been reported that the air-gapped network was compromised by a virus-infected USB thumb drive being plugged into a computer on the ICS network.

At a minimum, computers used to maintain and manage industrial systems must never be used for any other purpose (or removed from the facility). They must be regularly updated, employ anti-malware protection, and carefully scan any removable media (e.g., USB thumb drives) before being used. See the guidance published by the organizations listed earlier for more specifics.

If complete isolation from the corporate LAN and the internet is not an option, then it is essential to limit and screen permitted traffic accessing the ICS network through the use of carefully configured firewalls and network proxies. Consideration ought to be given to further segmenting the ICS network itself to limit the lateral spread of a compromise.

For ICSs that must be remotely accessible, compensating controls such as installing a web proxy or VPN should be considered to add an additional layer of security on top of whatever access controls are implemented on the ICS itself.

Security awareness training is also particularly important, and advocating security principles among all the technicians and plant operators is critically important. If those personnel burdened with the need to observe security controls understand the importance and rationale associated with those controls, they are less likely to view them as an impediment to their job of running the plant and something to be avoided or circumvented.

### Cloud-Based Systems

According to NIST, “*Cloud computing* is a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (e.g., networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction.” In short, cloud-based systems are remotely located, separately managed systems that are accessible by the internet.

NIST SP 800-145 and ISO/IEC 17788 define a number of characteristics that describe cloud computing:

*   **Broad network access:** Resources (physical and virtual) are accessible and managed over the network.
*   **Measured service:** Users pay only for the services they use.
*   **On-demand self-service:** Users can provision and manage services using automated tools without requiring human interaction.
*   **Rapid elasticity and scalability:** Services can be rapidly and automatically scaled up or down to meet demand.
*   **Resource pooling and multitenancy:** Physical or virtual resources are aggregated to serve multiple users while keeping their data isolated and inaccessible to other tenants.

There are three primary *cloud service models*, as shown in [Table 3.2](#c03.xhtml#c03-tbl-0002).

**[TABLE 3.2](#c03.xhtml#R_c03-tbl-0002)  Cloud Service Models**

| CLOUD SERVICE MODEL || SERVICE PROVIDED || SERVICE PROVIDER RESPONSIBILITIES || CUSTOMER RESPONSIBILITIES |
| --- | --- | --- | --- |
| Software as a service (SaaS) || Software application accessible to the customer over the internet (via a browser or API) || Provide and manage all infrastructure from server and network hardware to applications software || Provide the client device and manage user-specific configuration settings |
| --- | --- | --- | --- |
| Platform as a service (PaaS) || Web-based framework for developers to create customized applications || Provide and manage all infrastructure from server and network hardware to the libraries and runtime services necessary to run applications || Provide the application and manage the hosting environment |
| Infrastructure as a service (IaaS) || Infrastructure, including servers, network, storage, and operating systems, delivered through virtualization technology || Provide network and server infrastructure to support VMs and other virtualized resources || Provide and manage all components that run on the VM as well as limited aspects of network services |

With each service model, there is obviously a different division between the components supplied by the cloud service provider and those provided by the customer. This gives rise to the cloud shared responsibility model, as illustrated in [Figure 3.9](#c03.xhtml#c03-fig-0009). Security of cloud deployment is shared between the customer and the provider. The cloud service provider is responsible for the security of the cloud, and the customer is responsible for the security in the cloud. This also means that one has to consider the security capabilities and commitments of the provider when assessing the risk of a cloud deployment.

![FIGURE 3.9 The cloud shared responsibility model for IaaS, PaaS, and SaaS](images/c03f009.png)

**[FIGURE 3.9](#c03.xhtml#R_c03-fig-0009) The cloud shared responsibility model for IaaS, PaaS, and SaaS**

In particular, the cloud service provider is exclusively responsible for the following:

*   Physical security
*   Environmental security
*   Hardware (i.e., the servers and storage devices)
*   Networking (i.e., cables, switches, routers, firewalls, and internet connectivity)

The cloud service provider and the customer share responsibility for the following:

*   **Vulnerability and patch management:** Using [Figure 3.9](#c03.xhtml#c03-fig-0009) as a guide to determine which organization is responsible for which activities, the cloud service provider is responsible for patching the software below the responsibility dividing line, the customer is responsible for everything above the line (i.e., in the case of SaaS, the customer is responsible only for the computers and browsers used to access the cloud).
*   **Configuration management:** The cloud service provider is responsible for the infrastructure, the customer for everything above. Network configuration and security is a special case, as the customer of an IaaS provider typically has configuration control over significant aspects of the network routing and firewalls through APIs and web-based management consoles. As these network controls are a key component of the security of an IaaS cloud deployment, the customer must take great care in ensuring their proper configuration.
*   **Training:** Both the cloud provider and customer are responsible for the specific training required for their own personnel.

In the IaaS situation, in which the customer has responsibility for many aspects of network security, providing perimeter and interior security requires a different approach from traditional data center security. In the cloud, the customer rarely has the ability to install actual networking or security devices (such as firewalls, routers, switches, and the like), but instead is given API access to a limited number of firewall and router functions that they can use to control access to their VMs. Also, depending on the cloud service provider, access to network traffic, as one might expect in a data center using a network tap or a SPAN (mirror) port, may or may not be available, or might be available only on a sampled basis such as NetFlow or sFlow.
