The Wayland protocol was initially released back in 2009, and it is now used by many current Linux desktop environments such as GNOME Shell and KDE Plasma. If you really want to dig down into Wayland, visit its website at https://wayland.freedesktop.org/.

# Checking Your Display Server

If you are not sure what display server your desktop environment is currently using, X11 or Wayland, you can quickly determine the answer. The following steps will guide your discovery.

+   1. Log in to your system’s GUI. This will start a GUI session for you.
+   2. Open a terminal emulator application.
+   3. Type echo $WAYLAND_DISPLAY at the command line and press the Enter key. If you get no response and just a command-line prompt back, most likely your system is using X11. If you receive a response, then your desktop environment is probably using Wayland. An additional test will help you ensure what is in use.
+   4. You need to get the GUI session number, so type loginctl and press Enter. Note the session number.
+   5. Type the command loginctl show-session session-number -p Type at the command line, where session-number is the number you obtained in the previous step. If you receive Type=Wayland, then your desktop environment is using Wayland. If instead you receive Type=X11, then your system is using the X11 display server.

The Wayland compositor is Weston. However, Weston provides a rather basic desktop experience. It was created as a Wayland compositor reference implementation. For those of you who are unfamiliar with the term reference implementation, it means that Weston was created to be a compositor requirements example for those developers who want to create their own Wayland compositor. Thus, Weston’s core focus is correctness and reliability.

Wayland’s compositor is swappable. In other words, you can use a different compositor if you need a more full-featured desktop experience. Several compositors are available for use with Wayland, including Arcun, Sway, Lipstick, and Clayland, to name a few. However, you may not need to go out and get a Wayland compositor. Many desktop environments create their own Wayland compositors, which is typically embedded within their windows manager. For example, Kwin and Mutter both fully handle Wayland compositor tasks.

> If you have any legacy X11 applications that will not support Wayland, do not despair. There is the XWayland software, which is available in the Weston package. XWayland allows X-dependent applications to run on the X server and display via a Wayland session.