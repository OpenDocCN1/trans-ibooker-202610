# FIGURE 7-1

A notification message from the host-based firewall

![](bbox=[0.376, 0.138, 0.798, 0.376])

Most host-based firewalls have a number of common features, which are listed next:

+   - **Block incoming traffic** Packets sent to the system from a host on the network or Internet can be blocked.
+   - **Block outgoing traffic** Packets leaving the system headed out to the network or Internet can be blocked.
+   - **Notification** Most host-based firewalls can be configured to show a message if someone tries to connect to your system or if a program on your system tries to send data out on the network or Internet, as shown in Figure 7-1.
+   - **A default rule** Once enabled, most firewalls have a default rule of denying all traffic unless you allow the traffic in. You can change the default rule to allow all traffic except the packets you specify, but this is seldom practiced.
+   - **Create rules** Most firewalls allow you to add rules on top of the default rule to customize what traffic is permitted or denied to enter or leave the system.

# Configuration Overview

When you configure a host-based firewall, you typically either install the software, if it is provided by a third-party company, or enable the firewall, if you are using the firewall software that comes with the operating system.

When you enable the firewall, it normally has a default rule of denying all traffic unless the traffic is in response to a request from the system with the firewall. For example, you can still surf the Internet when Windows Firewall is enabled because you request the web page by typing the URL of a web site (actually you are sending an HTTP GET request), so the response (in this case, the web page) is allowed to pass through the firewall.