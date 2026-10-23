Membership of an Azure AD group can either be **assigned** or **dynamic**. Assigned memberships are direct user assignments by an administrator or group owner. Dynamic group memberships use rules that are automatically evaluated to determine group membership based on user or device attributes. The rules for dynamic group membership can allow for soft matching, which could be abused to escalate privileges. See the *Dynamic group membership* section in *Chapter 4, Exploiting Reader Permissions*, for more information on this attack.

In Azure environments that sync with on-premises AD domains, the complexity of organizing users can increase exponentially as there can be a large number of groups that are imported from the on-premises domain. While there may be a limited number of groups that are actually utilized in the Azure tenant, you may need to deal with hundreds (or thousands) of groups that are in an Azure AD tenant. This shouldn’t cause any issues while assessing the Azure environment, but it may complicate things as you begin to pivot to on-premises environments. We will cover tools that help simplify assessing AD groups in *Chapter 7, Exploiting Owner and Privileged Azure AD Role Permissions.*

# Role definition

After security principals, the second component of Azure RBAC is a role definition. This term describes *a collection of permissions*. A permission describes an action that *may* or *may not* be performed on a resource, such as read, write, and delete. We can examine the permissions under a default Azure role to see what it allows us to do.

Here are the permissions for the **Contributor** role in Azure:

```
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants full access to manage all resources, but does not allow you to assign roles in Azure RBAC, manage assignments in Azure Blueprints, or share image galleries.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b2498ac-6180-42a0-ab88-20f7382dd24c",
  "name": "b2498ac-6180-42a0-ab88-20f7382dd24c",
  "permissions": [
    {
      "actions": [
        "*"

```