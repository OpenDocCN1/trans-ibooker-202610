
On the following screen of this wizard, we define all of the relationship parameters between the two DHCP servers. As you can see in our next screenshot, this is the screen where you define simple things that don't have too much impact such as Relationship Name and creating a Shared Secret that the two DHCP servers will use to interact with each other, but this screen is also where you create the definition of which type of failover is happening for this scope. We already talked about Load balance mode and Hot standby mode, and here is where you would additionally define, in Load balance mode, what weights to assign the two servers for establishing that load balancing.

Unless you have a particular reason to deviate, the default settings are generally what you want. Make sure that Maximum Client Lead Time remains configured at one hour or more. You may bump that number down smaller for testing failover in your network, but in a production environment one hour is the general recommendation:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_197_0.png)

Figure 4.24: Configuring Maximum Client Lead Time to one hour or more

That's it! Click Next a couple more times, and your DHCP failover configuration will be pushed out to the second DHCP server. Now, if one of these servers were to go offline or have some kind of issue, DHCP clients coming in fresh or whose DHCP leases are expiring will be able to acquire a lease from either DHCP server setup for failover.

Now that DHCP failover has been established, you would sort of expect something inside the DHCP management console to have changed to visually indicate to us that there is failover configured on the scope, but that is not really the case. Everything in here looks exactly the same as before failover creation. I point this out to let you know that during future visits to this console, or if you are a new admin to an existing infrastructure and want to quickly identify whether or not DHCP failover is established in your environment, you can find this information by right-clicking on the scope. In the menu list that is displayed with that right-click, you will now find an option called Deconfigure Failover, which implies that failover is currently configured for this scope. Additionally, if you head into the properties of that scope, you will find a Failover tab that displays all pertinent information about DHCP failover:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_198_0.png)

Figure 4.25: Deconfiguring failover

One last verification, let's log into DC2 and see what the DHCP management console looks like now! Remember that previously the only thing I have done on DC2 is to install the DHCP role and to authorize this server. I never even opened up the DHCP management console on that server. If I do so now, you can see that my 10.10.10.0 scope has been replicated over to this server, including the current address lease information!

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_199_0.png)

Figure 4.26: Replicating the 10.10.10.0 scope to a DC2

## IPAM

The IP Address Management (IPAM) feature built into Windows Server 2019 is overlooked by many server administrators because it is a feature and not a full-blown role inside Windows. IPAM is a technology that allows centralized monitoring and management of DHCP and DNS in your environment. If all of your infrastructure is sitting inside one building, it is easy enough to simply use the DNS and DHCP management tools from any server or workstation in your network and have full control over both of those technologies. But how about larger and enterprise-class networks that span many locations, each with its own sets of DNS and DHCP servers? IPAM is useful for gathering up all of those differing namespaces and scopes and providing access to them from one interface.

Let's install the IPAM feature in my lab so you have an idea of where to start, should you choose to employ this feature. Begin by choosing a server upon which you want to install IPAM, and simply walk through the Add Roles and Features Wizard to add the feature called IP Address Management (IPAM) Server.

> Note: It is not recommended to install IPAM onto a domain controller, or onto a DHCP server as that will hinder its ability to acquire information from these systems.

Now that the feature is installed, launching Server Manager will display a new section in the left window pane called IPAM. Go ahead and click on IPAM, and you are now presented with a set of six tasks that need to be accomplished in order to use IPAM:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_200_0.png)

Figure 4.27: IPAM server tasks

The first step is already finished, we are connected to the IPAM server. In my lab, I installed IPAM on my CA1 server, but not for any particular reason. You could dedicate a server to this task or install the feature onto an existing server.

Moving on to Step 2, Provision the IPAM server, you will have a chance to read over some good descriptive text about how IPAM plans to interact with your servers. You have the option of manually configuring each server (who's going to do that?) but clearly the way Microsoft expects most of us to centrally roll this out is through the use of Group Policy. We discuss Group Policy in the next chapter of this book, but for the purpose of setting up IPAM we can pretty much just flow through these configuration wizards, selecting the default settings, and understand that under the hood this console is establishing GPOs for us, which will then be pushed down to the servers in our network to configure them for reporting into IPAM.

The only selections that need to be made inside *Step 2* are what database for IPAM to utilize, and which provisioning method should be used to send IPAM settings out to your servers. For the database, if you have a SQL server you may certainly use it to store IPAM information, but the default selection is **Windows Internal Database (WID)**, which is a built-in database platform used by many of the Windows Server roles and features. We will utilize WID for this new IPAM server, and as we already mentioned, the rolling out of IPAM settings is easily accomplished via Group Policy. On the screen where you select **Group Policy Based** as the provisioning method, you will also need to define a **GPO name prefix**. When this console creates the GPOs needed for IPAM to work, it will define names for the GPOs and those names cannot be altered, but what Microsoft allows here is for you to assign a prefix to those GPO names, so that when you later look inside the Group Policy management tool, all of the GPO configurations related to IPAM will be listed together, all having the same prefix that you define on this screen:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_201_0.png)

Figure 4.28: Selecting a provisioning method

You'll notice a little informational text in Figure 4.28, as well as on the summary screen when you finish Step 2, regarding a PowerShell cmdlet called Invoke-IpamGpoProvisioning. Common sense would tell us that after Step 2 in the IPAM implementation, you would move on to Step 3, right? Actually not in this case. Configuring server discovery (Step 3) is a step that we need to take, but at this point the IPAM console is waiting for those GPOs to be created so that they can be used and referenced during the rest of the setup process.

Pause on your work inside the IPAM steps, and open an administrative PowerShell prompt, making sure that you are logged in to your server with a domain admin account. Then simply run Invoke-IpamGpoProvisioning. This will ask you to key in the name of your domain, as well as that GPOPrefixName you specified during the wizard; be sure to type it in exactly the same!

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_202_0.png)

Figure 4.29: Running Invoke-IpamGpoProvisioning

You will be required to enter Y and press Enter three times to finish through this command. In reading the warning text that is being displayed, the same text three times in a row, this is asking you for confirmation simply because when these GPOs are being created, their security filtering settings are being modified away from default GPO security filtering behavior. We will explain GPO security filtering soon, but for IPAM's purposes, simply type Y and continue through these prompts. Now that Invoke-IpamGpoProvisioning has done its work, if we sneak a quick peek into Group Policy Management, you can see that there are three new GPOs now that were not there a minute ago:
