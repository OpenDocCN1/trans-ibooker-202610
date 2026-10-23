> **Important note**
> Microsoft has great documentation on how this feature works within Azure AD. Please refer to the following URL for additional information: https://docs.microsoft.com/en-us/azure/role-based-access-control/elevate-access-global-admin#how-does-elevated-access-work.

Now that we have seen how we can escalate rights, let's go through a practical example.

# Hands-on exercise – Preparing for the Global Administrator/Owner exploit scenarios

This hands-on exercise will prepare us for the rest of the exercises in this chapter. To follow along with the scenarios that we will cover in this chapter, you will need to set up a user with Global Administrator permissions in your Azure AD tenant and another user with the Owner permissions role in your Azure subscription. We have automated this using a PowerShell script that you can run from the Azure Cloud Shell. Here are the tasks that we will complete in this exercise:

+   1. Open a web browser and browse to the Azure Portal (https://portal.azure.com). Sign in with the azureadmin credentials.
+   2. In the Azure Portal, click on the Cloud Shell icon in the top-right corner of the Azure Portal. Select PowerShell:
+   3. In the PowerShell session within the Cloud Shell pane, run the following command to download a script to create a user account with Owner permissions, a second user with Global Administrator permissions, and set up the required vulnerable workloads:

![](bbox=[0.13, 0.56, 0.86, 0.62])

Figure 7.7 – Click the icon to open Cloud Shell

```
PS C:\> Invoke-WebRequest https://bit.ly/owner-scenarios -O owner-scenarios.ps1
PS C:\> Get-ChildItem
```