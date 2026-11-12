The auto attribute tells Linux to assign the link local address, which allows the Linux system to communicate with any other IPv6 device on the local network but not a global address.

Since version 17.04, the Ubuntu distribution has deviated from the standard Debian method and utilizes the Netplan tool to manage network settings. Netplan uses simple YAML text files in the /etc/netplan folder to define the network settings for each network interface installed on the system. By default, Netplan passes the network settings off to the Network Manager tool, so you don’t need to worry about how the Netplan configuration files are set.

For Red Hat–based systems, you’ll need to define the network settings in two separate files. The first file defines the network and netmask addresses in a file named after the network interface name (such as ifcfg–enp0s3). Listing 8.3 shows an example from a CentOS Linux system.

# Listing 8.3: Sample CentOS network interface configuration settings

```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=c8752366-3e1e-47e3-8162-c0435ec6d451
DEVICE=enp0s3
ONBOOT=yes
IPV6_PRIVACY=no
```

The second file required on Red Hat–based systems is the network file, which defines the host name and default gateway, as shown in Listing 8.4.

# Listing 8.4: Sample CentOS network file configuration settings

```
NETWORKING=yes
HOSTNAME=mysystem
GATEWAY=192.168.1.254
```