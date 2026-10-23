In the previous exercise, you created an Azure virtual machine using a public Azure image and looked through other available images.

# Azure Bastion

This Azure feature is new and available for making a remote connection to an Azure virtual machine from within the portal. There may be some organizations that cannot enable ports 3389 or 22, which provide the ability to connect via Remote Desktop (RDP) or SSH to a machine due to security compliance regulations. Additionally, those connections, when made from an on-premise client to a cloud-hosted server, would happen across the internet, which may not be secure enough for some enterprises. By using Azure Bastion to connect to the cloud-hosted servers, you only need port 443 opened, and the connection remains within the secure Azure network infrastructure. Read more about Azure Bastion here:

```
docs.microsoft.com/en-us/azure/bastion/bastion-create-host-portal
```

# Using Images

An image in the Azure VM context is similar to the definition of an image in the container context discussed in the previous section. An image is a template that defines the environment requirements in which it will run. From an Azure VM perspective, the big difference is that the operating system is part of the image definition. Review Figure 4.7 if you need a refresher on the differences. There are numerous tools that help you to create an image for deployment to an Azure VM, for example, Azure VM Image Builder, System Preparation Tool (SYSPREP), Disk2VHD, snapshots, and export from VMware, VirtualBox, from an already created Azure VM or Hyper-V (VHDs) from the portal. We won’t cover all those tools and options in detail now, but an interesting one is snapshots. We have discussed a bit about managed disks, specifically the OS disk. It is possible to create a snapshot of that disk and then navigate to the disk in the portal where you will find a button on the Overview tab that allows you to export it and then create a VM from that snapshot; there are similar capabilities in on-premise virtualization software as well. The simplest way to create an image of an existing Azure VM is described in Exercise 4.6. You would want to do this after the VM that you provisioned is complete and ready to be shared and used in production. In the following exercise, you will create an image from the VM created in the previous exercise and use it to deploy a new VM.

# EXERCISE 4.6

# Creating an Azure VM Custom Image

+   1. Connect via RDP or Bastion to the VM you created in the previous exercise and run C:\Windows\System32\Sysprep\sysprep.exe. Enter the settings shown in Figure 4.21. Do not restart the VM; it will shut down automatically.