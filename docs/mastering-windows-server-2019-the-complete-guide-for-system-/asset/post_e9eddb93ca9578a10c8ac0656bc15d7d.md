
## The Windows 10 experience continued

Historically, a new release of any Microsoft operating system has meant learning a slightly new and changed user interface, or sometimes a drastically new and changed interface like that of Windows 8. Server 2019 is an exception to this rule, and subsequent versions will likely follow suit. The ongoing Windows 10 releases give us first looks into the same graphical platform that will reside on our newest LTSC release of Windows Server. This idea started when Windows Server 2016 was first released. Now that Windows 10 updates are releasing but continuing on with essentially the same desktop interface, the same is true for Server 2019. Logging in and using Windows Server 2019 is, in a lot of ways, the same experience that you have had inside Windows Server 2016. Even so, some reading this book have never experienced logging into a server of any kind before, and so we will certainly be looking over that interface and learning some tips and tricks for navigating around smoothly and efficiently within Server 2019.

## Hyper-Converged Infrastructure

When you see the phrase **Hyper-Converged Infrastructure (HCI)**, it is important to understand that we are not talking about a specific technology that exists within your server environment. Rather, HCI is a culmination of a number of different technologies that can work together and be managed together, all for the purposes of creating the mentality of a **Software-Defined Datacenter (SDDC)** as it is sometimes referred to. Specifically, HCI in the Microsoft world is most often referred to as the combination of Hyper-V and **Storage Spaces Direct (S2D)** on the same cluster of servers. Clustering these services together enables some big speed and reliability benefits over hosting these roles separately, and on their own systems.

Another component that is part of, or related to, a software-defined datacenter is **Software-Defined Networking (SDN)**. Similar to how compute virtualization platforms (like Hyper-V) completely changed the landscape of what server computing looked like 12 or more years ago, we are now finding ourselves capable of lifting the network layer away from physical hardware and shifting the design and administration of our networks to be virtual and managed by the Windows Server platform.

A newly available tool that helps configure, manage, and maintain clusters as well as HCI clusters is the always-improving **Windows Admin Center (WAC)**. WAC can be a hub from which to interface with your Hyper-Converged Infrastructure.

## Windows Admin Center

**Windows Admin Center** (WAC) is one of the coolest things I've seen yet that came to us around the same time as the Server 2019 release. This is a free tool, available to anyone, that you can use to start centrally managing your server infrastructure. While not fully capable of replacing all of the traditional PowerShell, **Remote Desktop Protocol (RDP)**, and **Microsoft Management Console (MMC)** administration tools, it enables you to do a lot of normal everyday tasks with your servers, all from a single interface.

WAC has been continually updated and improved since release, and can now be used to manage servers, to build out clusters and HCI environments, and as a centralized administration tool not only for your on-premise servers but for those residing inside Azure as well. Prior to official release and naming, Windows Admin Center was formerly known as Project Honolulu, if you ever encounter a text that still includes that title.

We will take a closer look at Windows Admin Center in *Chapter 2, Installing and Managing Windows Server 2019*.

## Windows Defender Advanced Threat Protection

If you haven't done any reading on **Advanced Threat Protection (ATP)**, you may see the words Windows Defender and assume I am simply talking about the antivirus/ anti-malware capabilities that are now built into both Windows client operating systems, as well as Windows Server starting with 2016. While it is true that Windows Server 2019 does come out of the box with built-in antivirus, the ATP service and Windows Defender as a whole are becoming much, much more.

We'll discuss it in more depth in *Chapter 9, Hardening and Security*, but the short summary is that Windows Defender Advanced Threat Protection is a cloud-based service that you tap your machines into. The power of ATP is that many thousands, or perhaps even millions, of devices are submitting data and creating an enormous information store that can then be used with some AI and machine learning to generate comprehensive data about new threats, viruses, and intrusions in real time. ATP customers then receive the benefits of protection as those new threats arise. It's almost like crowd-sourced anti-threat capabilities, with Azure handling all of the backend processing.

## Banned passwords

Active Directory has stored all of our user account information, including passwords, for many years. The last few releases of the Windows Server operating system have not included many updates or new features within AD, but Microsoft is now working with many customers inside their cloud-based Azure AD environment, and new features are always being worked on in the cloud. Banned passwords are among those things. Natively an Azure AD capability, it can now be synchronized back to your on-premise domain controller servers, giving you the ability to create a list of passwords that cannot be used in any fashion by your users. For example, the word *password*. By banning *password* as a password, you effectively ban any password that includes the word *password*. For example, P@ssword, Password123!, or anything else of similar bearing that might otherwise pass muster for meeting standard complexity requirements.

## Soft restart

The ability to perform a soft restart was actually new with Server 2016, but it had to be manually added into Server 2016 and I don't think anybody really ever started using it. I have never seen a single person initiate a soft restart, so I assume it is not well-known and I will include it here in our list of features. In an effort to speed up reboots, there is an optional reboot switch called **soft restart**, which is now included automatically inside Server 2019. So, what is a soft restart? It is a restart without hardware initialization.

In other words, it restarts the operating system without restarting the whole machine. It is invoked during a restart by adding a special switch to the shutdown command. Interestingly, in Server 2016 you could also invoke a soft restart with the Restart-Computer cmdlet in PowerShell, but that option seems to have fallen away in Server 2019. So, if you want to speed up your reboots, you'll have to turn back to good old Command Prompt. Note the following using the shutdown command:

```
shutdown /r /soft /t 0
```

Here /r is for restart, /soft is for soft restart, and /t 0 is for zero seconds until reboot initiates.

## Integration with Linux

Heresy! Under whose authority did I type the word Linux inside a book about Windows Server?! Historically, corporate computing environments have run Windows, or they have run Linux, or maybe they have run both but with a very clear separation between the two. Windows Server 2019 blurs that line of separation. We now have the ability to run Linux VMs within our Microsoft Hyper-V and to even interface with them properly. Did you know some Linux operating systems actually know how to interact with a mouse? Before now, you didn't have much chance of that when trying to run a Linux-based VM on top of Windows Server, but we now have some compatibility implemented in Hyper-V.

Linux-based containers can also be run on top of Server 2019, which is a big deal for anyone looking to implement scaling applications via containers.

You can even protect your Linux virtual machines by encrypting them, through the use of shielded virtual machines!

## SAC releases are shrinking!

We have already learned that LTSC major releases of Windows Server only happen once every few years at most, but what's new in those bi-annual SAC releases? Most of the enhancements made there revolve around containers, since the SAC versions of Windows Server 2019 are only used for containerized purposes, but there is one common theme that resounds with almost every new release – the container image shrinks!

While this doesn't seem earth-shattering, if you've been around Microsoft Windows products for a long time and have watched new releases, you know that almost every updated release has grown in size, not shrunk. Minimizing the footprint of container images is a priority for Microsoft, because the smaller they are the more you can squeeze onto your server, and the more efficient they will be!

As an example, the base container image for Windows Server 1903 was 5.1 GB on disk, while the image for Windows Server 2004 (again, ouch with the name) is a gigabyte-penny-pinching 3.98 GB.
