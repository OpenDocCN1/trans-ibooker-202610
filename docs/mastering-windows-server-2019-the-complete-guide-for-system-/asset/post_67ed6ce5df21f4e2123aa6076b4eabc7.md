
Remember, right now, we are running PowerShell on this local system (I happen to be logged in to my WEB3 server), so the only execution policy I am setting is the local one for my WEB3 system. If I wanted to change this setting globally or for a group of machines at the same time, I could utilize Group Policy for that change. The location inside Group Policy for configuring PowerShell script execution policy is Computer Configuration | Policies | Administrative Templates | Windows Components | Windows PowerShell | Turn on script execution.

## Using the Tab key

Before we get started navigating inside PowerShell, there is one important thing I want to point out: get used to pressing the Tab key when you are inside the PowerShell prompt! If you type the first few letters of any command or cmdlet or even an added variable to a cmdlet and then press Tab, the remainder of the cmdlet name will be automatically populated on the screen.

If we type get-co and then press Tab, the prompt automatically populates the full Get-Command cmdlet. Since there are multiple cmdlets that started with get-co, if you press Tab numerous times, you can see that it cycles through all of the available cmdlets that start with those letters.

Tab also works with file and folder names. For example, I downloaded a hotfix that needs to be installed onto a server. I want to launch this hotfix using the PowerShell prompt that I already have open, but I don't want to spend an entire minute or more trying to type out the huge filename of this hotfix. I have already navigated to the folder where my hotfix resides, and now if I simply type the first few letters of the filename and press the Tab key, PowerShell will populate the remainder of the filename. From there, all we need to do is press Enter to launch that installer:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_478_0.png)

Figure 11.9: The tab key auto-fills file or command names

## Useful cmdlets for daily tasks

When I started incorporating PowerShell into my daily workflow, I found it useful to keep a list of commonly used commands and cmdlets handy. Until you get to the point where they become memorized and second nature, if you don't have a quick and easy way to recall those commands, chances are you aren't going to use them and will revert to the old method of configuring your servers.

Here is a list of some of the items I use regularly when I'm building servers. Some are traditional commands that would also work from Command Prompt and some are cmdlets, but they are all useful when working inside a PowerShell window:

- Get-Command: This is useful for finding additional commands or cmdlets that you may want to run or research.
- Get-Command -Name *example*: Enhances the usefulness of Get-Command by adding the -Name switch to the end of it, so that you can filter results to whatever types of cmdlets you are searching for.
- GCM: This is simply a short alias for Get-Command. I only wanted to point this one out because many of the PowerShell cmdlets have aliases, such as gcm, that allow you to launch these commonly used cmdlets with fewer keystrokes.
- Get-Alias: Since we just mentioned the GCM alias for Get-Command, you may be wondering what other aliases are available inside PowerShell. To see a complete list, simply plug in the Get-Alias cmdlet. Using aliases rather than full cmdlet names can greatly reduce the number of characters you type inside PowerShell.
- Rename-Computer: This allows you to set a new hostname for the server.
- Add-Computer: Use the Add-Computer cmdlet to join servers or computers to a domain.
- hostname: This displays the name of the system you are currently working on. I use hostname all the time to make sure that I really am working on the server that I think I am. Have you ever rebooted the wrong server? I have. By running a quick hostname command, you can get peace of mind that the function you are about to perform is really happening on the correct system.
- $env:computername: This presents you with the hostname of the system you are working on, but I'm calling it out to show that PowerShell can easily tap into your environment variables in order to pull out information. The simpler hostname command is useful when you are logged into a local system and are simply trying to verify its name, but the ability to pull information from a variable, such as $env:computername, will be much more useful when creating scripts or trying to perform a function against a remote system.
- Logoff: The name is self-explanatory; Logoff just logs you out of the system. Rather than trying to find the **Sign out** function by clicking around inside your server's **Start** menu, you can throw a quick Logoff command into either a Command Prompt or PowerShell window, and you will be immediately logged out of that server. I use this one all the time when closing out RDP connections.
- Install-WindowsFeature: Use PowerShell to simplify the addition of new roles or features on your servers.

Both shutdown and Restart-Computer are useful for shutting down or restarting a server. On my own computer, these commands are most commonly preceded by the hostname command. When rebooting a server, you want to take special care that you restart the correct machine, so I find it best to open a PowerShell prompt, do a quick hostname check and then run a Restart command from that same prompt. This ensures that I am restarting the server that was returned in the hostname output:

```
shutdown /r /t 0
```

If you run a simple shutdown command, the system will shut down in one minute. I'm not sure why this is the default, as I have never found any IT administrator who actually wanted to wait that extra minute before shutting down their system. Instead, it is more efficient to set a time limit before that shutdown commences. In the preceding command, I have told the shutdown command that I want to restart instead of shutting down; that is what /r does. I have also told it to wait zero seconds before performing this restart. This way, it happens immediately; I don't have to wait for that default 60-second timer.

## Query user or quser

Most useful in RDS environments, the quser command will display all of the users that are currently logged in to a server, including statistics about whether they are logged in locally or remotely, and how long their session has been active. I commonly use quser to discover RDS sessions that have been sitting around idle for a long time, so that I can end those sessions and free up resources on the server:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_480_0.png)

Figure 11.10: The quser command displays logged-in sessions

```
quser /server:WEB1
```

Using quser in combination with the /server switch allows you to see the currently logged-in users on a remote system. This way, you can remain logged in to a single server in your RDS farm but can check on the user sessions for all of your systems without having to log into them. You could even write a script that runs this command against each of your session host servers and outputs the data to a file. This output could then be run on a schedule and used as a reporting mechanism for keeping track of which users were logged in to which RDS session host servers at any given time.

## IP addressing cmdlets

When we built a Server Core instance together earlier in this book, we utilized the Sconfig tool to define IP addressing for that server. While that was a quick and easy way to accomplish such a task, let's define the PowerShell cmdlets that are used for the configuration of IP addressing information on a server NIC:

```
New-NetIPAddress -InterfaceIndex 12 -IPAddress 10.10.10.40
-PrefixLength 24 -DefaultGateway 10.10.10.1
```

The above is just an example command with some sample numbers; the point here is that we can use New-NetIPAddress to assign IP addresses to NICs on your servers.

Often used in combination with New-NetIPAddress, use the following command to set the DNS server addresses in your NIC properties:

```
Set-DnsClientServerAddress -InterfaceIndex 12 -ServerAddresses
10.10.10.2,10.10.10.3
```

## Using Get-Help

How many hundreds of times have you used the /? switch in Command Prompt to pull some extra information about a command that you want to run? The extra information provided by this help function can sometimes mean the difference between a command being useful or completely useless. PowerShell cmdlets have a similar function, but you cannot simply /? at the end of a PowerShell cmdlet because a space following a cmdlet in PowerShell indicates that you are about to specify a parameter to be used with that cmdlet. For example, if we try to use /? with the Restart-Computer cmdlet to find more information about how to use Restart-Computer, it will fail to recognize the question mark as a valid parameter, and our output will be as follows:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_481_0.png)

Figure 11.11: PowerShell cmdlets do not work with /?
