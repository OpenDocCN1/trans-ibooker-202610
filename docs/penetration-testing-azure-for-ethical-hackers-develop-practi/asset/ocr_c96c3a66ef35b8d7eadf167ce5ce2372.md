# Azure Resource Manager

Regardless of the tool or method that we are using to interact with the Azure platform and Azure resources, the communication happens through a single central endpoint called Azure Resource Manager (ARM). You can think of it as a centralized layer for resource management in Azure (see Figure 1.19). The advantage of this approach is that there is consistency regardless of the tool that we are using. Authentication and access are all handled the same way.

When we make a request for an operation to be performed, using any of the tools that we described earlier, Resource Manager will talk to "resource providers" that perform the action we've requested, as illustrated in the following diagram:

![](bbox=[0.12, 0.38, 0.88, 0.68])

Figure 1.19 – ARM

Resource providers are services that provide different types of resources. For example, the network resource provider is responsible for network resources (virtual networks, network interfaces, and so on), while the compute resource provider is responsible for compute resources (VMs).