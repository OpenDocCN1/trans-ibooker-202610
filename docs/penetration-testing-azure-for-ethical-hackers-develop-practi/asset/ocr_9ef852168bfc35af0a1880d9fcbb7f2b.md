# Security principals

A **security principal** is a fancy term to describe *an Azure AD object that we want to assign privileges to*. This could be a user account, a group, a service principal, or a managed identity. These are all different types of identities that exist in Azure AD, as shown in the following screenshot:

![](bbox=[0.22, 0.21, 0.76, 0.42])

Figure 1.3 – Azure RBAC security principals

**Important note**
Do not confuse "security principal" with "service principal." *Security principal* is an overall term used to describe objects in Azure AD (including users, groups, and service principals). *Service principal* is a specific type of security principal.

While there are many facets to a security principal, keep in mind that every security principal has an **object ID** that uniquely identifies it in Azure AD. This object ID is typically used as a reference when assigning a role to the security principal. Next, we will cover the different types of security principals.

# User accounts

*A user account is a standard user identity in Azure AD.* These accounts can be internal to the Azure AD tenant, or an external "guest" account. In either case, the general administration of the users will occur at the Azure AD tenant level.

Internal user accounts are user identities created in the Azure AD tenant by an administrator. Additionally, these may be user identities that are synchronized from an on-premises AD environment to Azure AD (see Figure 1.2).