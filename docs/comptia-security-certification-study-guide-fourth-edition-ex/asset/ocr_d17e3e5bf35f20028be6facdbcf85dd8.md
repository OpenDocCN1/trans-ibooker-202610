+   9. In the Profile wizard step, ensure that the rule applies when you are connected to a corporate domain, on a private network, and a public network by enabling the check box for each of those network profiles. Click Next.
+   10. In the final wizard step, type **RDP Traffic** as the name for the rule and click Finish.

# Linux Firewalls and TCP Wrappers

The Windows operating system is not the only system that has a firewall feature; most operating systems allow you to enable a host-based firewall. If you would like to enable the firewall feature in Linux (I am using Fedora), you can follow these steps:

+   1. Choose the System menu and then choose Administration | Security Level and Firewall.
+   2. On the Firewall Options tab, choose to enable the Linux firewall, as shown in Figure 7-5.
+   3. You can then open ports on the firewall by performing any of the following tasks:

+   - **Trusted services** — You can use the list of trusted services to open a particular port to allow communication to that service. To open a port for a service, simply select the check box for that service. For example, if I wanted clients to connect to the FTP service on my Linux system, I would enable the FTP check box.
+   - **Other ports** — If you want to open a port that is not listed in the trusted services, you can expand Other Ports and click the Add button. In the Add Port dialog box, type the port number and protocol (either TCP or UDP) for the port you wish to open (see Figure 7-6).

![](bbox=[0.362, 0.575, 0.731, 0.942])

**FIGURE 7-5**
Configuring the Linux firewall