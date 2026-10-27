
It should be noted that many years ago, Windows servers had IIS installed by default, but that is no longer the case. When assessing the security of a server, you should review what is installed, and if IIS is installed because someone installed it, you should verify it is needed—and if not, uninstall it.

If the web server is not to be accessed by the public, then ensure that you disable anonymous access (which allows anyone to access the server) and enable authentication on the server. If you are working with confidential data on the web site, be sure to secure the web traffic with SSL/TLS.

## DNS Servers

To harden your DNS server, you should apply all the steps from the previous “All Servers” section, but in addition you should limit zone transfers on the DNS server. Zone transfers occur when the primary DNS server sends the DNS data to the secondary DNS server. If a hacker obtains the DNS data by doing a zone transfer, they will know the IP addresses used by your different systems.

In the properties of your DNS server, you should see an option where you can limit which systems can receive zone transfers from your DNS server. You normally would make sure only your DNS servers are listed here. Figure 6-11 displays how to limit zone transfers on a Microsoft DNS server.

### FIGURE 6-11

Controlling DNS zone transfers

![](img/3e3af33e2c753e920c09ecd40b5a457a_294_0.png)

You can also block TCP 53—the port used by zone transfers—at your firewall so that someone outside the network cannot perform zone transfers. This may not be an option if you have a secondary DNS server at another location across the Internet.

## EXERCISE 6-4

### Limiting DNS Zone Transfers

In this exercise you ensure that DNS zone transfers on your Windows server are limited to only to your secondary DNS servers.

- 1. Ensure that you have the ServerA and Windows 10 VMs running. Log out of each system.
- 2. Log on to the ServerA VM. From the Start menu, choose DNS to launch the DNS management console.
- 3. Expand ServerA on the left and then expand Forward Lookup Zones. Right-click your DNS zone and choose Properties. For example, right-click certworld.loc and choose Properties.
- 4. In the zone Properties dialog box, choose the Zone Transfers tab.
- 5. Check the Allow Zone Transfers check box and then select the Only to the Following Systems radio button.
- 6. Type the IP address of your Windows 10 client and then click the Add button to add the Windows 10 system as a system that can receive zone transfers from this DNS server. For this exercise, pretend that the Windows 10 system is your secondary DNS server.
- 7. Click OK.
- 8. Go to the Windows 10 VM and start a command prompt.
- 9. Type the following commands into the command prompt (pressing ENTER after each line). Your domain name, for example, might be certworld.loc.
- 10. You should see the DNS data on the screen. If you try the same commands from a different system, you should get a “query refused” error because zone transfers are only authorized for the one IP address.

```
nslookup
ls certworld.loc
```

## DHCP Servers

You can help secure your DHCP servers by applying all the hardening techniques discussed in the “All Servers” section, but you can also apply a few additional precautions. First, when you create a scope (IP addresses for the DHCP to give out), create only enough addresses for

what is needed on your network. For example, if you have 20 systems that need addresses, you should create a scope that gives out only 20 addresses. The benefit is that if an unauthorized individual connects to the network (system #21), there won’t be any available IP addresses in the scope for the DHCP server to give to the client system. The result is that the system will be unable to communicate on the network.

On top of having only enough addresses in the scope, you can also implement address reservations, where each of the 20 addresses you created in the scope has a MAC address associated with it. The benefit is that each address will be assigned only to the network card that has the MAC address associated with it.

## SMTP Servers and FTP Servers

With both SMTP servers and FTP servers, you should apply the hardening techniques discussed in the “All Servers” section, but again, you should also take your security practices a bit further.

When dealing with SMTP servers, make sure you are protecting the server with a firewall, and open TCP port 25 to allow the SMTP server to pass through the firewall to reach the server. You also need to ensure that SMTP relaying is disabled on the SMTP server. SMTP relaying is the concept that your SMTP server forwards any SMTP message not destined for it onto the destination server. Relaying is a bad thing because a hacker could send spam messages to your server that your server then forwards to the destination. From the destination point of view, your server is doing the spamming!

With FTP servers, make sure you limit who can upload files to the FTP server, and maybe allow only files to be downloaded from the server. You also should decide whether to allow anonymous access to the FTP server or to force people to authenticate to the server. If you do force authentication, be sure that you do it in a secure way.

## Common Mitigation Strategies

You have learned about some common strategies to reduce (mitigate) the likelihood that a security incident occurs. Here is a summary of those points for your review for the certification exam:

- Network segmentation Segmenting the network allows you to control which systems can communicate with one another on the network. You can use VLANs and IP subnets to create the network segments. In certain common scenarios, you will want to segment the network in order to separate the production systems from customer systems—for example, hotel guests or student systems in a classroom environment. You also want to segment top-secret systems from other classified systems in a highly secured environment.

- **Security layers** Implement security in layers by patching systems, using firewalls and network segmentation, and hardening systems.
- **Application firewalls** Use application layer firewalls, as they allow you to filter traffic based on payload data in the packet.
- **Manual updates** Perform updates of systems on a regular basis to ensure that any known security issues in the system are patched.
- **Firmware version control** Be sure to apply firmware updates to devices such as routers and switches on a regular basis.
- **Wrappers** When possible, you should use wrappers, such as TCP wrappers, to create connections to specific services on a system. For example, in Linux you can use the `/etc/hosts.allow` file to specify systems that can access services such as the FTP service and HTTP service.
- **Control redundancy and diversity** Controlling redundancy and diversity is the security principle of ensuring that you diversify the products used to create layers of security. For example, if you decide to have multiple firewalls between your internal network and the Internet, you could create multiple layers of firewalls that a hacker will need to compromise in order to get access to the internal network. If you used the same firewall product at each layer, when the hacker figures out how to compromise the first firewall, they can easily bypass the remainder. If you purchase different firewall products to use at each layer, each will have different vulnerabilities, and the hacker will need to take the time to figure out how to get past each one instead of using knowledge gained from the first one.

## CERTIFICATION SUMMARY

In this chapter you learned about methods to mitigate common security threats to systems and applications. The following key points should be remembered when preparing for the Security+ certification exam:

- All systems should be hardened, which is the removal of unnecessary software and the disabling of unnecessary services.
- When hardening a system, investigate each of the services running on the system, and determine if the service is needed. If the service is not needed, it should be disabled.
- As part of hardening a system, make sure you disable unnecessary accounts and rename the default accounts such as “administrator.” Also be sure to patch the system on a regular basis.
