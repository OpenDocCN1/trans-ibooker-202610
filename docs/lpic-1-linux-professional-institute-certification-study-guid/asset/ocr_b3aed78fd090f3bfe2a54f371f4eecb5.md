# # Remote X11 Connections

The simplest way to run an X11 desktop remotely is to just forward the standard X11 desktop protocol stream across the network to the remote client. For example, assume the host workstation1 is located in a remote area but you need to run a graphical program from it using your local host, called workstation2. Follow these steps to accomplish that:

+   1. Log in to the workstation2 host using the standard graphical desktop environment.
+   2. Open a terminal session to obtain a command prompt.
+   3. Type `xhost +workstation1`, where *workstation1* is either the hostname of the remote workstation1 host or its IP address. The xhost command allows the client workstation to receive data from the sending remote workstation.
+   4. Log in to the remote host workstation2 using a secure shell (SSH) connection. This will provide a standard text command prompt interface.
+   5. At the command prompt, type `export DISPLAY=workstation2:0.0`, where *workstation2* is the hostname or IP address of your local workstation. This command redirects any graphical output generated on the remote workstation1 to the local workstation2 X server.
+   6. Launch a graphical application from the command prompt on workstation1. The graphical desktop will appear as a new window in your local workstation2 desktop.
+   7. When you're done, close the launched program and type `xhost -workstation1` on your local workstation2 host to remove the permissions to receive data from the remote host.

> Besides the *xhost* command, you may need to allow connections through any firewall software running on your Linux system. The X11 desktop uses TCP ports in the range of 6000 to 6063. Also, the *xhost* command allows X connections from remote hosts to your local workstation only for the current session. If you regularly send graphical windows from a specific remote host to your local workstation, you can add the entry to the X11 server configuration files. The *xauth* command allows you to add, remove, and list remote hosts in the X11 configuration file.

# # Tunneling Your X11 Connection

Another method that provides remote GUI interactions within a secure tunnel is *X11 forwarding*. X11 forwarding allows you to interact with various X11-based graphical utilities on a remote system through an encrypted network connection. This method is enacted using the openSSH service.

First you need to check to see if X11 forwarding is permitted. This setting is in the openSSH configuration file, `/etc/ssh/sshd_config`. The directive X11Forwarding should be set to yes in the remote system's configuration file. If the directive is set to no, then you