
cover a few of these desktop environments in this chapter and look at universal accessibility to them as well.

## The X GUI Login System

By default, when a Linux system boots, it presents a text login console. This works just fine for server environments but is somewhat lacking in today's desktop world. Most desktop users prefer some type of graphical login screen to appear, allowing them to select a graphical desktop environment (if more than one is available on the system), and log in using their user ID and password.

The display manager component is responsible for controlling the graphical login feature. Just about every desktop Linux distribution installs a display manager package that starts at boot time to present a graphical login environment. Every Linux display manager package uses the X Display Manager Control Protocol (XDMCP) to handle the graphical login process. Figure 6.2 shows the display manager used in Ubuntu.

## FIGURE 6.2 The Ubuntu display manager login screen

![](img/ca7617340ddbe2360d36ce3428729fa9_364_0.png)

The X Display Manager (XDM) package is the basic display manager software available for Linux. It presents a generic user ID and password login screen, passing the login attempt off to the Linux system for verification. If the system authenticates the login attempt, XDM starts up the appropriate X server environment and Windows desktop environment.

Although the XDM display manager is somewhat generic, there are some configuration features you can modify to change things a bit. The main configuration file is /etc/X11/xdm/xdm-config. In most situations, you'll never need to modify any of these settings.

Most window managers create their own display manager and expand the capabilities of the XDM display manager. Here are a few of the more popular display managers you'll see:

- KDM: The default display manager used by the KDE desktop environment
- GDM: The default display manager used by the GNOME desktop environment
- LightDM: A bare-bones display manager used in lightweight desktop environments such as Xfce

## Common Linux Desktop Environments

Unlike other operating systems, Linux provides a wealth of different desktop environments for you to choose from. While most Linux distributions focus on a single desktop environment, you're still free to install a different desktop environment yourself.

This section covers the most common desktop environments you'll encounter in the various Linux distributions.

## Getting to Know GNOME

The GNOME desktop environment was created around the late 1990s. It is very popular and is found by default on Linux distributions such as CentOS and Ubuntu. Currently it is maintained by a large group of volunteers who belong to the GNOME Foundation. You can find out more about the GNOME project at www.gnome.org.

The year 2011 was a pivotal time in GNOME's history. GNOME 2 was a more traditional desktop user interface, and when GNOME 3 (now formally called GNOME Shell) was released in 2011, with its nontraditional interface, many users reacted strongly. This spurred a few GNOME project forks. However, over time and with a few changes, GNOME Shell gained ground. For those who still prefer the traditional desktop user interface that GNOME 2 provided, the GNOME Classic desktop environment is available.

## FIGURE 6.3 The GNOME Shell desktop environment

![](img/ca7617340ddbe2360d36ce3428729fa9_366_0.png)

Keep in mind that a default desktop environment may be modified slightly for each Linux distribution. For example, GNOME Shell on CentOS does not have a Favorites Bar displaying unless you click Activities in the panel, whereas GNOME Shell on Ubuntu automatically displays the Favorites Bar.

To better understand a particular graphical interface, try out a desktop environment for yourself. However, to help you with memorizing the assorted components making up these different desktops, we are providing tables where you can compare and contrast the distinct graphical environments. Some of the GNOME Shell's various components are briefly described in Table 6.1.

| Name | Program name and/or description |
| :--- | :--- |
| Display Manager | GNOME Display Manager (GDM). |
| File Manager | GNOME Files (sometimes just called Files). Formerly called Nautilus. |
| Favorites Bar | GNOME Shell Dash (sometimes called the Dock). |
| Panels | A single panel located at GNOME Shell frame’s top. |
| System Tray | Located on the right-hand side of the single panel. |
| Windows Manager | Mutter. |

An interesting feature of the GNOME Shell is that the panel, which contains the system tray, is available on the Display Manager as well as within the GNOME Shell.

## Probing KDE Plasma

The Kool Desktop Environment (KDE) got its start in 1996, with its first version released in 1998. Through time, the name KDE was no longer just referring to a desktop environment, but instead it specified the project’s organization and the strong community that supported it. KDE had many additional software projects besides its famous desktop environment. Thus, in 2009 KDE’s desktop environment was rebranded as KDE Plasma. You can find out about the KDE group as well as its various products at www.kde.org.

## FIGURE 6.4 The KDE Plasma desktop environment

![](img/ca7617340ddbe2360d36ce3428729fa9_367_0.png)

In Figure 6.4, notice that the panel is located at the primary UI frame's bottom. This is a more traditional panel location used on older systems and one of the reasons KDE Plasma is known for being a good desktop environment for those who are new to Linux. On this panel, the system tray, which contains notifications, the time, and various other plasmoids (widgets), is located on the panel's right side. The Application menu, a launcher for various programs as well as containing the Favorites Bar, is on the panel's far-left side. Table 6.2 briefly describes some of the KDE Plasma components.

| Name | Program name and/or description |
| --- | --- |
| Display Manager | SDDM (Simple Desktop Display Manager) |
| File Manager | Dolphin |
| Favorites Bar | Displayed inside Application menu |
| Panels | A single panel located at the Plasma frame's bottom |
| System Tray | Located on the right side of the single panel |
| Widgets | Called Plasmoids |
| Windows Manager | Kwin |

To help those users who are familiar with accessing their files via folder icons, KDE Plasma offers a folder view. You can see folders depicted in the default UI on the openSUSE Leap distribution in Figure 6.4. These icons on the primary desktop window allow you to launch the Dolphin file manager and jump straight to the directory named on the folder icon.

> Many desktop environments have multiple UIs available for each user called workspaces. Workspaces are individual desktops. For example, you can have two GUI apps open on one workspace and just a terminal emulator open on the other workspace. Switching between the workspaces can be done via mouse clicks or keystroke combinations, such as Ctrl+Alt+Up arrow or Down arrow on Fedora 28's Wayland desktop environment. Using multiple workspaces can be very handy, especially if you need to quickly look productive at work when your boss walks by.

## Considering Cinnamon

The Cinnamon desktop environment got its start in 2011, when many users reacted strongly to the release of GNOME 3 (now GNOME Shell). Developers of the Mint Linux distribution began creating Cinnamon as a fork of GNOME 3. It was officially “GNOME-free” as of late 2013. Cinnamon is still managed by the Mint development team, and you can find out more at their website: www.linuxmint.com.
