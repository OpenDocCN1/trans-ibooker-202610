The most common methods of volume activation require that devices be connected to an organization's network or connected via the VPN to *check in* from time to time with the organization's activation service to maintain their licenses. However, when people work from home and off the corporate or school networks, their devices' ability to receive or maintain activation is limited.

There are several ways to do a Windows activation:

+   - A key management service
+   - A multiple activation key
+   - An Active Directory-based activation
+   - A Windows 10 subscription activation

# Using Windows via a Windows 365 cloud PC

The preceding options to activate Windows 10 Enterprise are built into a Windows 365/cloud PC. If you are going to self-manage and deploy virtual desktops on Azure, this means that Windows 10 Enterprise will be automatically activated via Azure **Key Management Service (KMS)**.

# Azure KMS – cloud PC/Windows 365/AVD

If your Windows desktops are virtual and run on Azure such as with **Azure Virtual Desktop (AVD)**, you need to connect to the Azure `kms.core.windows.net:1688` KMS server in order to activate your virtual machine. This happens automatically when using images from the Azure image gallery. Make sure that your Azure **virtual network (vNET)** can connect to this address, and configure your firewall policies and rules accordingly. You will learn more about this later on in the cloud PC/Windows 365 and requirements chapters.

Windows 11 will be added later this fall to the offering once the OS is made generally available. More on Windows 11 is covered later on in the book.