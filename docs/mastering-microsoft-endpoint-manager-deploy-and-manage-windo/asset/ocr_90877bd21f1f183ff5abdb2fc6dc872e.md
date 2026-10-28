# Domain-name resolution

A very common error that happens is customers configure their Azure environment, create an Azure **virtual network (VNet)**, but forget about changing the **DNS servers** setting on the VNet to their own domain server.

Always make sure to change your **Domain Name System (DNS)** server to **Custom** and enter the **Internet Protocol (IP)** address of your own DNS service environment that can resolve your **Active Directory Domain Services (AD DS)** domain.

The enrollment of your cloud PCs will fail. The good thing is that our Watchdog service, explained earlier in *Chapter 3, Introducing Windows 365*, will notify you about it as well.

The **DNS servers** setting is shown in the following screenshot:

![](bbox=[0.117, 0.339, 0.852, 0.704])

Figure 17.4 – DNS servers

Let's continue with the networking segment of troubleshooting and talk more about the different service **Uniform Resource Locators (URLs)** we have to check and whitelist for traffic.