
## Steps:

- Rule Type
- Program
- Protocol and Ports
- Scope
- Action
- Profile
- Name

To which ports and protocols does this rule apply?

Protocol type:
Protocol number:
Local port:
Remote port:
Internet Control Message (ICMP) settings:

Figure 9.15: Applying the rules to specific protocols

For the rest of the ICMP rule creation, follow the same procedures outlined when we created the RDP rule, choosing to either allow or block this traffic, and for which firewall profiles. Once finished, your new ICMPv4 rule is immediately enacted, and if you have configured an Allow rule, your new server will now successfully respond to ping requests:

```
PS C:\Users\administrator.CONTOSO> ping ral

Pinging ral.contoso.local [10.10.10.13] with 32 bytes of data:
Reply from 10.10.10.13: bytes=32 time<1ms TTL=128
Reply from 10.10.10.13: bytes=32 time<1ms TTL=128
Reply from 10.10.10.13: bytes=32 time<1ms TTL=128
Reply from 10.10.10.13: bytes=32 time<1ms TTL=128
```

Figure 9.16: Pinging your server and now receiving an ICMP response

If ever you need to modify a rule or dig into more advanced properties of a firewall rule, back at the **Inbound Rules** screen you can right-click on any individual firewall rule and head into **Properties**. Inside these tabs exists the opportunity to modify any criteria about the rule. For example, you could accommodate additional ports, you could modify which firewall profiles it applies to, or you could even restrict which specific IP addresses this rule applies to by use of the **Scope** tab.

This enables you to apply your firewall rule only to traffic coming or going from a specific portion of your network, or a certain subset of machines. For example, here I have modified my **Scope** tab to reflect that I only want this firewall rule to apply to traffic that is coming in from the 192.168.0.0/16 subnet:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_400_0.png)

Figure 9.17: Applying the firewall rule to specific IP addresses or subnets

## Managing WFAS with Group Policy

Managing firewall rules on your servers and clients can be a huge step toward a more secure environment for your company. The best part? This technology is enterprise class and free to use since it's already built into the OSes that you use. The only cost you have associated with firewalled at this level is the time it takes to put all of these rules in place, which would be an administrative nightmare if you had to implement your entire list of allows and blocks on every machine individually.

Thank goodness for Group Policy. As with most settings and functions inside the Microsoft Windows platform, setting up a firewall policy that applies to everyone is a breeze for your domain-joined machines. You can even break it up into multiple sets of policies, creating one GPO that applies firewall rules to your clients and a separate GPO that applies firewall rules to your servers, however you see fit. The point is that you can group many machines together into categories, create a GPO ruleset for each category, and automatically apply it to every machine by making use of Group Policy's powerful distribution capabilities.

You are already familiar with creating GPOs, so go ahead and make one now that will contain some firewall settings for us to play with. Link and filter the GPO accordingly so that only the machines you want to have the settings will actually get them. Perhaps a good place to start is a testing OU, so that you can make sure all the rules you are about to place inside the GPO work well together and with all of your other existing policies, before rolling the new policy out to your production workforce.

Once your new GPO is created, right-click on it from inside **Group Policy Management Console** and click on **Edit...**:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_402_0.png)

Figure 9.18: Using Group Policy to centrally manage Windows Firewall rules

Now that we are looking at the insides of this new GPO, we just need to figure out where the correct location is for us to create some new firewall rules. When you are looking inside the rules on the local machine itself, everything is listed under a **Windows Defender Firewall with Advanced Security** heading, and that is located at **Computer Configuration** | **Policies** | **Windows Settings** | **Security Settings** | **Windows Defender Firewall with Advanced Security** | **Windows Defender Firewall with Advanced Security**:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_403_0.png)

Figure 9.19: Location of firewall rule settings inside a GPO

As you can see, this is also the place to go when you want to make sure that particular firewall profiles, or Windows Firewall as a whole, are specifically turned on or off. So, this is the same place that you would go if you wanted to disable Windows Firewall for everyone.

