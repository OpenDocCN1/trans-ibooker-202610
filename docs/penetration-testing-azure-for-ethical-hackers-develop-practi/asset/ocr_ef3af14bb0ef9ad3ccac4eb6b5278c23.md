Every Azure subscription has a trust relationship with one Azure AD tenant to manage access to the subscription. It is common for organizations to connect their on-premises AD to Azure AD using a tool called Azure AD Connect, as shown in the following diagram:

![](bbox=[0.12, 0.22, 0.82, 0.36])

Figure 1.2 – Using Azure AD Connect to synchronize objects to Azure AD

As the core of authentication and authorization, Azure AD is a prime target for information gathering, as well as different identity-based attacks. We will be covering more of the authorization model for Azure subscriptions in the Understanding the Azure RBAC structure section in this chapter.

+   - **Root Management Group**: This is the top of the Azure resource organization hierarchy, *if it’s enabled*. By default, the root management group is not enabled for an organization that is new to Azure, which means each subscription is managed as an individual entity. However, managing subscriptions individually creates a governance model that does not scale well. This is especially difficult for medium-to large-sized organizations with multiple subscriptions to manage.

Many production Azure environments will have the root management group enabled. This is partly because Microsoft recommends enabling it as part of their Cloud Adoption Framework document (https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/decision-guides/subscriptions/). This document describes some of the best practices for adopting the Azure cloud and is a great source of information for those looking at building out an environment in Azure.

+   - **Child Management Group**: If an organization has enabled the root management group, they could create child management groups under the root to simplify the management of their subscriptions. Child management groups allow organizations to group subscriptions together in a flexible structure, mainly to centrally manage access and governance. Child management groups can be nested and can support up to six levels of depth.