Dynamic Host Configuration protocol (DHCP) snooping DHCP snooping is a feature you can enable on your switch that allows your switch to detect rogue DHCP servers that have been connected to your network without authorization.

> exam Watch For the Security+ certification exam, remember key security features of a switch, such as VLANs for segmentation, BPDU guard, and DHCP snooping. Remember that with BPDU guard, the switch places the port in an err-disabled state if a BPDU message is sent to the port (likely a hacker performing a man-in-the-middle attack).

# Network Address Translation

Network address translation, or NAT, is a network technology used for years that allows you to use a private address range on the inside of the network that is then translated to a public address used on the NAT gateway (see Figure 8-16). The security benefit is that you hide the internal IP addresses used by systems surfing the Internet because all outbound traffic has the source IP address translated to that of the NAT gateway.

As mentioned, the internal network typically uses what is known as a private address range, which is not usable on the Internet. This means that any outbound traffic has to have the source IP address in the packet converted from the private IP address to a public,

# FIGURE 8-16

NAT is used to hide the internal network and share a public address.

![](bbox=[0.24, 0.61, 0.92, 0.88])