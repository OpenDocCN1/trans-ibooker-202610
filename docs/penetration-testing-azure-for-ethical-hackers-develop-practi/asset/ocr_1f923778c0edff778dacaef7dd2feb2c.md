# Introducing Lava

Lava is a Python-based Microsoft Azure exploitation framework. It has multiple built-in modules that can be used to exploit various misconfigurations in Azure. One of the authors of this book contributes regularly to the project. We will be using it to exploit privileged VMs for role escalation in the next hands-on exercise.

**Lava information**
**Creator**: Matt Rotlevi (Twitter: @mattrotlevi)
**Open Source or Commercial**: Open source project
**GitHub Repository**: https://github.com/mattrotlevi/lava
**Language**: Python

# Hands-on exercise – exploiting privileged VM resources using Lava

In this exercise, we will download Lava in WSL on the pentest VM, use it to find privileged IaaS resources in an Azure subscription, and then escalate privileges from the Contributor role to the Owner role.

Here are the tasks that we will complete in this exercise:

+   - Authenticate to Azure using the Azure CLI tool in WSL.
+   - Download Lava in WSL.
+   - Use a Lava module to obtain a list of privileged VMs in a subscription.
+   - Use a Lava module to exploit a VM with the Owner role assignment.

Let's begin!

+   1. Open PowerShell on your pentest VM.
+   2. In the PowerShell console, type bash to switch to Windows Subsystem for Linux.
+   3. Use the following command to switch to the root user. This is needed for the installation task that we want to perform. Enter the administrative password when prompted:

```
azureuser@PentestVM:~# sudo su -
```