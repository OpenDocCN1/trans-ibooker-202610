# FIGURE 6.10 Using TigerVNC

![](bbox=[0.14, 0.135, 0.845, 0.495])

When configuring your VNC server, be sure to employ openSSH port forwarding for the VNC server ports (covered later in this chapter). Also configure your firewalls to allow traffic through port 22 (or whatever port number you are using for SSH traffic).

# Grasping Xrdp

Xrdp is an alternative to VNC. It supports the Remote Desktop Protocol (RDP) and uses X11rdp or Xvnc to manage the GUI session.

Xrdp provides only the server side of an RDP connection. It allows access from several RDP client implementations, such as rdesktop, FreeFDP, and Microsoft Remote Desktop Connection.

Xrdp comes systemd-ready, so you can simply install, enable, and start the server using the systemctl commands. The package name on Linux is xrdp. Note that it may not be in your Linux distribution’s standard repositories.

After installing and starting the Xrdp server, adjust the firewall so that traffic can access the standard RDP port (TCP 3389). Now direct your RDP client choice to the server via its hostname or IP address, and if necessary, provide the client the RDP port number.

Depending on your RDP client, you may be presented with a screen that denotes that the server is not trusted. If this is the server you just set up, you are fine to continue. You will need to enter the Linux system’s user authentication information, but the login screen depends on the Xrdp client software you are using. An example of Xrdp in action is shown in Figure 6.11.