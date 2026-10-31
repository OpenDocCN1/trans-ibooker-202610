
Double-clicking on any of these objects and then visiting the **Attribute Editor** tab will display details on password criteria for that object. There is no *need* to visit these objects from inside ADUC, as you can always make tweaks and changes to your fine-grained password policy from inside the ADAC console, but seeing these PSOs helps us to understand how Active Directory is storing and processing these policies:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_418_0.png)

Figure 9.29: Viewing password criteria outside of ADAC

## Advanced Threat Analytics – end of support

In my opinion, one of the coolest security features to have come out of Microsoft over the past handful of years is **Advanced Threat Analytics (ATA)**, and yet I have hardly heard anyone talking about it. Perhaps because they never went so far as to add it as a native feature built into the Windows Server OS. ATA is an on-premises software that rides on top of Windows to produce some amazing functionality. Essentially, what ATA does is monitor all of your Active Directory traffic and warn you of dangerous or unusual behavior in real time, immediately as it is happening.

Unfortunately, ATA reached the end of its mainstream support in January 2021. Extended support continues for five more years, however, and so I am choosing to retain information in this latest book edition because this is still a valid technology and will continue to exist in environments where it is installed for years to come. There is additional information at the end of this section regarding the ATA roadmap.

## What is (was) ATA?

The idea of ATA is pretty simple to understand and makes so much sense that we are all going to wonder why it took so many years for someone to come up with this idea. Active Directory has been around for a very long time, and attacks against your environment utilizing user accounts for just as long. Discovering and troubleshooting attacks within the directory is complicated and messy; there is so much information being logged all the time inside AD that it is almost impossible to catch or track bad guys as they do their worst. Following some form of attack, you can certainly pore over log data from multiple domain controllers and piece together some semblance of what happened, but being able to identify that happening *as it is happening* is just a pipe dream. Or is it…?

ATA is an advanced form of Active Directory monitoring that uses machine learning. This is the coolest part of ATA. You configure your network so that all of the traffic flowing in or out of your domain controllers also lands in the ATA system. The most secure way to accomplish this is at the networking level, establishing port mirroring so that all of the domain controller packets also make their way to ATA, but at a level that an attacker would not be able to see.

This way, even if someone nefarious is inside your network and is on the lookout for some kind of protections working against them, ATA remains invisible to their prying eyes. However, port mirroring that traffic is something that smaller companies may not be able to do or may be too complex for an initial setup, and so a second option exists to install an ATA lightweight agent right onto the domain controllers themselves. This agent then sends the necessary information over to the ATA processing servers.

In either case, those ATA processing servers receive all of this data and start finding patterns. If Betty uses a desktop computer called **BETTY-PC** and a tablet called **BETTY-TABLET**, ATA will see that pattern and associate her user account with those devices. It also watches for her normal traffic patterns. Betty usually logs in around 8 a.m. and her traffic usually stops somewhere around 5 p.m. She typically accesses a few file servers and a SharePoint server. After a week or so of collecting and monitoring data, ATA has a pretty good idea of Betty's standard MO.

Now, one night, something happens. ATA sees a bunch of password failures against Betty's account. That in itself might not be something to get too excited about, but then all of a sudden Betty logs into a terminal server that she doesn't typically access. From there, her credentials are used to access a domain controller. Uh oh, this clearly sounds like an attack to me. With the tools built into Active Directory that we currently have at our disposal, what do we know? Nothing, really. We might see the password failures if we dig into the event logs, and based on that we could try poking around other servers' event logs in order to find out what that account was accessing, but we really wouldn't have any reason to suspect anything, nor any prompts or warnings that anything was happening whatsoever. This could be the beginning of a very large breach, and we would never see it. Thankfully, ATA knows better.

The management interface for ATA is like a social media feed, updated almost in real time. During the events I have just laid out, if we had been looking at the ATA media feed, we would have seen all of these items happen, as they happened, and it would be immediately obvious that someone compromised Betty's account and used it to gain access to a domain controller. There has never been a technology that watches Active Directory traffic so intensely, and there has certainly never been anything that learns patterns and behavioral diversions like this. It is truly an amazing technology, and I don't say that only because I happen to know the guys who built it. But since I do, I can tell you that they are brilliant, which is already pretty obvious since Microsoft scooped them up.

Even though this technology is now being morphed into something at the cloud level, let's take a few minutes and review a couple of screenshots from the ATA interface so you have an idea of what this social media-style feed looks like. This screenshot was taken from a Microsoft demo where they purposefully stole the Kerberos ticket from a user, and then utilized it on another computer in order to access some confidential files that only Demi Albuz should have been able to access. While ATA did not stop this activity, it immediately—and I mean within seconds—alerted inside this feed to show the Pass-the-Ticket Attack:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_421_0.png)

## Identity Theft Using Pass-the-Ticket Attack

Demi Albuz's Kerberos tickets were stolen from CLIENT1 to CLIENT2 and used to access DC1 (CIFS).

Recommendations

- Disconnect the relevant computers from the network or move them into an isolated environment and start a forensics procedure by investigating: unknown processes, services, registry entries, unsigned files, and more
- Disable Demi Albuz's account

Figure 9.30: ATA identifying a pass-the-ticket attack

Here's another example where a user named Almeta Whitfield suddenly accessed 16 computers that she does not usually access, another big red flag that something is going on with her user account:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_422_0.png)

Figure 9.31: ATA alerts on abnormal behavior

## Azure ATP

While ATA is an on-premises solution, its replacement lives in the cloud. Azure Advanced Threat Protection is Microsoft's recommended replacement for ATA, though that announcement seems to have caused many grumblings among ATA users. The primary catch is licensing – Azure ATP is only available so far with Microsoft 365 E5 licenses, which carry a hefty price tag.

Documentation is still online for ATA, and further learning materials can be found here: https://docs.microsoft.com/en-us/advanced-threat-analytics/what-is-ata.

For more information on the new Azure ATP, the following link is a good starting point: https://techcommunity.microsoft.com/t5/security-compliance-identity/introducing-azure-advanced-threat-protection/ba-p/250332.

## General security best practices

Sometimes we need only to rely on ourselves, and not necessarily on functionality provided by the OS, to secure our systems. There are many common-sense approaches to administratorship (it's kind of fun being behind this keyboard: I get to make up new words all day long) that are easy to accomplish but are rarely used in the field.
