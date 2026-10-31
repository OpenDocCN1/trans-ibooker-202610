
## Summary

The nature of the world today demands that most companies enable their employees to work from wherever they are. Working from home has become normal over the past handful of years, and specifically in this last year, with a worldwide pandemic, we have seen staggering increases in the percentage of employees who work outside of an office building. Companies need a secure, stable, and efficient way to provide access to corporate data and applications for these mobile workers. The Remote Access role in Windows Server 2019 is designed to do exactly that. With three different ways of providing remote access to corporate resources, IT departments have never had so much remote access technology available at their fingertips, built right into the Windows operating system that they already own. If you are still supporting a third-party or legacy VPN system, you should explore the new capabilities provided here and discover how much they could save your business.

DirectAccess and Always On VPN are particularly impressive and compelling connectivity options—a fresh way of looking at remote access. Automatic connectivity means your machines are constantly patched and updated because they are always connected to your management servers. You can improve user productivity and network security at the same time. These two things are usually oxymorons in the IT world, but with the Microsoft remote access stack, they hold hands and sing songs together.

Next, we are going to take a look at some of the security functions built into your Windows Server 2019 operating systems, and at some of the ways that your servers can be hardened to provide even better security than what comes out of the box.

## Questions

- 1. What does AOVPN stand for?
- 2. What are the two primary protocols used for connecting AOVPN clients?
- 3. In which version of Windows 10 was AOVPN released?
- 4. In what special instance would an AOVPN client be required to be joined to your domain?
- 5. Does DirectAccess require your corporate internal network to be running IPv6?
- 6. What is the name of the internal website that DirectAccess clients check in with in order to determine when they are inside the corporate network?
- 7. What role does a Web Application Proxy server hold in a federation environment?

## 9 Hardening and Security

*8.19 million dollars.* For anyone who read that in the voice of Dr. Evil, my hat goes off to you. For anyone who has no idea what I'm talking about, you may have had a sheltered childhood. Joking aside, that number is significant to IT security. Why? Because 8.19 million dollars is the average cost to a business when they are the victim of a data breach. I originally heard this and other scary statistics at a Microsoft conference in Redmond, and the numbers have continued to climb year on year. How about looking at another statistic that can be used to get approval for an increase in your security budget? Depending on which study you read, the average number of days of **dwell time** an attacker has in your network (the time they spend hanging around inside your files and infrastructure before they are detected and eradicated) is around 200. Think about that—200 days! That is the better part of a year that they're camping out for before you discover them! What are they typically doing during those 200 days? Siphoning all of your data bit by bit out of the back door. Another number is 76%—as in the percentage of network intrusions that happen as a result of compromised user credentials. Furthermore, it is becoming more and more difficult to identify these attacks in the first place because attackers are using legitimate IT tools to grab what they want. For instance, an attacker could socially engineer their way into the trust of a single employee and leverage that trust to get a remote access connectivity tool installed onto the user's work computer. Why use malware when you can use something that is *trusted* and is going to fly under the radar of intrusion detection systems? Makes sense to me.

Data security, network security, credential and identity security – these things are all becoming harder to accomplish, but there are always new tools and technologies coming out that can help you fight off the bad guys. Windows Server 2019 is the most secure server OS that Microsoft has produced; in this chapter, let's discuss some of the functionality included that makes that statement true:

- Windows Defender Antivirus
- Windows Defender Firewall – no laughing matter
- Encryption technologies
- Azure AD Password Protection
- Fine-grained password policy
- Advanced Threat Analytics – end of support
- General security best practices

### Windows Defender Antivirus

The term Windows Defender has been around for many years, but its terminology and capabilities have evolved numerous times with new OS releases. Defender existed even as far back as 2005, at the time providing very simple antimalware protection. The Windows 8 era introduced a fairly staggering change for Windows users, including Windows Defender in the operating system free out of the box. Although this sounds great on paper, Defender's capabilities were not taken too seriously by the IT population. Fast forward to today, however, and what is now known as Windows Defender Antivirus is a much improved antimalware/antivirus that is running on millions of Windows 10 client computers as well as, you guessed it, current versions of Windows Server. Defender Antivirus exists in the OS and is enabled by default, and as a result has a level of integration and responsiveness that is hard for third-party vendors to match. I can't tell you how many times I have tracked memory leaks and random server reboots back to a poorly functioning third-party antivirus software, which is unacceptable in today's server world. Some still consider the antivirus capabilities provided by Defender to be lackluster, perhaps only because it is free, but it does certainly carry some advantages over third-party AV. I have yet to see a Windows Defender product tank a client or server.

You have likely also seen and heard references to Windows Defender Advanced Threat Protection (ATP), recently renamed to Microsoft Defender Advanced Threat Protection. This is a family of products and systems that work together to protect your Windows machines. Antivirus/antimalware is only one of those capabilities, and built-in antivirus is a relatively new idea when talking about the Windows Server family of OSes.

The first server OS that we found with built-in Defender for antivirus was Server 2016. Many servers running in production for companies around the world are still Server 2012 R2 at this point (I know because I work on them every day), and so the improved existence of the Defender toolset in Server 2019 is yet another reason to start planning your migration today.

We simply do not have enough page space to dive into every aspect of Windows Defender ATP, and it is being continually improved upon. What we will do is explore some of the interfaces, make sure you know how to use the most common components that don't require policy-level manipulation or centralized administration, and expand your knowledge of some of the more advanced features that are available for further learning and digging.

### Installing Windows Defender Antivirus

You're done! Windows Defender Antivirus is installed by default in Windows Server 2019. In fact, unless you have somehow changed it, not only is Defender AV installed, it is automatically protecting your system as soon as the OS is installed. But don't take my word for it: if you open up Server Manager and choose **Add roles and features**, click ahead to the **Select features** page and you should find a checkbox already selected next to **Windows Defender Antivirus**:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_381_0.png)

Figure 9.1: Windows Defender Antivirus is preinstalled

If it's not already checked for some reason, then this is exactly the place to visit in order to get it installed and working.

### Exploring the user interface

The interface for the Windows Defender toolset is the same as within the latest versions of Windows 10, but if you haven't explored that yet, we will take a quick look at it here. Go ahead and launch **Settings** from inside the **Start** menu, then click on **Update & Security**.
