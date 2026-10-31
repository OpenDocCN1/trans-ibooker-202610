
## Working within PowerShell

The first step to doing real work with PowerShell is getting comfortable interfacing with the platform and becoming familiar with the daily routines of working from this command-line interface, rather than relying on your mouse pointer. Here, we will explore some of the most common ways that I have seen server administrators make use of PowerShell to enhance their daily workflow.

We will be working throughout this chapter inside the version of PowerShell that comes out of the box with Windows Server 2019, PowerShell 5.1. You may have heard the news of PowerShell 7’s release. If you are familiar with this and already utilizing it, everything that we discuss will work perfectly well on that updated platform. To administer a Windows Server instance, the differences between versions won’t have much of a bearing. Installing PowerShell 7 will not update PowerShell 5.1, but rather they will run side by side.

PowerShell 7 is open-source and comes with the ability to work cross-platform, with Windows, macOS, and Linux. It also includes some additional support for working with Docker containers. If you seek out and install PowerShell 7, keep in mind that the two versions are different executable files:

- PowerShell 5.1 = powershell.exe
- PowerShell 7 = pwsh.exe

## Launching PowerShell

The first thing we need to do is get PowerShell opened up to start using it. The PowerShell console is installed by default in all recent versions of Windows, so you can run it from the Start menu, pin it to the Desktop, or access it in any way that you normally open any application.

Since I tend to prefer using my keyboard for everything, the way that I normally open PowerShell is to hold down the WinKey and press R to open a Run prompt, type the word powershell, and press Enter:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_473_0.png)

Figure 11.3: Launching PowerShell with only your keyboard

As you can see, since I am logged in to an administrative account on my server, my PowerShell prompt has been opened with elevated permissions. The word **Administrator** is listed in the top toolbar of the PowerShell window. It is important to note that, just like Command Prompt, you can open a PowerShell prompt with either regular user permissions or elevated administrator privileges. It is generally safer to work from within a regular PowerShell session that does not have elevated rights unless the task that you are trying to accomplish requires those extra permissions.

Another quick and easy way to open PowerShell on any newer Windows platform is by right-clicking on the Start button and selecting it right from the quick-tasks list that is presented. As you can see in Figure 11.4, I have right-clicked on the Start button of one of my new Server 2019 machines and can choose from here to open PowerShell or even an elevated (administrative) PowerShell prompt:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_474_0.png)

Figure 11.4: Launching PowerShell from the quick admin tasks menu

If you right-click on your Start button and do not find options for PowerShell but rather for opening Command Prompt, do not be dismayed. This is a configurable option; you can show either Command Prompt or PowerShell options in the quick admin tasks menu. If you right-click on your taskbar and select Taskbar settings, you will find an option that is called Replace Command Prompt with Windows PowerShell in the menu when I right-click the start button or press Windows key+X. Toggling this option will swing your quick admin menu back and forth between the two command-line interfaces.

You also have the option of entering into a PowerShell prompt from inside an existing Command Prompt window. Normally, when you are working from Command Prompt, you cannot make use of any PowerShell cmdlets. Let's go ahead and give this a shot. Open an administrative Command Prompt window and try to type in the name of one of the cmdlets we mentioned earlier. Perhaps type Get-NetIPAddress to show us what IP addresses reside on this system. Whoops—that failed because Command Prompt doesn't recognize the Get-NetIPAddress cmdlet.

Now type the word powershell and press Enter. Instead of opening a separate PowerShell window, your prompt changes, but the application window itself remains the same. You have now entered the PowerShell shell from inside the black and white Command Prompt window, and you can start utilizing cmdlets as you wish. Running Get-NetIPAddress again now produces some information:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_475_0.png)

Figure 11.5: Invoking PowerShell from within Command Prompt

You can move from PowerShell mode back to regular Command Prompt mode by typing exit.

## Default execution policy

When you are working with the PowerShell command-line interface directly, you can simply open up PowerShell, start typing cmdlets, and start getting work done. However, one of the big advantages of using PowerShell comes when you start playing around with creating, saving, and running scripts. If you open up PowerShell, create a script, and then try to run it, you will sometimes find that it fails with a big messy error message, such as this one:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_475_1.png)

Figure 11.6: A common PowerShell script execution error

This shouldn't happen on a fresh instance of Windows Server 2019 but could if you have any GPOs being applied to your new server or if you are using a different operating system and are trying to run some PowerShell scripts; you might find yourself stuck at one of these error messages right out of the gate. While the nature of some versions of Windows to block the running of scripts by default is a security enhancement, it can be a nuisance to work around when you are trying to get something done. Thankfully, if you do encounter this problem, the resolution is easy: you simply need to adjust the Default Execution Policy (DEP) inside PowerShell so that it allows the execution of scripts to happen properly.

This is not a simple ON/OFF switch. There are five different levels within the DEP, and it is important to understand each one so that you can set your DEP accordingly, based on the security that you want in place on your servers. Here are descriptions of each level, in order of most to least secure.

### Restricted

The Restricted policy allows individual commands and cmdlets to be run but stops the running of scripts altogether.

### AllSigned

This requires that any script being run needs to be signed by a trusted publisher. When set to AllSigned, even scripts that you write yourself will have to be put through that validation process and signed before they will be allowed to run.

### RemoteSigned

RemoteSigned is the default policy in Windows Server 2019. For scripts that have been downloaded from the internet, it requires that these scripts are signed with a digital signature from a publisher that you trust. However, if you choose to create your own scripts, it will allow these local scripts to run without requiring that digital signature.

### Unrestricted

Scripts are allowed to run, signed or unsigned. You do still receive a warning prompt when running scripts that have been downloaded from the internet.

### Bypass mode

In Bypass mode, nothing is blocked and no warnings are given when you run scripts. In other words, you're on your own.

Sometimes a single execution policy doesn't meet all of your needs, depending on how you utilize PowerShell scripts. DEPs can be further enhanced by setting an Execution Policy Scope that allows you to set different execution policies to different aspects of the system. For example, the three scopes that you can manipulate are Process, CurrentUser, and LocalMachine. By default, the DEP affects LocalMachine so that any scripts running adhere to the DEP. But if you need to modify this behavior so that different DEPs are set for CurrentUser or even an individual process, you have the ability to do that.

If you are unsure about the current status of your DEP or suspect that someone may have changed it, you can easily view the currently assigned execution policy with a simple cmdlet called Get-ExecutionPolicy. As you can see in Figure 11.7, mine is set to Restricted, which explains my earlier error message when I tried running a script:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_477_0.png)

Figure 11.7: ExecutionPolicy is currently set to Restricted

Once you have decided on the level of DEP that you want on your server or workstation, you can set it accordingly with a quick cmdlet. For example, since this is a test lab and I want scripts to be able to run, and I am not really concerned about security since I am isolated, I am going to change mine to Unrestricted. Here is my command for doing just that:

```
Set-ExecutionPolicy Unrestricted
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_477_1.png)

Figure 11.8: Setting ExecutionPolicy to Unrestricted
