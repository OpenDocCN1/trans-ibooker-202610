
## Two ways to do the same thing

Potentially confusing as well, until you get used to navigating around in here, is that you can sometimes accomplish the same task in either **Control Panel** or the **Settings** menu, but the process that you take in each interface can have a vastly different look and feel. Let's take a look at that firsthand by trying to create a new user account on our server, once via **Control Panel**, and again via **Settings**.

### Creating a new user through Control Panel

You are probably pretty familiar with this. Open **Control Panel** and click on **User Accounts**. Then, click on the **User Accounts** heading. Now, click on the link to **Manage another account**. Inside this screen is your option to **Add a user account**. Click on that and you get the dialog box where you enter a username and password for your new user:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_57_0.png)

Figure 1.13: Adding a user account via Control Panel

### Creating a new user through the Settings menu

Let's take this newer Settings interface for a test drive. Open the Settings menu and click on Accounts. Now, click on Other users in the left column. There is an option here to Add someone else to this PC; go ahead and click on that:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_58_0.png)

Figure 1.14: Adding a user account via Settings

What in the world is that? Not what I expected, unfortunately. To my surprise, the old Control Panel user account launches a nice, fresh-looking interface from which I can create new user accounts. Accessing user accounts via the newer Settings console launches me into the old Local Users and Groups manager. Technically, from here I could definitely go ahead and create new user accounts, but it seems like there is some sort of a disconnect here. You would naturally think that the new Settings would initiate the newer, nicer screen for adding new user accounts, but we found the opposite to be true.

We walked through this simple example of attempting to perform the same function through two different interfaces to showcase that there are some items that can and must be performed within the new Settings menu context, but there are many functions within Windows that still need to be accomplished through our traditional interfaces. While Control Panel continues to exist, and probably will for a very long time, you should start navigating your way around the Settings menu and figure out what is available inside, so that you can start to shape your ideas for the best combination of both worlds in order to manage your servers effectively.

Just one last thing to point out as we start getting comfortable with the way that the new **Settings** menus look: many of the settings that we configure in our servers are on/off types of settings. By that I mean we are setting something to either one option or another. Historically, these kinds of configurations were handled by either drop-down menus or by radio buttons. That is normal; that is expected; that is Windows. Now, you will start to see little swipe bars, or sliders, that allow you to switch settings on or off, like a light switch. Anyone who has used the settings interface of any smartphone knows exactly what I am talking about. This user interface behavior has now made its way into the full Windows operating systems and is probably here to stay. Just to give you an idea of what it looks like inside the context of the new **Settings** menu, here is a screenshot of the current **Windows Update** settings page inside the **Update & Security** settings.

This is a good example of those on/off slider buttons:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_59_0.png)

Figure 1.15: Settings' Advanced options

## Task Manager

**Task Manager** is a tool that has existed in all Windows operating systems since the first days of the graphical interface, but it has evolved quite a bit over the years. One of the goals for Windows Server 2019 is to be even more useful and reliable than any previous version of Windows Server has been. So, it only makes sense that we finally remove Task Manager altogether, since it simply won't be needed anymore, right?

I'm kidding, of course! While Server 2019 will hopefully prove itself to indeed be the most stable and least needy operating system we have ever seen from Microsoft, Task Manager still exists and will still be needed by server administrators everywhere. If you haven't taken a close look at Task Manager in a while, it has changed significantly over the past few releases.

**Task Manager** is still typically invoked by either a *Ctrl + Alt + Del* on your keyboard then clicking on **Task Manager**, or by right-clicking on the taskbar and then choosing **Task Manager**. You can also launch Task Manager with the key combination *Ctrl + Shift + Esc* or typing *taskmgr* inside the **Run** or **Search** dialog boxes. The first thing you'll notice is that very little information exists in this default view, only a simple list of applications that are currently running. This is a useful interface for forcing an application to close that may be hung up, but not for much else. Go ahead and click on the **More details** link, and you will start to see the real information provided in this powerful interface.

We immediately notice that the displayed information is more user-friendly than in previous years, with both **Apps** and **Background processes** being categorized in a more intuitive way and multiple instances of the same application being condensed down for easy viewing. This gives a faster overhead view of what is going on with our system, while still giving the ability to expand each application or process to see what individual components or windows are running within the application, such as in Figure 1.16:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_61_0.png)

Figure 1.16: Task Manager: Processes

Make sure to check out the other tabs available inside Task Manager as well. Users will show us a list of currently logged-in users and the amounts of hardware resources that their user sessions are consuming. This is a nice way to identify on a Remote Desktop Session Host server, for example, an individual who might be causing a slowdown on the server. The Details tab is a little bit more of a traditional view of the Processes tab, splitting out much of the same information but in the older style we were used to seeing in versions of the operating system long ago. Then, the Services tab is pretty self-explanatory; it shows you the Windows services currently installed on the server, their status, and the ability to start or stop these services as needed, without having to open the Services console separately.

The tab that I skipped over so that I could mention it more specifically here is the **Performance** tab. This is a pretty powerful one. Inside, you can quickly monitor CPU, memory, and Ethernet utilization. As you can see in the following screenshot, I haven't done a very good job of planning resources on this particular virtual machine, as my CPU is hardly being touched but I am almost out of system memory:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_62_0.png)

Figure 1.17: Task Manager: Performance

> Another useful piece of information available inside this screen is server uptime. Finding this information can be critical when troubleshooting an issue, and I watch admins time and time again calculating system uptime based on log timestamps. Using **Task Manager** is a much easier way to find that information!

If you are interested in viewing more in-depth data about server performance, there is a link at the bottom of this **Task Manager** window where you can **Open Resource Monitor**. Two technologies provided inside Server 2019 for monitoring system status, particularly for hardware performance, are **Resource Monitor** and **Performance Monitor**. Definitely open up these tools and start testing them out, as they can provide both troubleshooting information and essential baseline data when you spin up a new server. This baseline can then be compared against future testing data so that you can monitor how new applications or services installed on a particular server have affected their resource consumption. We will discuss these additional monitoring tools in *Chapter 15, Troubleshooting Windows Server 2019*.

Moving back to **Task Manager**, there is just one other little neat trick I would like to test. Still inside the **Performance** tab, go ahead and right-click on any particular piece of data that you are interested in. I will right-click on the **CPU** information near the left side of the window. This opens up a dialog box with a few options, of which I am going to click on **Summary view**. This condenses the data that was previously taking up about half of my screen real-estate, into a tiny little window, which I can move to the corner of my screen. This is a nice way to keep hardware utilization data on the screen at all times as you navigate through and work on your server so that you can watch for any spikes or increases in resource consumption when making changes to the system:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_63_0.png)

Figure 1.18: Task Manager: Resources
