
![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_439_0.png)

Figure 10.8: Displaying your interface list

Now that we know the interface number, let's build the commands that are going to assign the new IP address settings to the NIC. I am going to use one command to assign the IP address, subnet mask prefix, and default gateway. I will use a second command to assign DNS server addresses:

```
New-NetIPAddress -InterfaceIndex 4 -IPAddress 10.10.10.12 -PrefixLength 24 -DefaultGateway 10.10.10.1

Set-DNSClientServerAddress -InterfaceIndex 4 -ServerAddresses 10.10.10.10,10.10.10.11
```

Figure 10.9 shows us the resulting output:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_439_1.png)

Figure 10.9: Assigning the IP and DNS server addresses

> Hold the phone! How did I get two PowerShell prompts open at the same time within the Server Core interface? Make sure to read the Accidentally closing Command Prompt section later in this chapter to discover how you can launch multiple windows and tools inside the Server Core console.

Now, all of these IP settings should be in place on the NIC. Let's double-check that with a Get-NetIPConfiguration command, seen in Figure 10.10. Alternatively, you could use good old ipconfig to check these settings, but where's the fun in that?

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_440_0.png)

Figure 10.10: Checking the IP settings

> Remember, you can always utilize DHCP reservations to make this a little bit easier. If you were to run a simple ipconfig /all from your Server Core and jot down the MAC address of your NIC, you could use this address to create a reservation in DHCP and assign a specific IP address to the new server that way.

## Setting the server hostname

Now that we have network connectivity, a good next step is setting the hostname of our server and joining it to the domain. First things first, let's see what the current name of the server is, and change it to something that fits our standards. When you freshly install Windows, it self-assigns a random hostname to the server. You can view the current hostname by simply typing hostname and pressing Enter.

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_441_0.png)

Figure 10.11: Checking the current hostname

To change the hostname of your server, we need to use PowerShell. Bring yourself into a PowerShell prompt if not already there, and all we need to do is use the Rename-Computer cmdlet to set our new hostname. I have decided to name this new server WEB4 because later we will install the Web Services role onto it and host a website. Remember, after renaming your computer just like in the GUI version of Windows Server, a system restart is necessary to put that change into action. So, following your Rename-Computer command, you can issue a Restart-Computer command to reboot the VM:

```
Rename-Computer WEB4

Restart-Computer
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_441_1.png)

Figure 10.12: Changing the hostname and restarting your system

## Joining your domain

The next logical step is, of course, joining your domain. These are the standard functions that we would perform on any new server in our environment but done in a way that you may have never encountered before since we are doing all of this strictly from the Command Prompt and PowerShell interfaces. To join a Server Core to your domain, head into PowerShell and then use the Add-Computer cmdlet. You will be asked to specify both the domain name and your credentials for joining the domain—the same information you would have to specify if you were joining a Windows Server 2019 in Desktop Experience mode to a domain. First, you must specify credentials that will be used to accomplish this domain join:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_442_0.png)

Figure 10.13: Entering credentials authorized to join this system to your domain

Then you tell it what domain you would like to join:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_443_0.png)

Figure 10.14: Specifying the domain to join

Alternatively, you could utilize the -DomainName parameter in combination with the original Add-Computer cmdlet to specify the name of the domain as part of the original command. And of course, after joining the domain, you need to Restart-Computer once again to finalize this change.

## Remote PowerShell

Once the new server is IP-addressed, named, and domain-joined, we can start doing some real administration on this new Server Core instance. You could certainly continue to log in and interface directly with the console, but as with managing any other server in your environment, there must be ways to handle this remotely, right? One of the ways that you can manipulate Server Core without having to sit in front of it is by using a remote PowerShell connection.

We will cover the process for using remote PowerShell to manipulate servers (both GUI and headless) in more detail in Chapter 11, PowerShell, but here is a glimpse of the commands necessary and the capabilities present when you are able to achieve a remote session from a PowerShell prompt on a workstation inside a domain-joined environment.

Open up PowerShell from another system—this can be a server or even a client operating system. This PowerShell window is obviously open within the context of whatever machine you are currently logged in to, and any commands you issue via PowerShell will elicit a response from the local system. To tap PowerShell into the WEB4 Server Core instance, I will issue the following command. After running this, I am prompted for a password corresponding to the administrator account, and will then be able to issue remote PowerShell commands against our Server Core:

```
Enter-PSSession -ComputerName WEB4 -Credential administrator
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_444_0.png)

Figure 10.15: Using Enter-PSSession to remotely connect to WEB4

Now we are sitting at a PowerShell prompt, remotely connected to the WEB4 Server Core box. You can see this by [WEB4] being listed to the left of our prompt. Perhaps you don't trust that little identifier, and want to make sure that this PowerShell window is now accessing and manipulating the remote WEB4 server? Let's issue a couple of quick commands, such as hostname and ipconfig, to prove that the information being given to us in this PowerShell session is really coming from the new WEB4 server:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_444_1.png)

Figure 10.16: PowerShell now responds as if I was logged in to WEB4

Now that we have a remote PowerShell connection to this new Server Core, we can do pretty much whatever we want to that server, right from this console. Commands and cmdlets issued inside this remote PowerShell session will manipulate WEB4, rather than our local workstation.

## Server Manager

While the initial configuration of your server will be handled to a degree from the command-line interfaces available at the console, once your server has been established on the network, it will likely be more advantageous for you to expand your horizons a little. You could probably find PowerShell cmdlets that allow you to manage and manipulate anything in your new server, but that is still a pretty new mentality for most of us — we are generally more accustomed to using graphical tools such as Server Manager. You already know that Server Manager can be used to manage multiple servers, local and remote, and is a piece of the Microsoft centralized management puzzle. This remote management capability in Server Manager that we explored earlier in the book allows you to tap into not only GUI-based Windows Servers but Server Core instances as well.

I want to install a role on my new WEB4 server. I could do that with PowerShell right on the server console, but instead let's try adding WEB4 into Server Manager, which is running on another one of my servers. I am going to log in to WEB3 and use Server Manager from there. Just like we have already seen, I can add a new server into Server Manager using the Manage menu and choosing Add Servers:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_445_0.png)

Figure 10.17: Adding new servers to be managed inside Server Manager
