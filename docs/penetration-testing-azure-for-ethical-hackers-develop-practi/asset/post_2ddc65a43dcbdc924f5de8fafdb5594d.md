
Here are the topics that we will cover:

- Escalating from Azure AD to Azure RBAC roles
- Escalating from subscription Owner to Azure AD roles
- Attacking on-premises systems to escalate in Azure

## Technical requirements

In this chapter, we will be talking about how we can use the BloodHound tool to find paths in Azure tenants (and normal Active Directory) that can be abused to escalate privileges. While it is not absolutely needed to follow along, it's a great tool to have in the arsenal:

- BloodHound: https://github.com/BloodHoundAD/BloodHound

## Escalating from Azure AD to Azure RBAC roles

As highlighted in Figure 7.1, an Azure AD role assignment does not grant access to manage Azure resources and an Azure RBAC role assignment does not grant access to manage Azure AD resources by default:

![](img/4fa338e84e0a590967a142dd798ba4e2_274_0.png)

Figure 7.1 – Azure AD roles versus Azure RBAC roles

**Important note**
Even though our discussion in this chapter centers on the Global Administrator role in Azure AD, there are other built-in roles in Azure AD that could be used to perform this attack. This includes the following roles: **User Administrator**, **Groups Administrator**, and **Directory Writers**. Additionally, any Azure AD custom role with the following permission could be exploited: `microsoft.directory/groups/members/update`.

This is a very simple attack and we exploited something similar in a hands-on exercise in *Chapter 4, Exploiting Reader Permissions*, but with a slight variation. If a user already has rights in a subscription, it would be easy to identify any privileged groups in the subscription, but what can we do if our Active Directory user doesn't have any subscription rights? One way is by reviewing the Azure AD sign-in log.

The Azure AD sign-in log automatically records both interactive and non-interactive sign-in activities. From an offensive perspective, the log contains a wealth of information that could give an attacker insight on which users have access to each Azure AD application, which user accounts or applications are easy targets, and information on how to evade defensive configurations!

In our case, we could either log in through the Portal or use Azure PowerShell to review the logs to see the users who have access to Azure resources. We could then correlate that information with the group membership of the users.

To use Azure PowerShell to review the sign-in logs, we first need to install the AzureAD preview PowerShell module. At the time of writing, this feature is only available in the preview version of the module. Some preview features never make it to the generally available release, so this *might* not be generally available in the future.

Note that this module cannot be installed side by side with the **Generally Available (GA)** version of the AzureAD module, so we may first need to remove the GA version if we have it installed. Assuming you have been following the examples, AzureADPreview should have been installed as part of setting up PowerZure.

If the module is not set up, you can use the following instructions to do this:

```
# Remove the GA version if it is installed
Uninstall-Module AzureAD
# Install the preview version of the AzureAD module. Close and re-open the PowerShell console afterwards
Install-Module AzureADPreview
# Verify that the GA version of the module is installed
```

```
Import-Module AzureADPreview
Get-Module AzureADPreview | select Name,Version
Name        Version
----        -------
AzureADPreview 2.0.2.138
```

To review the sign-in logs for interesting events, we can use the Get-AzureADAuditSignInLogs cmdlet as shown in the following commands (this command requires a native work/school Azure AD user account and not a Microsoft account). The following commands will filter the sign-in logs for successful Azure Portal or Azure PowerShell sign-ins in the time range specified (the date can be modified to the date before your current date to get the results for the past 24 hours). The output of these commands provides great information that we can use to verify the users of Azure management tools, which usually means that they have some level of access to Azure resources:

```
# First connect to Azure AD
Connect-AzureAD
# Get successful Azure Portal sign-ins in the time range (greater than the specified date)
Get-AzureADAuditSignInLogs -Filter "appDisplayName eq 'Azure Portal' and createdDateTime gt $((Get-Date).AddDays(-1).ToString('yyyy-MM-dd')) and status/errorCode eq 0"
# Get successful Azure PowerShell sign-ins in the time range (greater than the specified date)
Get-AzureADAuditSignInLogs -Filter "appDisplayName eq 'Microsoft Azure PowerShell' and createdDateTime gt $((Get-Date).AddDays(-1).ToString('yyyy-MM-dd')) and status/errorCode eq 0"
```

We can filter this further to get the properties that are useful to show us the security configurations of the user accounts. This information can be useful if we need to look to bypass defenses:

```
# Get interesting authentication properties for successful Azure Portal sign-ins
Get-AzureADAuditSignInLogs -Filter "appDisplayName eq 'Azure Portal' and createdDateTime gt $((Get-Date).AddDays(-1).ToString('yyyy-MM-dd')) and status/errorCode eq 0" | Select-Object UserPrincipalName, MfaDetail, AppliedConditionalAccessPolicies
```

Here is a sample output of this command:

![](img/4fa338e84e0a590967a142dd798ba4e2_278_0.png)

Figure 7.3 – PowerShell command results for sign-in auditing

The following Azure AD built-in roles have the permissions needed to read this information: Security Administrator, Security Reader, and Report Reader. Additionally, any custom Azure AD role that has the following permissions could also be used:

- microsoft.directory/groups/allProperties/allTasks - Grants permission to read and update group properties in Azure AD
- microsoft.directory/signInReports/allProperties/read - Grants permission to read sign-in reports

It is also worth noting that there are opportunities to move laterally from Azure AD to other cloud applications that may be using Azure AD as their identity provider if your pentest scope covers that. You can discover other applications that have a trust relationship with Azure AD by reviewing the Enterprise Applications in Azure AD. You can use this section to find out the users authorized for the applications and the groups that they belong to. If the access is granted via groups, you can add yourself to the groups to gain permissions to those cloud applications.

## Path 2 – Resetting user passwords

Another path that could be exploited to move laterally to another Azure subscription is to reset the password of a user who has permissions to another Azure subscription and logging in as that user. This path is less preferred as it impacts the existing access of a user and is more intrusive. It easily raises an alarm if a user is suddenly not able to log in to their account with the password that they have always used, and the audit logs reflect the recent password change event.

Apart from the Global Administrator role, the following Azure AD roles have permissions to reset user passwords: Password Administrator, Helpdesk Administrator, Authentication Administrator, User Administrator, and Privileged Authentication Administrator. The Privileged Authentication Administrator role has unrestricted permissions to reset passwords, but the other listed roles have limited permissions to reset passwords only for users who have fewer privileges than the roles have themselves. Microsoft has detailed all of the Azure AD roles with password reset permissions, and which users they can reset passwords for, at https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#password-reset-permissions.

Another point to consider for this path are additional security protections that may have been configured for an account. For example, you might want to review the sign-in logs to identify whether the user account was required to use MFA, or if other sign-in conditions were applied on previous sign-in events.

## Path 3 – Exploiting service principal secrets

Another path that could be used to move laterally from Azure AD to Azure resources is by exploiting an existing service principal that has Azure resource permissions. It is a common scenario in organizations to have service principals that have privileged role assignments, such as Contributor, at the subscription level. As a matter of fact, this is the default role assignment granted to service principals created automatically in Azure DevOps! We exploited this same scenario in Chapter 4, Exploiting Reader Permissions, to escalate privilege from a Reader role. Figure 7.4 shows how this attack would work:

![](img/4fa338e84e0a590967a142dd798ba4e2_279_0.png)

Figure 7.4 – Azure AD service principal to subscription relationship
