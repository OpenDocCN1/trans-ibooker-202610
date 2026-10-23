4. Create three more users where the directory roles are as follows:

+   - User
+   - Limited Administrator › Application Developer
+   - Limited Administrator › Cloud Application Administrator

5. Once completed, the list of users should resemble Figure 2.10.

# FIGURE 2.10 A list of Azure AD users

![](bbox=[0.14, 0.28, 0.87, 0.5])

When you sign in using one of the created users, the account will not be able to create any Azure resources. The reason is that the accounts are not linked to any subscription. This becomes apparent when navigating to the Subscription blade, which lists the subscriptions that are linked to the account. If the user clicks the + Add button, then they can create their own subscription that allows them to create Azure resources. In Exercise 2.6, you will grant a user access to a subscription.

There is a difference between Azure Active Directory and the security mechanism used within Azure to control access specifically to Azure resources. This is called role-based access control, as discussed in the next section. The point to note here is that AAD is a tenant that can be used to authenticate users across different products, while RBAC controls access to the Azure resources within the Azure Portal. When RBAC is covered in the next section, you will grant users access to the subscription and restrict access based on roles and groups.

Let’s now get into the details of other Azure Active Directory products and features.

# AAD Connect

AAD Connect, as shown in Figure 2.11, is used to create a hybrid identity solution existing between an on-premise Active Directory and an Azure Active Directory. One feature of