You can see in Listing 5.7 that this system is actually using systemd. You can verify this
by taking a look at PID 1, as shown in Listing 5.8.

# Listing 5.8: Checking PID 1

```
# ps -p 1
PID TTY          TIME CMD
1 ?        00:00:06 systemd
#
```

In Listing 5.8, the ps utility is used. This utility allows you to view processes. A process
is a running program. The ps command shows you what program is running for a par-
ticular process in the CMD column. In this case, the systemd program is running. Thus, this
Linux system is using systemd.

The init program or systemd is the parent process for every service on
a Linux system. If your system has the pstree program installed, you can
see a diagram depicting this relationship by typing pstree -p 1 at the
command line.

Keep in mind that these discovery methods are not foolproof. Also, there are other
system initialization methods, such as the now-defunct Upstart that used the initctl
utility as its daemon interface. The following brief list shows a few Linux distribution
versions that used Upstart:

+   - Fedora v9–v14
+   - openSUSE v11.3–v12.2
+   - RHEL v6
+   - Ubuntu v6.10–v15.04

If you are using the distribution versions recommended in Chapter 1, know that those
distributions are all systemd systems.

# Using the systemd Initialization
Process

The systemd approach introduced a major paradigm shift in how Linux systems manage
services. Services can now be started when the system boots, when a particular hardware
component is attached to the system, when certain other services are started, and so on.
Some services can be started based upon a timer.