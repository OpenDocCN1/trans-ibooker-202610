
Figure 4.35 shows how this attack would work:

![](img/4fa338e84e0a590967a142dd798ba4e2_169_0.png)

Figure 4.35 – Exploiting a misconfigured service principal

The following are the details:

1. readeruser starts out with read-only permission to Azure resources.
2. The user registers a service principal for an application that they are building (by default, all member users of an Azure AD tenant can register an application). Also, because the user registered the application, they are automatically made the owner (ownership rights on a service principal account allow users to set credentials (certificates and secrets) that can be used to authenticate, as we just demonstrated).
3. An administrator with a higher privilege assigns the Contributor role or a higher-privileged role to the application that readeruser registered.
4. This gives a route for readeruser's credentials to be used to escalate privileges by adding a new secret to the app since they are the owner of the app.
5. The app ID and new secret can then be used to escalate privileges.

In the next hands-on exercise, we will use one of the powerful functions within PowerZure to identify the owners of app registrations and to add credentials to the apps.

## Hands-on exercise – escalating privileges using a misconfigured service principal

In this exercise, we will use functions in PowerZure to escalate our privileges from the Reader role in the test subscription to the Contributor role using a misconfigured service principal. Here are the tasks that we will complete in this exercise:

- **Task 1:** Enumerate application registration ownership using PowerZure.
- **Task 2:** Exploit service principal ownership to add a new secret.
- **Task 3:** Authenticate as a service principal to escalate privilege.

Let's begin:

1. Open a new PowerShell session (as an administrator) in your pentest VM. Import the PowerZure module using the following commands:

```powershell
PS C:\> cd C:\Users\$env:USERNAME\PowerZure
Import-Module .\PowerZure.ps1
```

You can see that the current user is assigned the Reader role in the subscription. You can also validate this with the Show-AzureCurrentUser function of PowerZure:

![](img/4fa338e84e0a590967a142dd798ba4e2_170_0.png)

Figure 4.36 – Example of an authenticated Reader user

2. Let's verify whether the current user is assigned as the owner of a service principal using the following command:

```powershell
PS C:\> Get-AzureAppOwner
```

### Important note

Although there is explicit monitoring for app registration modification, it can be difficult to detect using manual analysis especially in an environment with potentially thousands of service principals. Also, the adding of a credential does not impact the existing secrets configured for the service principal. This means that any application using the service principal's existing secret will continue to work without interruptions.

From a blue team or defense perspective, Azure AD audit logs will record these events with the activity type of **Update application – Certificates and secrets management**. Azure Resource Graph and Application Change Analysis can also be helpful when investigating.

4. Switch to WSL and authenticate as the service principal using the following commands. Replace `APP_ID` with the application ID that you made a note of in *Step 3*. Replace `TENANT_ID` with the tenant ID that you made a note of in *Step 3*. Press *Enter*:

```bash
PS C:\> bash
pentestadmin@PentestVM:~# sudo su -
root@PentestVM:~# az login --service-principal --username APP_ID --password myPassword456 --tenant TENANT_ID
```

Here is a screenshot of the command and its output:

![](img/4fa338e84e0a590967a142dd798ba4e2_172_0.png)

Figure 4.39 – Example Az CLI login with the credentials

You are now logged in as the service principal. Next, we will verify the permissions that this service principal has.

5. Verify the role assignment and scope of the service principal using the following command. Replace APP_ID with the application ID that you made a note of in Step 3:

```bash
root@PentestVM:~# az role assignment list --assignee APP_ID --include-groups --include-inherited --query '[].{username:principalName, role:roleDefinitionName, usertype:principalType, scope:scope}'
```

You can see that this service principal is assigned the Contributor role at the subscription scope. Awesome! We have found a route to a role assignment that gives us more permissions than that of the current user!

![](img/4fa338e84e0a590967a142dd798ba4e2_173_0.png)

Figure 4.40 – Gathering the assigned roles for the affected service principal

While the Reader roles (Global Reader in Azure AD and Reader in Azure RBAC) normally do not have ownership permissions on service principals, remember that member users in an Azure AD tenant have permission to register applications and they are automatically made the owner of applications that they registered. Also, there are occasional situations where ownership rights are given to users. Since these ownership rights could pop up at any role level, we thought it would be good to address them early on.

One permission that is always available for the Reader role is the ability to connect to and pull images from Azure Container Registry (ACR). These rights don't allow readers to make changes to the images, but it does allow them to review the images.

## Reviewing ACR

For many Azure services, Microsoft usually keeps a separation between management plane access and data plane access. This means that explicit permissions beyond the typical management roles are needed to access data stored within services. This architecture was not implemented for the ACR service. It is an interesting architecture choice, but Azure users with Reader permissions on any Azure container registries have rights to connect to the container registry and access images. These users do not have any rights to push modified images back to the registry, but they are able to download and run container images locally. This allows us, as attackers, to review the contents of the images and potentially find issues in the application code, access secrets used by the application, and pivot with those secrets into other applications and services.

The process to do this is very simple. As an authenticated user, with Reader or higher permissions on the registry, we can generate a Docker login from the Azure CLI. The next exercise walks you through how this is done.

### Hands-on exercise – hunting for credentials in the container registry

In this exercise, we will pull container images from an Azure container registry to hunt for credentials that may be embedded in them. As for tools, we will be using the Azure CLI and Docker for the examples in this exercise.

Here are the tasks that we will complete in this exercise:

- **Task 1:** Authenticate to the Azure environment from WSL using the Reader role.
- **Task 2:** Enumerate the container registries in the subscription.
- **Task 3:** Generate a Docker login for the registry and authenticate in WSL.
- **Task 4:** Enumerate the container images and tags in the registry.
- **Task 5:** Pull the images and hunt for credentials in them.

Let's get started:

1. In your pentest VM, open a PowerShell console as an administrator, switch to WSL, and authenticate to Azure using the readeruser account. Replace `<DOMAIN>` with the domain name from the output of the setup script:

```powershell
PS C:\> bash
pentestadmin@PentestVM:~# sudo su -
root@PentestVM:~# az login -u readeruser@<DOMAIN> -p myPassword123
```

Here is a screenshot of the command output:

![](img/4fa338e84e0a590967a142dd798ba4e2_175_0.png)

Figure 4.41 – Example Az login with the reader account

2. List the container registries in the subscription using the following command:

```bash
root@PentestVM:~# az acr list -o table
```

You can see that there is a container registry with the name format acrXXXX (where XXXX is a randomly generated number during deployment). Make a note of this name:

![](img/4fa338e84e0a590967a142dd798ba4e2_175_1.png)

Figure 4.42 – Example generated ACR

3. Install Docker on WSL using the following command:

```bash
root@PentestVM:~# apt update && apt install docker.io -y
```
