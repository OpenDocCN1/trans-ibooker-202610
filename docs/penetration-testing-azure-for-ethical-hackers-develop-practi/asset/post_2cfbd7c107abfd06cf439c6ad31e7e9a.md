
The organization structure in Azure consists of the following levels: **Azure AD Tenant**, **Root Management Group**, **Child Management Group**, **Subscription**, **Resource Group**, and individual **Resources**. These different levels are shown in the following diagram:

![](img/4fa338e84e0a590967a142dd798ba4e2_24_0.png)

Figure 1.1 – Azure resource hierarchy

Here are the descriptions of each level from the top down:

- **Azure AD Tenant**: In order to manage Azure subscriptions and resources, administrators need an identity directory to manage users that will have access to the resources. Azure AD is the identity store that facilitates the authentication and authorization for all users in an Azure tenant’s subscriptions.

Every Azure subscription has a trust relationship with one Azure AD tenant to manage access to the subscription. It is common for organizations to connect their on-premises AD to Azure AD using a tool called Azure AD Connect, as shown in the following diagram:

![](img/4fa338e84e0a590967a142dd798ba4e2_25_0.png)

Figure 1.2 – Using Azure AD Connect to synchronize objects to Azure AD

As the core of authentication and authorization, Azure AD is a prime target for information gathering, as well as different identity-based attacks. We will be covering more of the authorization model for Azure subscriptions in the Understanding the Azure RBAC structure section in this chapter.

- **Root Management Group**: This is the top of the Azure resource organization hierarchy, *if it’s enabled*. By default, the root management group is not enabled for an organization that is new to Azure, which means each subscription is managed as an individual entity. However, managing subscriptions individually creates a governance model that does not scale well. This is especially difficult for medium-to large-sized organizations with multiple subscriptions to manage.

Many production Azure environments will have the root management group enabled. This is partly because Microsoft recommends enabling it as part of their Cloud Adoption Framework document (https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/decision-guides/subscriptions/). This document describes some of the best practices for adopting the Azure cloud and is a great source of information for those looking at building out an environment in Azure.

- **Child Management Group**: If an organization has enabled the root management group, they could create child management groups under the root to simplify the management of their subscriptions. Child management groups allow organizations to group subscriptions together in a flexible structure, mainly to centrally manage access and governance. Child management groups can be nested and can support up to six levels of depth.

- **Subscription**: To provision resources in Azure, we need an Azure subscription. An Azure subscription is the logical container where resources are provisioned. When we create a resource such as a **Structured Query Language (SQL)** database, the first thing we usually do is specify the subscription where the resource will be provisioned. The usage of that resource will also be billed to the subscription that is selected.

As noted in the **Child Management Group** description, an organization will typically have multiple Azure subscriptions. It is quite common for organizations to set up multiple subscriptions for separate environments, such as development and production, or for separating application environments.

- **Resource Group**: Within subscriptions, there are resource groups. Resource groups are logical containers that can be used to group and manage Azure resources such as **virtual machines (VMs)**, storage accounts, and databases.

Resource groups are best used to collect and group resources that need to be managed together or share the same life cycle. Depending on the subscription architecture, this can result in large numbers of resource groups in individual subscriptions.

From an access perspective, resource groups also allow us to segment access to different groups in the same subscription, but we will cover that in more detail in the *Understanding the Azure RBAC structure* section.

- **Resources**: Resources are the individual instances of Azure services that are deployed in an Azure subscription. The resource level is the bottom of the organization hierarchy. Outside of services with specific access policies (see the information about *key vaults*), we can’t segment down the resources hierarchy any further than this.

We will get into the specifics of how access is managed using RBAC later in the chapter but in general, an Azure AD account can have specified access to any of the resource organization levels outlined previously. From a penetration-testing perspective, access granted at a higher level gives wider scope for an attacker to discover vulnerabilities in cloud-service configurations and to move laterally within the environment. This concept will be more important when we discuss privilege escalation.

In the next section, we will cover an overview of some of the commonly utilized Azure services. These individual instances of services fall under the **Resources** category listed previously.

## An overview of Azure services

As we noted earlier in this chapter, there are over 200 services available in Azure. Even though this sounds like a lot of services, they can generally be grouped into five categories, outlined as follows:

- **Services that are used to host applications**: These services provide a runtime environment that can be used to execute application code or run container images. Services such as **Azure App Service**, **Azure Virtual Machine (Azure VM)**, and **Azure Kubernetes Service (AKS)** fall into this category. Organizations use them to host external and internal applications.
- **Services that are used to store data for applications**: These services are used to store different kinds of application data. Services such as Storage accounts, Azure SQL, and Cosmos DB fall into this category.
- **Services that are used to create applications**: These services are used to create workflows that run in the cloud. Services such as Logic Apps and Functions apps fall into this category.
- **Services that are used to enhance applications**: These are typically **Software-as-a-Service (SaaS)**-type services in Azure that are used to provide extra capabilities to other applications. A service such as Azure Cognitive Services falls into this category. This is used by developers to add intelligence to their custom applications using pre-built and pre-trained machine learning algorithms.
- **Services that are used to monitor or manage applications**: These are services that are used to manage or monitor other services or applications. Services such as Azure Automation, API Management, Application Insights, and Azure Monitor fall into this category. Additional security-focused services, such as Azure Sentinel and Azure Security Center, would also fall into this category. These services can also provide useful insights from a penetration-testing perspective.

As we progress through the book, we will touch on many services, but the core resources that are important to understand are outlined here.

This table outlines some of the most common Azure services that will be attacked in this book:

| Service | Description |
|---|---|
| App Services | This service is inclusive of App Service and Function apps in Azure. These services can be used for "serverless" web application and API hosting. |
| Storage Accounts | Storage accounts are used for most data storage operations in Azure. Files can be publicly or privately hosted, and files can be accessed through several protocols (**HyperText Transfer Protocol (HTTP)**, **Server Message Block (SMB)**, **Network File System (NFS)**). This service can also be used for static web content hosting. |
| Automation Accounts | Another "serverless" code service, Automation accounts allow you to run scripting code (PowerShell and Python) from "runbooks" to automate processes in the subscription. There is a lot of surface area in this service, so we will return to it frequently throughout the book. |
| Virtual Machines | One of the initial backbones of cloud services, these are VMs hosted in the cloud. There are multiple facets to deploying VMs in the cloud, so this service will also be a major focal point. |
| Key Vaults | Azure key vaults are the primary credential store within Azure. This includes the storage of keys, secrets, and certificates. Aside from utilizing the standard **Identity and Access Management (IAM)** rights assignments, access policies can be applied to provide additional restrictions on Key Vault data. |
| Azure SQL | This service is for hosting SQL databases in the cloud. Everything about the SQL database is managed by Microsoft, which can help simplify the usage of databases in Azure projects. |
| Azure Container Registry/Azure Container Instances | As many environments start migrating to containerization, the usage of cloud container registries and cloud container instances is becoming more popular. Azure Container Registry holds the container images used as base images for containerization. While it is a relatively small part of a containerized environment, this service is a commonly targeted one, as the images frequently house sensitive information. |
