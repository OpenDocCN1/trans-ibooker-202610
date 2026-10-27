![](bbox=[0.112, 0.135, 0.924, 0.249])

A screened subnet uses two screened routers and a firewall.

# Screened-Subnet Firewalls

A screened-subnet firewall configuration takes security to the next level by further isolating the internal network from the public network. An additional screening router is placed between the internal network and the dual-homed firewall. This provides two additional levels of security. First, by adding a screening router internally, you can protect the dual-homed firewall host from an attack by an internal source. Second, it makes an external attack much more difficult because the number of layers an attacker must go through is increased. Normally, the outside screening router will be configured to send any data that has passed the filter rule to the dual-homed firewall, which will perform more tests on the incoming traffic. Once the incoming traffic has passed the test performed by the dual-homed system, the traffic may then be sent to the internal screening router, where additional tests on the packet are performed. The internal screening router is typically configured to accept only data from the dual-homed firewall, ensuring that hackers can’t skip past the outside firewall layers. Figure 8-9 shows the screened-subnet firewall configuration.

> **For the Security+ certification exam, know the different types of firewall topologies such as dual-homed, screened host, and screened subnet.**

# Security Zones

Firewalls allow the network administrator to divide the network into different network segments known as zones. As shown in Figure 8-10, when creating your firewall plan, you will typically create three zones:

+   - **Private LAN/intranet** — The firewall placed in front of the private LAN ensures that no traffic from any other network is sent through the firewall to the private LAN. Note that this zone could be called the private zone, private LAN, or intranet zone.
+   - **DMZ** — The DMZ is an area between two firewalls (typically referred to as external and internal firewalls) that allows selected traffic from the Internet to pass through the external firewall into systems within the DMZ. The purpose of the internal firewall is to not allow any traffic originating from the Internet to pass through it. The DMZ is where you place any servers that need to be reached by the general public, such as a web server, SMTP server, FTP server, or DNS server.