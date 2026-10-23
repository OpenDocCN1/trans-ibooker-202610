If you need temporary access to a privileged account to achieve your persistence goals, keep in mind that you will need a privileged role to reset the credentials of the targeted account. For example, you may want to use a long-term persistence account to reset the password of a privileged user for short-term persistence activities in the tenant. This path may allow temporary access to the resources of the other account, but it should be pretty easy for defenders to trace the account that initiated the password resets.

> **Important Note**
> The role descriptions that are listed here come from the following Microsoft documentation page: https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#password-reset-permissions. This page has a very handy table that outlines which roles have rights to reset the passwords of other roles in the Azure AD tenant.

# Adding a secret to an existing service principal

Another less suspicious way to establish a backdoor identity for an Azure AD tenant is to add a password to an existing service principal. This was one of the methods that you practiced in Chapter 4, Exploiting Reader Permissions, using PowerZure. To perform this operation, an account that owns the service principal is needed or an account that has the permission highlighted in the following table. The built-in roles listed in the Azure AD Role column have this permission:

| Method | Permission Needed | Azure AD Role |
|---|---|---|
| Add a secret to an existing service principal | microsoft.directory/servicePrincipals/credentials/update | - Application Administrator
- Hybrid Identity Administrator
- Global Administrator
- Custom roles cannot currently be used |

Table 8.4 – Roles for adding a secret to service principals

Now that we have established how to access identities for persistence, let's look at how we can assign privileged roles to these identities.