
Instead, there is an even more powerful help function inside PowerShell. Get-Help is a cmdlet itself, and like any cmdlet, we need to use information following the cmdlet to specify and pull the information that we are looking for. So instead of using Get-Help at the end of a command, like we used to do with the question mark, we use it as its own entity.

Running Get-Help by itself only gives us more information about the Get-Help command, which may be useful to look over, but right now we are more interested in finding out how we can use Get-Help to give us additional information for a cmdlet we want to run, such as the Restart-Computer function. What we need to do is use Get-Help as a cmdlet, and then specify the other cmdlet as a parameter to pass to Get-Help, by placing a space between them:

```
Get-Help Restart-Computer
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_482_0.png)

Figure 11.12: Displaying help information for any cmdlet

The information provided by Get-Help is very comprehensive; in some cases, it has all of the same information that you can find on Microsoft Docs. Using Get-Help is a quick way of finding example syntax for commands and cmdlets that you may be unfamiliar with, and negates the need for you to launch a web browser to find that information. Make sure to start utilizing Get-Help to further your knowledge of any cmdlet in PowerShell!

> When using PowerShell on a fresh system, or one that you haven't worked on in a while, take a minute to run a command called Update-Help. This will cause PowerShell to reach out to Microsoft and download the latest versions of help files, so that you know you are viewing the most up-to-date information when using Get-Help.

## Formatting the output

When searching for information in PowerShell, I often encounter the case where so much information is provided to me that it's difficult to sort through. Are you trying to find useful cmdlets from Get-Command, or maybe track down a particular alias with Get-Alias? The output from these cmdlets can be staggeringly long. While we have discussed some parameters you can use to whittle down this output, such as specifying particular -Name parameters, there are a couple of formatting parameters that can also be appended to cmdlets, in order to modify the data output.

### Format-Table

The purpose of Format-Table is pretty simple: it takes the data output from a command and puts it into a table format. This generally makes the information much easier to read and work with. Let's look at an example. We have used Get-NetIPAddress a couple of times, but, let's be honest, its output is a little messy. Running the cmdlet by itself on my virtual server, which only has a single NIC assigned to it, results in four pages of data inside my PowerShell window, with all kinds of informational fields that are either empty or not important for finding the IP addresses assigned to my server:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_483_0.png)

Figure 11.13: Default output from Get-NetIPAddress

If we simply add Format-Table to the end of my Get-NetIPAddress cmdlet, the generated data is much easier on the eyes, while still giving me the important information that I am really looking for: the IP addresses being used on the system:

```
Get-NetIPAddress | Format-Table
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_484_0.png)

Figure 11.14: Formatted output of Get-NetIPAddress

Some of you may be familiar with a cmdlet called Select-Object, which can perform the same functions as Format-Table. While Select-Object seems to be the more widely known cmdlet, in my experience, it is actually less powerful than Format-Table, and so I suggest you spend some time playing around with the one we have discussed here.

### Format-List

Similar to the way that Format-Table works, you can utilize Format-List to display command output as a list of properties. Let's give it a quick try. We already know that Get-Command gives us the available cmdlets within PowerShell, and by default, it gives them to us in a table format.

If we wanted to view that output in a list instead, with more information being provided about each cmdlet, we could tell Get-Command to output its data in list format, with the following command:

```
Get-Command | Format-List
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_485_0.png)

Figure 11.15: Using Format-List to modify cmdlet output

This results in a tremendously long output of information, so long in fact that my PowerShell window had a problem displaying it all. Maybe we need to whittle that info down a little bit by narrowing our focus. Let's search for all of the cmdlets that include the word Restart while displaying them in list format:

```
Get-Command -Name *Restart* | Format-List
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_485_1.png)

Figure 11.16: List format information about Restart-related cmdlets

### Using a pipeline

Following the last couple of example cmdlets that we ran, you may be thinking to yourself, "I see that he is using that vertical line on the key above Enter on my keyboard, but why?"

Great question. In Command Prompt, we generally issue one command at a time. The same is often true for PowerShell when we are manually interacting with it, but in PowerShell, we have the potential for so much more power. One of those items of power is the ability to create a pipeline of commands. In other words, you can connect, or chain, commands together. This is commonly referred to as piping information from one cmdlet to another cmdlet and is done by using that little |.

Cmdlets often output data. If you then want to utilize that set of data against another cmdlet, this is where the pipe comes in handy. In our last example command, we told PowerShell to gather all of the commands that included the word Restart by performing Get-Command -Name *Restart*. Then we piped that output to Format-List. PowerShell took the outputted dataset and threw it at Format-List to display a different set of output. The pipe is used very often in PowerShell commands.

### Export to CSV

One common way to use a pipe is when grabbing information via PowerShell that you want to retain, document, or send to somebody else. A common method for storing output data is within CSV files, a popular enough request that Microsoft has baked a cmdlet in for just that purpose. Let's work with another cmdlet that is very useful, Get-EventLog, and combine it with Export-CSV to show off this capability.

Event logs are essential information in a Windows Server environment. Troubleshooting issues on a server almost always involves a step of reviewing the Windows event logs. There is, of course, a graphical interface from which to accomplish this, but filtering and searching within that GUI is not as useful as it should be. Instead, we can use PowerShell to output event log information, such as the following:

```
Get-EventLog System
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_487_0.png)

Figure 11.17: Viewing the system event logs in PowerShell

As you can see, the system event logs on this server contain a lot of information, and reading through it all within the PowerShell window could take the next three years. Filtering and searching is key to making this data useful, so let's see what this does instead:

```
Get-EventLog System | Export-CSV C:\Logs\SysLog.csv
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_487_1.png)

Figure 11.18: Piping output to a CSV file

PowerShell grabbed all of the information from system event logs and spit that info into a CSV file for us. We can now take that CSV file, open it inside a program like Microsoft Excel, and easily sort, filter, or search on these events in a much easier way than we could do from within the default Windows Event Viewer.

> You can also export to XML, HTML, or even pipe output directly to a printer!

### Pipes can invoke action

So far, we have only seen pipes modify outputted data. This is very useful, but I don't want you to walk away from this thinking that pipes are only useful for generating information. They can also take the output from one command and throw it at another command, but perhaps a command that actually causes a change or action to be performed. A good example is stopping a process or service. Let's pretend that for some reason you want to have a way to immediately stop all of the Hyper-V services on a server. Those services all have a DisplayName that begins with the word "Hyper-V." We can verify that by using the following command to view all of those services:

```
Get-Service -DisplayName hyper*
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_488_0.png)

Figure 11.19: Viewing services related to Hyper-V
