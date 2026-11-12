# Listing 5.20: Jumping to the emergency target unit (continued)

```
# systemctl list-units --type=target
UNIT               LOAD   ACTIVE SUB     DESCRIPTION
emergency.target   loaded active active Emergency Mode
[...]
#
# systemctl default
#
```

In Listing 5.20, the systemctl command is employed to jump into emergency mode. Notice that you do not have to add the .target extension on the emergency target unit's filename. This is true with all systemd targets. When you reach emergency mode, you must enter the root password at the prompt. When you reach the command line, you can enter commands listed in the welcome display or try out some additional systemctl commands.

> Other targets you can jump to include reboot, poweroff, and halt. For example, just type in systemctl isolate reboot to reboot your system.

Notice in Listing 5.20 that when the systemctl --is-system-running command is issued, the response is maintenance instead of running. Also, when the list-units command is employed, it shows that the emergency.target is active. The systemctl default command will cause the system to attempt to jump into the default target.

# Using the SysV Initialization Process

Many server administrators have gone through the process of moving from a SysVinit system to a systemd system. Recall that systemd is backward compatible with SysVinit, so understanding SysVinit is important.

First, if you want to experiment with the original SysVinit commands without interference from systemd or the now defunct Upstart, find a Linux distribution that uses the SysVinit initialization method. One way to find one is to visit the DistroWatch website and use their search tool at distrowatch.com/search.php. Scroll down to the Search by Distribution Criteria section, and for Init software, select SysV. Any Linux distributions still using SysVinit will display in the search results.

To get clean SysVinit listings for this book, we used a blast from the Linux distribution past, Fedora 7. To grab an ISO copy of this old distribution, visit archives.fedoraproject.org/pub/archive/fedora/linux/releases/.