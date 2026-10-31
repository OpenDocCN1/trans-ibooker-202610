
If I were to use Test-Connection in the standard way to try to contact all of the servers in my network, that would turn into a whole lot of output. But by using the -Quiet switch, I get back a simple True or False on whether each individual server could be contacted:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_307_0.png)

Figure 7.8: Using the -Quiet switch

## Telnet

telnet provides quite a bit of remote management capability; it essentially offers the ability to make a connection between two computers to manipulate the remote machine through a virtual terminal connection. Surprisingly, we are not here to discuss any of the actual functionality that telnet provides, because with regard to networking, I find it is quite useful as a simple connection-testing tool, without knowing anything about what functionality the telnet command itself actually provides.

When we discussed ping, we talked about the downside to ICMP: it is easily blocked, and it is becoming more common in today's networks not to allow pings to be successful. This is unfortunate since ping has always been the most common form of network-connection testing, but the reality is that, if ping makes our lives easier, it also makes the lives of hackers easier. If we cannot rely on ping to tell us with certainty whether we can contact a remote system, what do we use instead? Another case that I see often is where a server itself might be responding correctly, but a particular service running on that server has a problem and is not responding. A simple ping may show the server to be online, but it can't tell us anything about the service specifically. By using the Telnet Client commands, we can easily query a server remotely. Even more importantly, we can opt to query an individual service on that server, to make sure it is listening as it is designed to do. Let me give you an example that I use all the time.

I often set up new internet-facing web servers. After installing a new web server, it makes sense that I would want to test access to it from the internet to make sure it's responding, right? But maybe the website itself isn't online and functional yet, so I can't browse to it with Edge or any other browser. It is quite likely that we disabled pings on this server or at the firewall level because blocking ICMP over the internet is very common to lower the security vulnerability footprint on the web. So my new server is running, and we think we have the networking all squared away, but I cannot test pinging my new server because, by design, it fails to reply. What can I use to test this? telnet. By issuing a simple telnet command, I can tell my computer to query a specific port on my new web server and find out whether it connects to that port. Doing this establishes a socket connection to the port on that server, which is much more akin to real user traffic than a ping would be. If a telnet command connects successfully, you know your traffic is making its way to the server, and the server service running on the port we queried seems to be responding properly.

The ability to use telnet is not installed by default in Windows Server 2019, or any other Windows operating system, so we first need to head into Server Manager and then Add Roles and Features to install the feature called Telnet Client:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_308_0.png)

Figure 7.9: Installing Telnet Client

> You only need to install **Telnet Client** on the machine from which you want to do command-line testing. You do not have to do anything on the remote server that you are connecting to.

Now that the Telnet Client feature has been installed, we can utilize it from a Command Prompt or PowerShell to do work for us by attempting to make socket connections from our computer to the remote service. All we need to do is tell it what server and port to query. Then telnet will simply connect or time out, and based on that result, we can see whether that particular service on the server is responding. Let's try it with our own web server. For our example, I have turned off the website inside IIS, so we are now in the position where the server is online but the website is dead. If I ping WEB1, I can still see it happily responding. You can see where server-monitoring tools that rely on ICMP would be showing false positives, indicating that the server was online and running, even though our website is inaccessible. Just below the successful ping in Figure 7.10, you can see that I also tried querying port 80 on the WEB1 server. The command that I used for that is telnet web1 80. That timed out. This shows us that the website, which is running on port 80, is not responding:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_309_0.png)

Figure 7.10: Failed connection on port 80

If I turn the website back on, we can try telnet web1 80 again, and this time I do not get a timeout message. This time, my PowerShell prompt wipes itself clean and sits waiting on a flashing cursor at the top. While it doesn't tell me yay, I'm connected!, this flashing cursor indicates that a successful socket connection has been made to port 80 on my web server, indicating the website is online and responding:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_310_0.png)

Figure 7.11: A flashing cursor signals a successful connection

> After creating a successful telnet socket connection, you may be wondering how to get back to the regular PowerShell interface. Press the Ctrl + ] keys together (that second one is a closed bracket key, usually next to the backslash key on your keyboard), type the word quit, and then press Enter. This should return you to a regular prompt. Or simply close the PowerShell prompt and open a fresh one.

## Test-NetConnection

If ping has an equivalent and improved PowerShell cmdlet called Test-Connection, does PowerShell also contain an improved tool that works similarly to telnet for testing socket connections to resources? It sure does. Test-NetConnection is another way to query particular ports or services on a remote system, and the displayed output is friendlier than that of telnet.

Let's walk through the same tests, once again querying port 80 on WEB1. You can see in the following screenshot that I have run the command twice. The first time the website on WEB1 was disabled, and my connection to port 80 failed. The second time, I re-enabled the website, and I now see a successful connection:

```
Test-NetConnection WEB1 -Port 80
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_311_0.png)

Figure 7.12: Using Test-NetConnection to test individual port connections

## Packet tracing with Wireshark

Eventually, you might need to look a little deeper into your network packets. Now we are entering territory where your network team may also be involved, but if you are familiar with these tools, you may be able to solve the problem before needing to call for assistance. Making use of command-line tools to check on the status of servers and services is very useful, but occasionally it may not be enough. For example, you have a client application that is not connecting to the application server, but you don't know why. Utilities such as ping and even telnet might be able to connect successfully, indicating that network routing is set up properly, yet the application fails to connect when it opens. If the application's own event logs don't help you troubleshoot what is going on, you might want to take a deeper look inside the network packets that the application is trying to push toward the server.

This is where the **Wireshark** application comes in handy. Microsoft used to supply and support a couple of self-developed tools called NetMon and Message Analyzer that served similar functions, but both have now been officially retired. Probably because Wireshark is great, and it's free! This tool captures network traffic as it leaves from, or arrives at, a system, and captures the information that is *inside* the packets so that you can take a deeper look into what is going on. In our example of an application that cannot connect, you could run Wireshark on the client computer to watch outgoing traffic, and also on the application server to watch for incoming traffic from the client.

There is a whole lot of functionality inside Wireshark, and we don't have the space to cover all of it here, so I will leave you with a link from which to grab this tool so you can start testing it out for yourself: https://www.wireshark.org/download.html.

## TCPView

The tools that we have discussed so far are great and can be used daily for poking and prodding individual resources that you want to test, but sometimes, there are situations where you need to step back and figure out what it is you are looking for in the first place. Maybe you are working with an application on a computer and are not sure what server it is talking to. Or perhaps you suspect a machine of having a virus and trying to *phone home* to somewhere on the internet, and you would like to identify the location that it is trying to talk to or the process that is making the call. In these situations, it would be helpful if there was a tool that you could launch on the local computer that shows you all of the network traffic streams that are active on this computer or server clearly and concisely. That is exactly what **TCPView** does. TCPView is a tool created by Sysinternals; you may have heard of some of their other tools, such as ProcMon and FileMon. Running TCPView on a machine displays all of the active TCP and UDP connections happening on that computer in real time. Also important is the fact that you do not need to install anything to make TCPView work; it is a standalone executable, making it extremely easy to use and clear off a machine when you are finished with it.

You can download TCPView from https://technet.microsoft.com/en-us/sysinternals/tcpview.aspx.

Simply copy the file onto a computer or server that you want to monitor and double-click on it. *Figure 7.13* shows the TCPView interface running on my local computer, showing all of the connections that Windows and my applications are currently making. You can pause this output to take a closer look, and you can also set filters to pare down the data and find what you are really looking for. Filters get rid of the *noise*, so to speak, and enable you to look more closely at a particular destination or a specific process ID:
