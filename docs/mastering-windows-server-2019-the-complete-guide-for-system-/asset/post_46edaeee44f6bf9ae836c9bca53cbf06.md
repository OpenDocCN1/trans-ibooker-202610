
We can take the output of this Get-Service cmdlet and pipe it to Stop-Service, and the resulting command will immediately stop all services that begin with the word "Hyper":

```
Get-Service -DisplayName hyper* | Stop-Service
```

## PowerShell Integrated Scripting Environment

Most server administrators are familiar with the concept of creating batch files for use in the Command Prompt world. Have a series of commands that you want to run in sequence? Need to run this sequence of commands multiple times across different servers or over and over again in the future? Throwing multiple commands inside a text document and then saving it with the .BAT file extension will result in a batch file that can be run on any Windows computer, issuing those commands in sequence, which saves you the time and effort of having to plunk out these commands over and over inside the command-line interface.

Scripting in PowerShell is the same idea but is much more powerful. Commands in Command Prompt are useful, but limited, while PowerShell cmdlets have the ability to manipulate anything within the operating system. With PowerShell, we can reference items from inside environment variables or the registry, we can easily issue commands to remote systems, and we can even utilize variables inside a PowerShell script, just like you would do with any full programming language.

Let's explore a couple of different ways that can be used to start creating your first PowerShell scripts.

## PS1 files

Creating a simple .PS1 file (a PowerShell script file) is almost exactly the same idea as creating a .BAT file. Simply open up a text document using your favorite editor, throw in a series of commands or cmdlets, and then save the file as FILENAME.PS1. As long as your PowerShell environment allows the running of scripts – see earlier in the chapter about the DEP – you now have the ability to double-click on that .PS1 file, or launch it from any PowerShell prompt, to run the series of cmdlets inside that script. Let's give it a try and prove that we can get a simple script up and operational.

Since you are only going to create scripts that serve a purpose, let's think of a real-world example. I work with terminal servers quite a bit – pardon me, RDS servers – and a common request from customers is a log of what users logged in to which servers.

A simple way to gather this information is to create a logon script that records information about the user session to a file as they are logging in. To do this, I need to create a script that I can configure to run during the logon process. To make the script a little bit more interesting and flexible down the road, I am going to utilize some variables for my username, the current date and time, and record the name of the RDS server being logged in to. That way, I can look at the collective set of logs down the road, and easily determine which users were on which servers. I am going to use Notepad to create this script. I have opened up a new instance of Notepad, entered in the following commands, and am now saving this as C:\Scripts\UserReporting.ps1:

```
$User = $env:username
$RDSH = $env:computername
$Date = Get-Date

echo $User,$Date,$RDSH | Out-File C:\Scripts\UserReporting.txt -append
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_490_0.png)

Figure 11.20: Creating a simple PS1 script

You can probably already tell what this script is doing, but let's walk through it anyway. First, we are defining three variables. I am telling the script that $User needs to equal the environment variable username. This will give me the username of the person who is signing in. $RDSH is going to be the name of the server where the user is logging in, also pulled by accessing the server's environment variables. The third variable defined is $Date, which simply pulls the current system date by calling a PowerShell cmdlet named Get-Date.

After pulling all of the information into the PowerShell variables, I am then outputting these three items into a text file that is sitting on my server's hard drive.

If I run this script a few times, I can open up my UserReporting.txt file and see that every time the script is run, it successfully logs my specified variables into this report file:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_491_0.png)

Figure 11.21: Results of our PowerShell script

## Working with PowerShell ISE

If I'm being honest, putting together that simple script we just ran took some trial and error. I didn't have a copy of it readily available to work from, and I needed to test a couple of the lines individually in PowerShell before I was confident they would work in my script. I also first tried to pull the username without using the environment variable, and it didn't work. Why did I have so much trouble putting together just a few simple lines of code? Because as I type those lines in Notepad, I have absolutely no idea whether they are going to work when I save and attempt to run that script. All of the text is just black with a white background, and I am fully trusting my own knowledge and scripting abilities to put together something that actually works.

Thankfully, we have access to the **PowerShell Integrated Scripting Environment (ISE)**. This is a program that is installed by default in Windows Server 2019; it is a scripting shell that allows you to write PowerShell scripts and provides help along the way. Let's go ahead and open it up. If you have any PS1 PowerShell script files, you can simply right-click on one of them and choose **Edit**. Otherwise, by right-clicking on the PowerShell application icon (from the taskbar, for example), you will find an option to launch **Windows PowerShell ISE** right from that menu:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_492_0.png)

Figure 11.22: Launching PowerShell ISE

Now, if we start typing in the same script information that I used in Notepad a few minutes ago, you can see that even as we type, we get popups and prompts that help us decide which cmdlets or variables we want to utilize. Similar to the way that autocomplete keyboards on our smartphones work, ISE will give suggestions about what you are starting to type, so that you don't necessarily have to remember what the cmdlets or parameters are called; you can take an educated guess on what letter it starts with and then choose one from the list that is presented. There is also a list off to the right of all the commands available, and it is searchable! That is a great feature that really helps to get these scripts rolling:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_493_0.png)

Figure 11.23: ISE provides suggestions for cmdlets and variables

Also useful is the blue PowerShell mini screen that consumes the bottom half of the development window inside ISE. Basically, when you type in some commands, ISE helps to make sure they are all going to work by color-coding the cmdlets and parameters for easy identification, and then you can click on the green arrow button in the taskbar that is labeled **Run Script (F5)**. Even if you haven't saved your script anywhere yet, ISE launches through your commands and presents the output in the following PowerShell prompt window. This allows you to test your script, or test changes that you are making to an existing script, without having to save the file and then launch it separately from a traditional PowerShell window:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_494_0.png)

Figure 11.24: Testing the script inside ISE

What's even better is that you can highlight particular sections of your script and choose to run only isolated pieces of the code. This allows you to test certain sections of a script, or do something creative, such as keep one big PS1 script file that is full of common PowerShell commands that you might use on a daily basis. When you have the need to run just one of them, you can simply highlight the text that you want to run and click on the **Run Selection (F8)** button. By highlighting text before running the script from within ISE, only the selected cmdlet(s) will be put into action. In the following screenshot, you can see that I have numerous cmdlets listed inside my script file, but only the one that is highlighted has been run when I pressed F8:
