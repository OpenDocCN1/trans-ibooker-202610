Next, we will go through the process of using this feature to access a VM as a local administrator.

# Hands-on exercise – exploiting the password reset feature to create a local administrative user

In this exercise, we will exploit the password reset feature for Azure VMs to create a local administrative user that can be used to connect to the VM, where we will hunt for credentials on it. Here are the tasks that we will complete in this exercise:

+   1. Open PowerShell on your pentest VM.
+   2. In the PowerShell console, use the following command to authenticate to Azure:
+   3. When prompted, enter the Contributor user credentials that you obtained from the script output in the first exercise in this chapter:

```
Connect-AzAccount
```

Here is a screenshot of this:

![](bbox=[0.15, 0.41, 0.85, 0.52])

Figure 5.7 – Authenticating to the subscription

![](bbox=[0.28, 0.61, 0.73, 0.88])

Figure 5.8 – Connect-AzAccount authentication prompt