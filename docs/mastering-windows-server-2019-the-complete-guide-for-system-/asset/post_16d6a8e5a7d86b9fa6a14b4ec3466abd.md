
Open up an elevated PowerShell prompt, most easily accomplished via the quick admin tasks menu, accessed by right-clicking on the Start button. Then use the following command to view all of the available roles and features that we can install onto our server. It will also show you which ones are currently installed:

```
Get-WindowsFeature
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_85_0.png)

Figure 2.13: A list of all the roles and features available and installed

What I would like to do on this server is install the Telnet Client feature. I use Telnet Client pretty regularly for testing network connections, so it is helpful to have on this machine. Unfortunately, my PowerShell window currently has pages and pages of different roles and features in it, and I'm not sure what the exact name of the Telnet Client feature is in order to install it. So, let's run Get-WindowsFeature again, but this time let's use some additional syntax in the command to pare down the amount of information being displayed. I want to see only the features that begin with the letters TEL, as shown in the following examples:

```
Get-WindowsFeature -Name TEL*
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_85_1.png)

Figure 2.14: A list of features beginning with the letters TEL

There it is! Okay, so now that I know the correct name of the feature, let's run the command to install it, as shown in the following example:

```
Add-WindowsFeature Telnet-Client
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_86_0.png)

Figure 2.15: Install Telnet Client

One last thing to show you here – there is also a way to manipulate the Get-WindowsFeature cmdlet in order to quickly show only the roles and features currently installed on a server. Typing Get-WindowsFeature | Where Installed presents us with a list of the currently installed components. If I run that on my domain controller, you can see all the parts and pieces of my roles for AD DS, DNS, and DHCP, as well as my newly installed Telnet Client feature, as shown in Figure 2.16:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_86_1.png)

Figure 2.16: A list of all installed components

## Centralized management and monitoring

Whether you are installing new roles, running backups and maintenance programs, or troubleshooting and repairing a server, it is common sense that the first thing you would do is log directly into the server that you need to work on. Long ago this meant walking up to the server itself and logging on with the keyboard and mouse that were plugged right into that hardware. Then, quite a number of years ago, this became cumbersome and technology advanced to the point where we had the **Remote Desktop Protocol (RDP)** available to us. We quickly transitioned over to log into our servers remotely using RDP. Even though it's been around for many years, RDP is still an incredibly powerful and secure protocol, giving us the ability to quickly connect to servers from the comfort of our desk. And, as long as you have proper network topology and routing in place, you can work on a server halfway around the world just as quickly as one sitting in the cubicle next to you. In fact, I recently read that mining rights were being granted in outer space. Talk about a co-location for your datacenter! Maybe someday we will be using RDP to connect to servers in outer space. While this might be a stretch in our lifetimes, I do have the opportunity to work with dozens of new companies every year, and, while there are some other tools available for remotely managing your server infrastructure, RDP is the platform of choice for 99% of us out there.

Why talk about RDP? Because you probably all use it on a daily basis, and I needed to let you know that Windows Server 2019 includes some tools that make it much less necessary to our day-to-day workflow. The idea of centralized management in the server world has been growing through the last few Windows Server operating system rollouts. Most of us have so many servers running that checking in with them all daily would consume way too much time. We need some tools that we can utilize to make our management and monitoring, and even configuration processes, more efficient in order to free up time for more important projects.

## Server Manager

If you have worked on Windows Server recently, you are familiar with the idea that logging into any of your servers automatically invokes a large window on top of the desktop. This auto-launching program is **Server Manager**. As the name implies, it's here to help you manage your server. However, in my experience, the majority of server administrators do not utilize Server Manager. Instead, they close it as fast as they can and curse at it under their breath, because it's been popping up and annoying them during every server login for the past 10 years.

Stop doing that! It's here to help, I promise. Figure 2.17 shows the default view of Server Manager on my new domain controller:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_88_0.png)

Figure 2.17: Server Manager on my domain controller

What I like about this opening automatically is that it gives me a quick look into what is currently installed on the server. Looking at the column on the left side shows you the list of roles installed and available for management. Clicking on each of these roles brings you into some more particular configuration and options for the role itself. I often find myself hopping back and forth between many different servers while working on a project, and leaving Server Manager open gives me a quick way of double-checking that I am working on the correct server. The ROLES AND SERVER GROUPS section at the bottom is also very interesting. You might not be able to see the colors in the picture if you are reading a printed copy of this book, but this gives you a very quick view into whether or not the services running on this server are functioning properly. Right now, both my AD DS and DHCP functions are running normally, I have a nice green bar running through them. But, if anything was amiss with either of these roles, it would be flagged bright red, and I could click on any of the links listed under those role headings in order to track down what the trouble is.

Up near the top-right corner, you see a few menus, the most useful of which, to me, is the **Tools** menu. Click on that, and you see a list of all the available **Administrative Tools** to launch on this server. Yes, this is essentially the same Administrative Tools folder that has existed in each of the previous versions of Windows Server, now stored in a different location. Based on my experience, Server Manager is now the easiest way to access this myriad of tools all from a single location:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_89_0.png)

Figure 2.18: The Tools menu in Server Manager

So far, the functions inside Server Manager that we have discussed are available on any installation of Windows Server 2019, whether it is standalone or part of a domain. Everything we have been doing is only dealing with the local server that we are logged into. Now, let's explore what options are available to us in Server Manager for the centralization of management across multiple servers. The new mentality of managing many servers from a single server is often referred to as *managing from a single pane of glass*. We will use Server Manager on one of our servers in the network to make connections to additional servers, and after doing that we should have much more information inside Server Manager that we can use to keep tabs on all of those servers.

Front and center inside the **Server Manager** console is the section entitled **Welcome to Server Manager**. Under that, we have a series of steps or links that can be clicked on. The first one lets you configure settings that are specific only to this local server. We have already done some work with the second step when we added a new role to our server. Now we will test out the third step, **Add other servers to manage.**

By the way, this same function can also be called by clicking on the **Manage** menu at the top, and then choosing **Add Servers**, as shown in Figure 2.19:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_90_0.png)

Figure 2.19: Adding servers

Most of you will be working within a domain environment where the servers are all domain-joined, which makes this next part really easy. Simply click on the **Find Now** button, and the machines available within your network will be displayed. From here, you can choose the servers that you want to manage, and move them over to the **Selected** column on the right, as shown in Figure 2.20:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_90_1.png)

Figure 2.20: Select the servers you want to manage
