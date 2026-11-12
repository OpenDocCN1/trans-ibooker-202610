In Listing 5.19, using super user privileges, the systemctl isolate command caused the system to jump from the default system target to the multi-user target. Unfortunately, there is no simple command to show your system's current target in this case. However, the systemctl status command is useful. If you employ the command and give it the previous target's name (graphical.target in this case), you should see that it is no longer active and thus not the current system target. Notice that a short history of the graphical target's starts and stops is also shown in the status display.

The systemctl isolate command can be used only with certain targets. The target's unit file must have the AllowIsolate=yes directive set.

Two extra special targets are rescue and emergency. These targets, sometimes called modes, are described here:

**Rescue Target** When you jump your system to the rescue target, the system mounts all the local filesystems, only the root user is allowed to log into the system, networking services are turned off, and only a few other services are started. The systemctl is-system-running command will return the maintenance status. Running disk utilities to fix corrupted disks is a useful task in this particular target. This target is similar to the SysVinit single-user mode, covered later in this chapter.

**Emergency Target** When your system goes into emergency mode, the system mounts only the root filesystem, and it mounts it as read-only. Similar to rescue mode, it only allows the root user to log into the system, networking services are turned off, and only a few other services are started. The systemctl is-system-running command will return the maintenance status. If your system goes into emergency mode by itself, there are serious problems. This target is used for situations where even rescue mode cannot be reached.

Be aware that if you jump into either rescue or emergency mode, you'll only be able to log into the root account. Therefore, you need to have the root account password. Also, your screen may go blank for a minute, so don't panic. An example of jumping into emergency mode is shown in Listing 5.20.

# Listing 5.20: Jumping to the emergency target unit

```
# systemctl isolate emergency
Welcome to emergency mode! After logging in, type "journalctl -xb" to view
system logs, "systemctl reboot" to reboot, "systemctl default" or ^D to
try again to boot into default mode.
Give root password for maintenance
(or type Control-D to continue):
#
# systemctl is-system-running
maintenance
#
```