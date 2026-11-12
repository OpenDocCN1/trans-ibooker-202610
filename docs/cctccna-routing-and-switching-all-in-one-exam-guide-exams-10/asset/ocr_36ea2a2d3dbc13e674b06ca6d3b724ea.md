violation. If the port is green, it means there is a connection to a device, and if it is flashing green, it means there is network activity on that port.

+   - When in DUPLX display mode, if a port LED is showing a solid green light, then the port is running in full duplex. If the port LED is off, it means the port is in half-duplex mode.

# Basic Switch Configuration

+   - An IP address can be assigned to a VLAN interface on a switch to access it remotely via telnet or SSH.
+   - The **ip default-gateway** command assigns a router address the switch should use to access other subnets.
+   - The **show mac address-table** command displays the MAC address table of the switch and any addresses that have been learned.

# Interface Configuration

+   - Port security is used to prevent unauthorized access to a LAN on access interfaces (non-trunk connections).
+   - The **switchport port-security** commands are used to configure port security.
+   - The defaults for port security are learning one MAC address on the interface with a violation mode of shutdown.
+   - Sticky learning enables a switch to dynamically learn which MAC addresses are connected to an interface and then configure those as the port security addresses that are authorized to access the port.

# Working with VLANs

+   - A VLAN is a group of devices, in the same broadcast domain, that have the same network or subnet number. VLANs are not restricted to physical locations, so users can be physically located anywhere in the switched network.
+   - Static, or port-based, VLAN membership is manually assigned by the administrator. Dynamic VLAN membership is determined by information from the user device, such as its MAC address or 802.1x authentication credentials.