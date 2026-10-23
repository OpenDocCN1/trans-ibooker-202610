Apart from the Global Administrator role, the following Azure AD roles have permissions to reset user passwords: Password Administrator, Helpdesk Administrator, Authentication Administrator, User Administrator, and Privileged Authentication Administrator. The Privileged Authentication Administrator role has unrestricted permissions to reset passwords, but the other listed roles have limited permissions to reset passwords only for users who have fewer privileges than the roles have themselves. Microsoft has detailed all of the Azure AD roles with password reset permissions, and which users they can reset passwords for, at https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#password-reset-permissions.

Another point to consider for this path are additional security protections that may have been configured for an account. For example, you might want to review the sign-in logs to identify whether the user account was required to use MFA, or if other sign-in conditions were applied on previous sign-in events.

# Path 3 – Exploiting service principal secrets

Another path that could be used to move laterally from Azure AD to Azure resources is by exploiting an existing service principal that has Azure resource permissions. It is a common scenario in organizations to have service principals that have privileged role assignments, such as Contributor, at the subscription level. As a matter of fact, this is the default role assignment granted to service principals created automatically in Azure DevOps! We exploited this same scenario in Chapter 4, Exploiting Reader Permissions, to escalate privilege from a Reader role. Figure 7.4 shows how this attack would work:

![](bbox=[0.12, 0.605, 0.85, 0.795])

Figure 7.4 – Azure AD service principal to subscription relationship