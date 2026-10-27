# Reporting

Most systems, devices, and applications can be configured to send out notifications when specific events occur, but they use different methods to report different levels of severity associated with the event. The following are popular methods of reporting you need to be familiar with for the Security+ exam.

**Alarms**  An alarm is used to report critical events that typically require some form of action from the system or network administrator. For example, an alarm may be used to notify an administrator of suspicious traffic on the network. In this case, the alarm is used to attract the attention of the network administrator so that they can investigate the issue.

**Alerts**  An alert is a less critical type of notification used to notify the system or network administrator that a specific event has occurred, but no action may be required by the administrator. Typically, an alert is used to notify the administrator of a change that has occurred, such as a system coming online or a printer being purged.

**Trends**  A trend is a type of reporting method used to identify security issues such as someone performing a port scan on the network. Trend analysis typically involves looking at log files or packet captures and analyzing the information to identify a trend that may help the administrator understand what is happening on the network. For example, if the network administrator is looking at a packet capture and sees that the same source IP address is connecting to multiple ports within a very short time, then most likely a port scan is occurring.

# CERTIFICATION OBJECTIVE 6.03

# Server Hardening Best Practices

This section covers common practices you should follow to harden servers on the network such as locking down the number of open ports and services running on each server. This section outlines the steps for securing different types of servers common to most networks.

# All Servers

Before looking at popular types of servers and the common steps you should take to secure those servers, I want to stress the steps that should be taken with all servers. The first thing you should do with any server is harden it so that only necessary software and services are installed on it.