By clicking on the Windows Defender Firewall Properties link shown in Figure 9.19, you can determine the status of each firewall profile individually:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_404_0.png)

Figure 9.20: Separate tabs to modify each firewall profile's behavior

Once you are finished setting your profiles according to your needs, click on OK, and you find yourself back at the WFAS part of the GPO. Just like inside the local WFAS console, you have categories for Inbound Rules and Outbound Rules. Simply right-click on Inbound Rules and click on New Rule... to get started with building a rule right into this GPO. Walk through the same wizard that you are already familiar with from creating a rule in the local WFAS console, and when you are finished, your new inbound firewall rule is shown inside the GPO:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_405_0.png)

Figure 9.21: A successfully implemented firewall rule

Faster than you can say Jack Robinson, this new firewall rule is already making its way around Active Directory and installing itself onto those computers and servers that you defined in the policy's links and filtering criteria.

## Encryption technologies

An idea that has taken a fast step from something the big organizations are playing around with to everybody needs it is the use of encryption. Most of us have been encrypting our website traffic for many years by using HTTPS websites, but even in that realm there are surprising exceptions, with a lot of the cheap web-hosting companies still providing login pages that transmit traffic in clear text. This is terrible, because with anything that you submit over the internet now using regular HTTP or an unencrypted email, you have to assume that it is being read by someone else. Chances are you are being paranoid and nobody is actually intercepting and reading your traffic, but you need to know that if you are accessing a website that says HTTP in the address bar, or if you are sending an email from any of the free email services, any data that is being entered on that web page or in that email can easily be stolen by someone halfway around the world. Data encryption is an absolute requirement for corporate information that needs to traverse the internet; though at the same time that I say that, the back of my mind is telling me that the vast majority of companies are still not using any kind of encryption technology on their email system, and so that is still a potential disaster waiting to happen for many.

While we are getting better and better at protecting internet browser traffic, we traditionally are still not paying a lot of attention to data that is *safe* within the walls of our organization. The bad guys aren't dumb, though, and they have a very large toolbox of tricks to socially engineer their way into our networks. Once inside, what do they find? In most cases, it's a big free-for-all. Get a hold of one user account or one computer and you've got keys to a large part of the kingdom. Fortunately, there are several technologies built into Windows Server 2019 that are designed to combat these intrusions and protect your data even while it sits within the four walls of your datacenter. Let's look at some information on them so that you can explore the possibility of using these encryption technologies to further protect your data.

## BitLocker and the virtual TPM

**BitLocker** is a technology that has become pretty familiar to see on our client systems within corporate networks. It is a full-drive encryption technology, giving us the advantage of making sure our data is fully protected on laptops or computers that might be stolen. If a thief gets their hands on a company laptop, claws out the hard drive, and plugs it into their computer...sorry Charlie, no access. The entire volume is encrypted. This makes all kinds of sense for mobile hardware that could be easily lost or stolen. But in the beginning stages of this technology, there was never real consideration of using BitLocker to protect our servers.

With the escalated adoption of cloud computing resources, suddenly it makes much more sense to want BitLocker on our servers. More particularly when talking about the cloud, what we really want is BitLocker on our **Virtual Machines (VMs)**, whether they be client or server OSes. Whether you are storing your VMs in a true cloud environment provided by a public cloud service provider or you are hosting your own private cloud where tenants reach in to create and manage their own VMs, without the possibility of encrypting those virtual hard drives—the VHD and VHDX files—your data is absolutely **not** secure. Why not? Because anyone with administrative rights to the virtualization host platform can easily gain access to any data sitting on your server's hard drives, even without any kind of access to your network or user account on your domain. All they have to do is take a copy of your VHDX file (the entire hard drive contents of your server), copy it to a USB stick, bring it home, mount this virtual hard disk on their own system, and bingo—they have access to your server hard drive and your data. This is a big problem for data security compliance.

Why has it historically not been feasible to encrypt VMs? Because BitLocker comes with an interesting requirement. The hard drive is encrypted, which means that it can't boot without the encryption being unlocked. How do we unlock the hard drive so that our machine can boot? One of two ways.
