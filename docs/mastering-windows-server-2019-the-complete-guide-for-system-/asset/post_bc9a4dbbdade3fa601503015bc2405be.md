
## Installing Windows Server 2019 onto a new server

First, just like you have already done, we need to prepare our first server by getting the Windows Server 2019 operating system installed. Refrain from installing any full roles onto the server, because, depending on the role and its unique configuration, the Sysprep process that we run shortly could cause problems for individual role configurations. Install the operating system and make sure device drivers are all squared away, and you're ready for the next step.

## Configuring customizations and updates onto your new server

Next, you want to configure customizations and install operating system updates onto your new server. Each setting or installation that you can do now that is universal to your batch of servers will save you from having to take that step on your servers in the future. This portion may be slightly confusing because I just told you a minute ago not to install roles onto the master server. This is because a role installation makes numerous changes in the operating system, and some of the roles that you can install lock themselves down to a particular hostname running on the system. If you were to do something like that to a master server, that role would more than likely break when brought up on a new server. Customizations that you can put into place on the master server are things such as plugging in files and folders that you might want on all of your servers, such as an Admin Tools folder or something like that. You could also start or stop services that you may or may not want running on each of your servers, and change settings in the registry if that is part of your normal server prep or hardening process. Whatever changes or customizations you put into place, it's not a bad idea to run a full slew of tests against the first new server that you build from this master image, just to make sure all of your changes made it through the Sysprep process.

Now is also the time to let Windows Update install and to put any patches on this new server that you want to have installed on all of your new servers in the future. There is nothing more frustrating than installing a new operating system in 5 minutes, only to have to sit around and wait 4 hours for all of the current updates and patches to be installed before you can use the new server. By including all of these updates and patches in the master image, you save all of that download and installation time for each new server that you spin up.

> Continue to save yourself time and effort by creating new copies of your master images every few months. This way the newest patches are always included in your master image and it continues to save you more and more time throughout the life of Windows Server 2019.

## Running Sysprep to prepare and shut down your master server

Now that our master server is prepped how we want, it is time to run the Sysprep tool itself. To do that, open up an administrative Command Prompt and browse to C:\Windows\System32\Sysprep. Now you can make use of the Sysprep.exe utility inside that folder to launch Sysprep itself.

As with many executables that you run from Command Prompt, there are a variety of optional switches that you can tag onto the end of your command to make it do specific tasks. From your Command Prompt window, if you simply run the sysprep.exe command, you will see a graphical interface for Sysprep, where you can choose between the available options, as shown in Figure 2.36:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_110_0.png)

Figure 2.36: Sysprep options

Since I always use the same set of options for Sysprep, I find it easier to simply include all of my optional switches right from the command-line input, therefore bypassing the graphical screen altogether. Here is some information on the different switches that are available to use with sysprep.exe:

- `/quiet`: This tells Sysprep to run without status messages on the screen.
- `/generalize`: This specifies that Sysprep is to remove all of the unique system information (SID) from the Windows installation, making the final image usable on multiple machines in your network, because each new one spun up from the image will get a new, unique SID.
- `/audit`: This restarts the machine into a special audit mode, where you have the option of adding additional drivers into Windows before the final image gets taken.
- `/oobe`: This tells the machine to launch the mini-setup wizard when Windows next boots.
- `/reboot`: This restarts when Sysprep is finished.
- `/shutdown`: This shuts down the system (not a restart) when Sysprep is finished. This is an important one and is one that I typically use.
- `/quit`: This closes Sysprep after it finishes.
- `/unattend`: There is a special answerfile that you can create that, when specified, will be used in conjunction with the Sysprep process to further configure your new servers as they come online. For example, you can specify in this answerfile that a particular installer or batch file is to be launched upon the first Windows boot following Sysprep. This can be useful for any kind of cleanup tasks that you might want to perform, for example, if you had a batch file on your system that you used to flush out the log files following the first boot of new servers.

The two that are most important for our purpose of wanting to create a master image file that we can use for quick server rollouts in the future are the `/generalize` switch and the `/shutdown` switch. `/generalize` is very important because it replaces all of the unique identification information, the SID info, in the new copies of Windows that come online. This allows your new servers to co-exist on the network with your original server, and with other new servers that you bring online. The `/shutdown` switch is also very important because we want this master server to become sysprepped and then immediately shut down so that we can create our master image from it.

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_111_0.png)

> Make sure that your server does NOT boot into Windows again until after you have created your master image or taken your master copy of the .VHDX file. The first time that Windows boots it will inject the new SID information, and you want that only to happen on new servers that you have created based on your new image.

So, rather than simply throwing all of the switches at you and letting you decide, let's take a look at the ones that I typically use. I will make use of /generalize so that I make my new servers unique, and I also like to use /oobe so that the mini-setup wizard launches during the first boot of Windows on any of my new systems. Then, I will of course also use /shutdown, because I need this server to be offline immediately following Sysprep so that I can take a copy of the hard drive to be used as my master image. So, my fully groomed sysprep command is shown in the following code:

```
sysprep.exe /generalize /oobe /shutdown
```

After launching this command, you will see Sysprep moving through some processes within Windows, and after a couple of minutes, your server will shut itself down, as shown in Figure 2.37:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_112_0.png)

Figure 2.37: Sysprep and shutting down

You are now ready to create your master image from this hard disk.

## Creating your master image of the drive

Our master server is now shut down, and we are ready to create our master image from this server. If it is a physical server, you can use any hard disk imaging utility in order to create an image file from the drive. An imaging utility like those from the company Acronis will create a single file from your drive. This file contains an image of the entire disk that you can use to restore down onto fresh hard drives in new servers in the future. On the other hand, most of you are probably dealing with virtual servers most often in your day-to-day work lives and prepping new servers in the virtual world is even easier.
