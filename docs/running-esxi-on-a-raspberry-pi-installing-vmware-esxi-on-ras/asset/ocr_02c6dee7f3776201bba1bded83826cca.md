7. Start a graphics terminal. In the search box at the top of the desktop, enter XTerm.

![](bbox=[0.15, 0.18, 0.8, 0.35])

8. In the XTerm terminal, start a graphics application by entering /usr/bin/xeyes.

9. Verify that the eyes follow the mouse.

10. Close XTerm.

# Ubuntu

To use the Ubuntu VM, it will need a graphical desktop installed on it. Ubuntu has different desktops that can be installed, and the following are instructions for installing a basic GNOME desktop:

+   1. Log in to your Ubuntu instance from the console.
+   2. Switch to root user, and then enter sudo su.
+   3. Install the GNOME desktop and some applications by entering

```
apt install -y gnome-session gdm3
apt install -y libreoffice
apt install -y x11-apps
apt-get install -y gnome-terminal
```