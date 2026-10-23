# # Reviewing ACR

For many Azure services, Microsoft usually keeps a separation between management plane access and data plane access. This means that explicit permissions beyond the typical management roles are needed to access data stored within services. This architecture was not implemented for the ACR service. It is an interesting architecture choice, but Azure users with Reader permissions on any Azure container registries have rights to connect to the container registry and access images. These users do not have any rights to push modified images back to the registry, but they are able to download and run container images locally. This allows us, as attackers, to review the contents of the images and potentially find issues in the application code, access secrets used by the application, and pivot with those secrets into other applications and services.

The process to do this is very simple. As an authenticated user, with Reader or higher permissions on the registry, we can generate a Docker login from the Azure CLI. The next exercise walks you through how this is done.

# ## Hands-on exercise – hunting for credentials in the container registry

In this exercise, we will pull container images from an Azure container registry to hunt for credentials that may be embedded in them. As for tools, we will be using the Azure CLI and Docker for the examples in this exercise.

Here are the tasks that we will complete in this exercise:

+   - **Task 1:** Authenticate to the Azure environment from WSL using the Reader role.
+   - **Task 2:** Enumerate the container registries in the subscription.
+   - **Task 3:** Generate a Docker login for the registry and authenticate in WSL.
+   - **Task 4:** Enumerate the container images and tags in the registry.
+   - **Task 5:** Pull the images and hunt for credentials in them.

Let's get started:

+   1. In your pentest VM, open a PowerShell console as an administrator, switch to WSL, and authenticate to Azure using the readeruser account. Replace `<DOMAIN>` with the domain name from the output of the setup script:

```
```powershell
PS C:\> bash
pentestadmin@PentestVM:~# sudo su -
```
```