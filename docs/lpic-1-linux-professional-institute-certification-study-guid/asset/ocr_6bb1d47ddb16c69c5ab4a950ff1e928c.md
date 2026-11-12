+   - Device: Configures the session’s video card(s)
+   - Screen: Sets the session’s screen resolution and color depth
+   - Module: Denotes any modules that need to be loaded
+   - Files: Sets file path names, if needed, for fonts, modules, and keyboard layout files
+   - Server Flags: Configures global X server options
+   - Server Layout: Links together all the session’s input and output devices

Keep in mind that many desktop environments also provide dialog boxes in their UI, which allow you to configure your GUI X sessions. Most likely you will have no need to create or tweak the X11 configuration file. However, if you really want to dig into the X11 configuration file’s details, view its man page via the man 5 xorg.conf command.

If you need to troubleshoot X problems, the X.Org package provides some help. If something goes wrong with the display process, the X.Org server generates the .xsession-errors file in your Home directory (often referred to as ~/.xsession-errors). This is a standard text log file that indicates what went wrong with the X.Org server.

In addition, two utilities are available that can help: xdpyinfo and xwininfo. The xdpyinfo command provides information about the X.Org server, including the different screen types available, the default communicate parameter values, protocol extension information, and so on.

The xwininfo utility provides window information. If no options are given, an interactive utility asks you to click on the window for which you desire statistics. The displayed stats include location information, the window’s dimensions (width and height), color map ID, and so on.

> **WARNING** Be aware that the xwininfo command will hang if you are running a Wayland session instead of an X.Org session. Press Ctrl+C to exit out of the hung command.

> **NOTE** If your X.Org session hangs for any reason, you can reset it by pressing the Ctrl+Alt+Backspace key combination. This restarts the X server, which will attempt to auto-detect the hardware again and generate the configuration file.

# Figuring Out Wayland

Wayland is a replacement for the X.Org display server. It is designed to be simpler, more secure, and easier to develop and maintain than the X.Org software. Wayland specifically defines the communication protocol between a display server and its various clients. However, Wayland is also an umbrella term that covers the compositor, the window server, and the display server.