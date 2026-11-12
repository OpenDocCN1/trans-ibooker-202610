Originally in Linux there was only one software package that supported X, called XFree86. However, in 2004 the XFree86 project changed their licensing requirements, which caused many Linux distributions to switch to the X.Org foundation's implementation of X, simply called X.Org.

The X.Org's display server fully implements the X Window System version 11 standards, including using the same configuration file format as the original XFree86 package. However, many distributions created their own customizations of the X.Org server; thus, you will see a wide variety of names concerning the Linux X display server, such as X.org-X11, X, X11, and X.Org Server. We'll use either X or X11 in this chapter.

Within the past few years, a new X display server package called Wayland has made headway in the Linux world. The Wayland package provides more advanced features that support newer display hardware and security, and supports additional types of input devices. Wayland is quickly gaining followers in the Linux world and may soon become the default display server used in most Linux distributions.

This section walks through the basics of both the X.Org and Wayland display servers.

# ## Examining X.Org

The X.Org package keeps track of display card, monitor, and input device information in a configuration file, using the original XFree86 format. The primary configuration file is /etc/X11/xorg.conf, though the file is sometimes stored in the /etc/ directory. Typically, however, this file is no longer used. Instead, individual applications or devices store their own X11 settings in separate files stored in the /etc/X11/xorg.conf.d directory. When the X11 server boots, it reads the configuration settings stored in those files to customize how it interacts with different display cards, monitors, keyboards, mice, and other input or output devices.

Don't be surprised, though, if you go looking on your Linux system for either the xorg.conf file or the xorg.conf.d directory and don't find either of them. The X.Org software can create the session configuration on the fly using runtime auto-detection of the hardware involved with each GUI's session. These days, the X.Org software can detect most common hardware devices, so no manual configuration is required.

However, in some cases, auto-detection might not work properly, and you need to make X11 configuration changes. In this case, you can manually create the configuration file. To do this, shut down the X Server by going to a command prompt, using the command `sudo telinit 3` (this usually works on both SysVinit and systemd systems), and using super user privileges to generate the file via the `Xorg -configure` command. The file, named xorg.conf.new, will be in your local directory. Make any necessary tweaks, rename the file, move the file to its proper location, and restart the X server.

The xorg.conf file has several sections. Each section contains important configuration information:

+   - Input Device: Configures the session's keyboard and mouse
+   - Monitor: Sets the session's monitor configuration
+   - Modes: Defines video modes