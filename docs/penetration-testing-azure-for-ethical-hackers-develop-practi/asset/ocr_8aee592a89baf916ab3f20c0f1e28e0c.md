The commands will be successful, proving that you have used your elevated Global Administrator role to modify permissions for Azure resources! Here is the output of the command:

```
root@PentestVM:~#
root@PentestVM:~# userPrincipalName=$(az ad signed-in-user show --query userPrincipalName -o tsv)
root@PentestVM:~# az role assignment create --role "Owner" --assignee $userPrincipalName
{
  "canDelegate": null,
  "condition": null,
  "conditionVersion": null,
  "description": null,
  "id": "/subscriptions/204cce89-.../providers/Microsoft.Authorization/roleAssignments/5ac7db39-887b-40c6-a743-e562f2120a6a",
  "name": "5ac7db39-887b-40c6-a743-e562f2120a6a",
```

Figure 7.17 – Owner role assignment

We have now modified our privileges to an Azure subscription from Azure AD. Let's examine the reverse scenario and techniques that can be employed to achieve this.

# Escalating from subscription Owner to Azure AD roles

As part of a pentest, you could also be interested in pivoting from an Azure subscription to Azure AD. This objective could be part of an attack chain that has a goal of opening backdoors in Azure AD for persistence. We will cover the topic of persistence in Chapter 8, Persisting in Azure Environments. Here are some techniques that could be leveraged to achieve this.

# Path 1 – Exploiting privileged service principals

Similar to user accounts, service principals and managed identities can also be assigned to Azure AD roles. Many attackers consider service principals and managed identities to be easier targets as they are usually excluded from security policies such as conditional access and MFA.

An attacker could exploit the privileges of an Azure AD account with rights to service principals or managed identities to gain access to the security privileged principals. This is a possible path, but it may be rare for you to run into, especially if an organization strictly follows the principle of keeping role assignments separate for the scopes.