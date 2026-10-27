![](bbox=[0.072, 0.136, 0.825, 0.265])

# Firewall Topologies

Let’s move on to firewall topologies. Popular configurations for firewalls include dual-homed, screened-host, and screened-subnet firewalls.

**Dual-Homed Host Firewalls** A dual-homed host firewall consists of a single computer with two physical network interfaces that acts as a gateway between the two networks. The server’s routing capability is disabled so that the firewall software installed on the system can handle all traffic management. Firewall software or proxy server software is typically run on this system to pass packets from one side of the dual-homed system to the other. You must be careful not to enable routing within the network operating system that will be used as the dual-homed system; otherwise, you will bypass your firewall software and simply be routing data. Figure 8-7 shows a dual-homed host firewall configuration.

**Screened-Host Firewalls** Screened-host firewall configurations are considered by many to be more secure than dual-homed firewalls. In this configuration, you place a screening router between the dual-homed host and the public network. This enables you to provide packet filtering before the packets reach the dual-homed firewall, thereby adding an extra layer of network security. The dual-homed system can then run firewall software or proxy server software to provide additional security to this configuration. Figure 8-8 shows a screened-host configuration.

![](bbox=[0.072, 0.706, 0.874, 0.911])