The easiest way I have found to add a USB drive is using the ESXi shell. I use an SSH connection to the shell so I can cut and paste commands into it. If you didn’t enable SSH as shown in a previous chapter, you may wish to do so now. If you are using a Windows 10 machine, you can use its built-in SSH client from the command line. Other OSs also have SSH clients.

From the command prompt, enter *ssh root@<IP address of your ESXi server>* and then enter your password (see Figure 7-1).

![](bbox=[0.135, 0.308, 0.905, 0.573])

***Figure 7-1.** SSH to ESXi*

The following are the steps to manually add a USB datastore. In the next section, I will show you how to download and run a script that does these steps for you:

+   1. Stop the USB arbitrator service. This will prevent USB devices from being used as pass-through devices by entering
+   
+      ```sh
+      /etc/init.d/usbarbitrator stop
+      chkconfig usbarbitrator off
+      ```