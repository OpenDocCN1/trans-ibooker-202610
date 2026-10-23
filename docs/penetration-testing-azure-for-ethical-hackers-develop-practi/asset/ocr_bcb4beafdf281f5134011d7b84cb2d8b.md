From an attack standpoint, we can exploit the permissions on the management plane to gain access to keys and/or tokens that we can use to access data stored in the data plane. The management plane permissions that we can exploit are as follows:

+   - `Microsoft.Storage/storageAccounts/listkeys/action`: Allows the Contributor role to read the access keys of storage accounts. The access keys give full access to the data plane.
+   - `Microsoft.Storage/storageAccounts/listAccountSas/action`: Allows the Contributor role to generate a SAS token for data plane access at the storage account level.
+   - `Microsoft.Storage/storageAccounts/listServiceSas/action`: Allows the Contributor role to generate a SAS token for data plane access at the service level.

It is worth noting that the READER role that we covered in *Chapter 4, Exploiting Reader Permissions*, does not have these permissions.

Attack tools such as MicroBurst and Lava exploit the preceding permissions to list these storage account keys at scale, which can then be used to download the contents of the storage account services. Also, if you are wondering how a user with Contributor permissions is able to list objects in a storage account when using the Azure portal, it is due to the fact that the portal uses the permissions of the Contributor to access the storage account key, and then uses key-based authentication to display the data in the portal. Switching to Azure AD authorization will show that Contributor and Owner roles have no default dataActions permission.

In this next section, we will use the MicroBurst toolkit to gather these keys for storage accounts in the subscription.

# Hands-on exercise – Dumping Azure storage keys using MicroBurst

Here are the tasks that we will complete in this exercise:

+   - **Task 1:** Extract storage account keys at scale using MicroBurst.
+   - **Task 2:** Explore and download storage account data using Azure Storage Explorer.