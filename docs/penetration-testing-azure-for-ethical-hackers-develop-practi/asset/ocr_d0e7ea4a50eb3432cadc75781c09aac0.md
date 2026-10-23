By the end of the chapter, we will have a good understanding of how organizations use Azure and how to approach an Azure environment as a penetration tester.

# Technical requirements

You won’t need any additional software for most of this first chapter, as we will be focusing on attaining a high-level understanding of the Azure infrastructure. Having Azure portal access to an existing subscription is certainly handy for following along, but not needed for understanding the concepts.

At the end of the chapter, we will review the multiple methods for accessing the Azure management interfaces. So, if you do have access to an existing Azure environment, the following tools will be helpful to have available:

+   - The Azure **command-line interface (CLI)**
+   - The Az PowerShell module
+   - The **Azure Active Directory (Azure AD)** PowerShell module

If you don’t have access to an existing Azure environment, don’t worry. We will go through the steps to creating your own testing environment in *Chapter 2, Building Your Own Environment.*

# The basics of Microsoft's Azure infrastructure

At the time of writing (late 2020–early 2021), the Azure platform consists of over 200 services and seems to be expanding all the time. It may feel that there is a lot of ground to cover here, but during a penetration test, you will typically only need to focus on a subset of the available services that you have in scope.

In general, it is important to understand how the environment is structured at the Azure platform level (subscriptions, RBAC, resources), and how the available services can be abused to gain additional privileges in the environment.

The lessons in this section will be fundamental to your understanding of how Azure functions as a platform, so pay close attention. For those with a solid Azure background, feel free to skim this chapter to refresh on the core principles.

In this section, we will gain an understanding of the Azure cloud platform and its regions, how Azure tenants are typically structured, and how the resources under the tenant are managed.