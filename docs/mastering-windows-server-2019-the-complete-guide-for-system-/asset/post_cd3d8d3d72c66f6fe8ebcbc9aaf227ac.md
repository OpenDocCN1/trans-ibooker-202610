
Add the new WEB4 server into our list of managed machines and it is now manageable from inside this instance of Server Manager. Getting back to what my original intentions were, I want to install the Web Server (IIS) role on WEB4. If I use the Add roles and features function inside Server Manager, I can now choose to manipulate the WEB4 server:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_446_0.png)

Figure 10.18: Modifying WEB4 from WEB3's Server Manager

Just like with any server running the full Desktop Experience version of Windows Server, we can now finish walking through the role installation wizard, and the Web Server role will be installed on WEB4.

## Remote Server Administration Tools

Also true is the fact that you can manage Server Core instances with the Remote Server Administration Tools (RSAT) in Windows 10. RSAT is essentially just a copy of Server Manager and accompanying administrative tools that are designed to run on the client operating system. In our case, I already have a Windows 10 machine on which I installed RSAT earlier in the book, so I will test by logging in to that guy and adding WEB4 to the interface. I just finished installing the IIS role on WEB4 in our previous task, so I should be able to see that listed inside RSAT when I connect it to WEB4.

If you haven't used RSAT before and haven't read over that section of our text, it is important to know that there is no application called Remote Server Administration Tools. Instead, after the RSAT installation has been completed, take a look inside your Start menu for the application called Server Manager. This is how you utilize a Windows 10 client to remotely manage Windows Server 2019 instances:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_447_0.png)

Figure 10.19: Server Manager inside Windows 10, courtesy of RSAT

Precisely as you would do from a Server Manager interface of Windows Server 2019, go ahead and walk through the wizard to add other servers to manage. Once I have added WEB4 as a managed server in my Win10's Server Manager, I can see IIS listed inside my Dashboard. This indicates that my IIS service running on WEB4 is visible, accessible, and configurable right from my Windows 10 desktop computer. For the majority of the tasks that I need to accomplish on WEB4, I will never have to worry about logging in to the console of that server.

If I right-click on the WEB4 server name from within this RSAT console, you can see that I have many features available to me that I can use to manage this remote Server Core instance:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_447_1.png)

Figure 10.20 Interacting with remote servers via Server Manager

So, you can see that there are ways to use the GUI tools to manage our GUI-less instances of Windows Server. It's just a matter of putting your mind into a place where you are thinking of servers as headless, and that tools such as PowerShell or Server Manager really don't care at all whether the server they are changing is local or remote. The processes and tools are the same either way. You can see in the previous screenshot that I could even click from here to launch a remote PowerShell connection to WEB4. Clicking on this button immediately launches a PowerShell prompt that is remotely tied to the WEB4 server, even though I am currently only logged in to my Windows 10 workstation. This is even easier than issuing the Enter-PSSession cmdlet from inside PowerShell.

## Accidentally closing Command Prompt

Let's now take a look at one more thing directly from the Server Core console; this is a common hurdle to overcome if you haven't utilized Server Core much. We tend to close windows and applications that are no longer being used, and so you might unconsciously close the Command Prompt window that is serving your entire administrative existence within a Server Core console session. Now you're sitting at a large blank screen, with seemingly no interface and nowhere to go from here.

How do you get back to work on this server? Do we have to turn the server off and back on to reset it? That would interrupt any roles or traffic that this server might be serving up to users, so obviously isn't the ideal approach.

There is a simple way to get Command Prompt back, and that is by using Task Manager to launch a new instance of Command Prompt. After mistakenly closing your current Command Prompt window, when sitting at the empty black screen of a Server Core console, you can press Ctrl + Alt + Del and you will be presented with the following options:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_448_0.png)

Figure 10.21: Ctrl+Alt+Del works inside Server Core

There are a few different functions you can perform here, which is pretty neat. But to get our Command Prompt window back, arrow-down to **Task Manager** and press **Enter**. This will launch the **Task Manager** application that we are all familiar with. Now, click on **More details** to expand Task Manager's screens. Drop down the **File** menu, and then click on **Run new task**:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_449_0.png)

Figure 10.22: Running a new task via Task Manager

In the **Create new task** box, type `cmd` and then click **OK**:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_449_1.png)

Figure 10.23: Opening Command Prompt

Alternatively, you could specify to launch any application directly from this **Create new task** prompt. If you were interested in moving straight into PowerShell, instead of typing cmd, you could instead simply type powershell into that prompt, and it would open directly:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_450_0.png)

Figure 10.24: Opening PowerShell in Server Core

## Windows Admin Center for managing Server Core

While Command Prompt from the console, remote PowerShell connections, remote Server Manager administration, and even the RSAT tools running on a Windows 10 workstation are all valid and powerful tools for administering our Server Core instances, they have all now been upstaged by the release of Windows Admin Center. You have already learned what Windows Admin Center can do for centrally managing your entire server infrastructure, but what we need to point out here is that WAC can be used for servers both with and without graphical interfaces.

I have spoken with many Windows Server administrators about the topic of Server Core, and one of the biggest blocks to implementing these more efficient and secure server platforms is an apprehension that, once configured, ongoing administration and maintenance of these servers will be more difficult to handle. Admins who are familiar and comfortable working within the Windows Server Desktop Experience know exactly what needs to be done to accomplish their daily tasks, but remove that point-and-click interface and suddenly the workday gets a lot more complicated.

Thankfully, you don't have to memorize the PowerShell handbook to use Server Core! Windows Admin Center treats Server Core instances in the same way that it does a server running Desktop Experience. It just works!

We already have WAC installed on a server in our test lab, so let's open it up and add in my new WEB4 server to be administered and take a look at what options are available for ongoing maintenance of this server.

When we first connect to WEB4 via the WAC console, there is nothing in here that even indicates this is a Server Core instance. We have all of the WAC tools and utilities available to click on:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_451_0.png)

Figure 10.25: WAC overview of WEB4

Let's try a couple of things from Windows Admin Center. You have power controls right near the top of the screen from which you could easily shut down or restart the server. That is much easier and faster than having to establish a remote PowerShell connection from which you can issue commands to accomplish the same actions. There are also performance metrics on the home screen (if you scroll down), showing your consumed CPU, Memory, and Networking resources. Without WAC, you would need to log in to WEB4 and launch Task Manager to see these statistics:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_452_0.png)

Figure 10.26: Server performance metrics
