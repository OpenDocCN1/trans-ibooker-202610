
You can see all three of these functions in Figure 1.1:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_46_0.png)

Figure 1.1: The new Start menu

Now that is a breath of fresh air. A simple but useful Start menu, and more importantly, one that loads quickly over remote connections such as RDP or Hyper-V consoles.

## The Quick Admin Tasks menu

As nice as it is to have a functional Start menu, as a server administrator I still very rarely find myself needing to access the traditional menu for my day-to-day functions. This is because many items that I need to access are quickly available to me inside the Quick Admin Tasks menu, which opens by simply right-clicking on the Start button. This menu has been available to us since the release of Windows 8, but many IT professionals are still unaware of this functionality.

This menu has become an important part of my interaction with Windows Server operating systems, and hopefully, it will be for you as well. Right-clicking on the Start button shows us immediate quick links to do things like open Event Viewer, view the System properties, check Device Manager, and even Shut down or Restart the server. The two most common functions that I call for in this context menu are the Run function and using it to quickly launch a PowerShell prompt. Even better is the ability from this menu to open either a regular user context PowerShell prompt or an elevated/administrative PowerShell prompt. Using this menu properly saves many mouse clicks and shortens troubleshooting time:

> Alternatively, this menu can be invoked using the WinKey + X keyboard shortcut!

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_47_0.png)

Figure 1.2: The Quick Admin Tasks menu

While I find it extremely beneficial to have PowerShell options listed here in the Quick Admin Tasks menu, you may still prefer using Command Prompt over PowerShell. There is an easy way to swap out the PowerShell options in the Quick Admin Tasks menu with Command Prompt options – both regular user context as well as admin context. To make that change, take the following steps:

- 1. Right-click on the taskbar and select Taskbar settings.
- 2. Disable the option (it is enabled by default) to Replace Command Prompt with Windows PowerShell in the menu when I right-click the start button or press Windows key+X:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_48_0.png)

Figure 1.3: Replace Command Prompt with Windows PowerShell in the menu

## Using the Search function

While the Quick Admin Tasks menu hidden behind the Start button is useful for calling common administrative tasks, using the Search function inside the Start menu is a powerful tool for interfacing with literally anything on your Windows Server. Depending on who installed applications and roles to your servers, you may or may not have shortcuts available to launch them inside the Start menu. You also may or may not have desktop shortcuts or links to open these programs from the taskbar. I find that it is often difficult to find specific settings that may need to be tweaked in order to make our servers run like we want them to. Control Panel is slowly being replaced by the newer Settings menu in newer versions of Windows, and sometimes this results in the discovery of particular settings being difficult. All of these troubles are alleviated with the search bar inside the Start menu. By simply clicking on the Start button, or even easier by pressing the Windows key (WinKey) on your keyboard, you can simply start typing the name of whatever program, setting, or document that you want to open up. The search bar will search everything on your local server, and present options to you for which application, setting, or even document to open.

As a most basic example, press WinKey on your keyboard, then type notepad and press the Enter key. You will see that good old Notepad opens right up for us. We never had to navigate anywhere in the Programs folder in order to find and open it. In fact, we never even had to touch the mouse, which is music to the ears for someone like me who loves doing everything he possibly can via the keyboard:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_49_0.png)

Figure 1.4: Windows Search

An even better example is to pick something that would be buried fairly deep inside **Settings** or the **Control Panel**. How about changing the amount of time before the screen goes to power save and turns itself off? The traditional server admin will open **Control Panel** (if they can find it), probably navigate to the **Appearance and Personalization** section because nothing else looks obviously correct, and still not find what they were looking for. After poking around for a few more minutes, they would start to think that Microsoft forgot to add in this setting altogether. But alas, these power settings are simply moved to a new container and are no longer accessible through **Control Panel** at all. We will discuss the new **Settings** screen momentarily in this chapter, but ultimately for the purposes of this example, you are currently stuck at the point where you cannot find the setting you want to change. What's the quick solution? Press your *WinKey* to open the **Start** menu, and type monitor (or power, or just about anything else that would relate to the setting you are looking for). You'll see in the list of available options showing in the search menu one called **Choose when to turn off the screen**. Click on that, and you have found the setting you were looking for all along:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_49_1.png)

Figure 1.5: Screen-related settings

You will also notice that you have many more options on this Search screen than what you were originally searching for. Search has provided me with many different items that I could accomplish, all relating to the word monitor that I typed in. I don't know of a more powerful way to open applications or settings on Windows Server 2019 than using the search bar inside the Start menu. Give it a try today!

## Pinning programs to the taskbar

While Windows Server 2019 provides great searching capabilities so that launching hard-to-find applications is very easy, sometimes it's easier to have quick shortcuts for commonly used items to be available with a single click, down in the traditional taskbar. Whether you have sought out a particular application by browsing manually through the Start menu or have used the Search function to pull up the program that you want, you can simply right-click on the program and choose Pin to taskbar to stick a permanent shortcut to that application in the taskbar at the bottom of your screen. Once you have done this, during future logins to your session on the server, your favorite and most-used applications will be waiting for you with a single click. As you can see in Figure 1.6, you also have the ability to pin programs to the Start menu, which of course is another useful place from which to launch them regularly:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_50_0.png)

Figure 1.6: Pinning programs to the taskbar

Many readers will already be very familiar with the process of pinning programs to the taskbar, so let's take it one step further to portray an additional function you may not be aware is available to you when you have applications pinned.

## The power of right-clicking

We are all pretty familiar with right-clicking in any given area of a Windows operating system in order to do some more advanced functions. Small context menus displayed upon a right-click have existed since the two-button mouse rolled off the assembly line.

We often right-click in order to copy text, copy documents, paste the same, or get into a deeper set of properties for a particular file or folder. Many day-to-day tasks are accomplished with that mouse button. What I want to take a minute to point out is that software makers, Microsoft and otherwise, have been adding even more right-click functionality into application launchers themselves, which makes it even more advantageous to have them close at hand, such as inside the taskbar.

The amount of functionality provided to you when right-clicking on an application in the taskbar differs depending on the application itself. For example, if I were to right-click on Command Prompt, I have options to either open **Command Prompt**, or to **Unpin from taskbar**. Very simple stuff. If I right-click again on the smaller menu option for Command Prompt, I have the ability to perform the same functions, but I could also get further into **Properties**, or **Run as administrator**. So, I get a little more enhanced functionality the deeper I go:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_51_0.png)

Figure 1.7: Right-click to Run as administrator
