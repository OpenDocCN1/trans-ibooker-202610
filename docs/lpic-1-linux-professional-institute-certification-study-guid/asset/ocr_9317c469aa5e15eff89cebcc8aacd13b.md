cover a few of these desktop environments in this chapter and look at universal acces-
sibility to them as well.

# The X GUI Login System

By default, when a Linux system boots, it presents a text login console. This works just fine
for server environments but is somewhat lacking in today's desktop world. Most desktop
users prefer some type of graphical login screen to appear, allowing them to select a graphi-
cal desktop environment (if more than one is available on the system), and log in using their
user ID and password.

The display manager component is responsible for controlling the graphical login fea-
ture. Just about every desktop Linux distribution installs a display manager package that
starts at boot time to present a graphical login environment. Every Linux display manager
package uses the X Display Manager Control Protocol (XDMCP) to handle the graphical
login process. Figure 6.2 shows the display manager used in Ubuntu.

# FIGURE 6.2 The Ubuntu display manager login screen

![](bbox=[0.155, 0.425, 0.855, 0.81])

The X Display Manager (XDM) package is the basic display manager software avail-
able for Linux. It presents a generic user ID and password login screen, passing the login
attempt off to the Linux system for verification. If the system authenticates the login