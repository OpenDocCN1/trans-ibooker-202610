You should now have shell access to the Linux VM using the credentials exposed in the resource group deployment output. From here, we can look for opportunities to move laterally or escalate privileges. In this case, let's check to see whether this Linux VM has a managed identity associated with it. As part of checking for the managed identity, we will be querying the Azure Instance Metadata Service (IMDS).

> **Important note**
> The IMDS is a REST API endpoint that is available at a non-routable IP address (169.254.169.254) from within a VM instance. It can be used to obtain information about the instance platform configuration and to request access tokens if the instance is assigned a managed identity. It requires a specific header (Metadata:true) to be present, so this typically requires you to make requests to the service via command execution.
> 
> You can learn more about it from this documentation: https://docs.microsoft.com/en-us/azure/virtual-machines/windows/instance-metadata-service.

Remember that a managed identity is an identity that can be associated with an Azure resource and granted permissions in Azure (you can review Chapter 1, Azure Platform and Architecture Overview, for more information).

4. Review the Azure VM instance metadata to obtain more information about how the VM is configured in Azure. You can do this with the following commands:

```
azureadmin@LinuxVM:~# sudo su -
root@LinuxVM:~# apt update -y
root@LinuxVM:~# apt install jq -y
root@LinuxVM:~# curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance?api-version=2020-09-01" | jq
```