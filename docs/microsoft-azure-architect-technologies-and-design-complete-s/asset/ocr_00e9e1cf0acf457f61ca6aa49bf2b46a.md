The prefix is based on a bit mask where each of the four octets in the IP address is converted to bits, and the chosen route map entry is the one that has the most overlap of the CIDR range. /8 means that the first eight bits of the octet are the same, whereas /16 means the first 16 are equal. To get much further into this complex area, it is time to create an Azure VM. Although we get into the details of computing in the next chapter, it will be hard to provide good examples without a VM. In Exercise 3.3, you will create an Azure VM to use for the analysis of the networking infrastructure created around it.

# Azure Resources Have an Associated Cost

This is the first time so far that you will create an Azure resource that will have a direct cost. Even if you never use it, when you complete step 12, there will be a charge. Take note of this because as we progress, we will create resources that have a cost. I will not notify you each time. At the end of each exercise, you may consider deleting the resource group; however, examples from this point will likely build upon each other and call upon resources created in previous exercises.

# EXERCISE 3.3

# Creating an Azure Virtual Machine

+   1. Log in to the Azure Portal at portal.azure.com.
+   2. Enter Virtual Machine into the Search Resources, Services, And Docs box at the top of the portal. Select Virtual Machines + Add.
+   3. On the Basics tab, select the subscription and resource group into which you want the Azure VM to be placed (for example, CSHARPGUITAR-DB3-RG).
+   4. Provide a name for the Azure virtual machine (for example, CSHARPGUITAR-VM). Remember that VM names are limited to 15 characters.
+   5. Select the region; it can be the same as the one for the resource group, but it doesn’t have to be.
+   6. Leave the availability options at the defaults and choose Windows Server 2019 Data-center as the image. Leave Size at the default.
+   7. Enter a username and password and confirm the password.
+   8. Select the Allow Selected Ports radio button and enable all ports: HTTP, HTTPS, SSH, and RDP.
+   9. Specify that you do not already have a license; if you do, then select Yes. Click the Next: Disks button.
+   10. Leave the defaults and click the Next: Networking button.