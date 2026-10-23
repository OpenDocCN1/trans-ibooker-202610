Tokens typically have a life span of 60 days. Therefore, those stored in a token store must be refreshed from time to time. Each authentication provider has their own design and requirements for refreshing a token. It’s similar to sending a GET request to /.auth/refresh when the status result from an authentication request results in an expired token error.

In Chapter 4, you will configure Easy Auth as part of creating an Azure App Service instance, and the contents of an authentication request and response are described and presented in Chapter 7.

# Summary

In this chapter, we covered the most important step when migrating to the Azure platform: security. The fact that Azure Active Directory (AAD) is the feature that authenticates the access to the portal, via Azure PowerShell or Azure CLI, should be clear. AAD additionally provides an API that can be consumed from code running on the Azure platform to provide authentication into the application. Authorization is handled by role-based access control that protects the Azure resources within a subscription based on role and scope.

There are threat prevention, networking, compute, and additional Azure products and features that increase the level of security from a platform and application perspective such as WAF, Confidential Computing, Easy Auth, and NSGs. Combined, Azure provides all the necessary capabilities to run the most sensitive, private compute executions, house sensitive data, and comply with the most stringent security policies and restrictions.

# Exam Essentials

Understand Azure Active Directory. Know how to configure the most common features within AAD to pass the Azure Solutions Architect Expert exam. The most common have been discussed in this chapter. Understand that AAD provides security around the Azure Portal and is used to authenticate users. AAD also authenticates when calling a REST API, executing Azure PowerShell cmdlets, or running Azure CLI commands on Azure resources. AAD can be configured to authenticate for other Azure SaaS resources such as Office 365 or Dynamics Online.

Understand role-based access control. RBAC is an authorization model that exists within the Azure Portal. It comes with more than 40 built-in roles that are useful for granting permissions on create, read, update, and delete (CRUD) operations for Azure resources. Individuals are added to a role, and then that role is granted permission to an Azure resource at either the subscription, resource group, or resource level. RBAC also supports custom roles if one of the built-in roles does not meet your needs.