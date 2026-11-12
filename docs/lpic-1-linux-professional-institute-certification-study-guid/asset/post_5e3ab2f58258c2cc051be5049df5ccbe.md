
them when looking for specific application hosts on the network. Table 8.1 shows some of the more common well-known application ports.

### TABLE 8.1 TCP and UDP well-known ports

| Port | Protocol | Application |
|---|---|---|
| 20 | TCP | File Transfer Protocol (FTP) data |
| 21 | TCP | File Transfer Protocol (FTP) control messages |
| 22 | TCP | Secure Shell (SSH) |
| 23 | TCP | Telnet interactive protocol |
| 25 | TCP | Simple Mail Transfer Protocol (SMTP) |
| 53 | TCP&UDP | Domain Name System (DNS) |
| 80 | TCP | Hypertext Transfer Protocol (HTTP) |
| 110 | TCP | Post Office Protocol version 3 (POP3) |
| 123 | UDP | Network Time Protocol (NTP) |
| 139 | TCP | NetBIOS Session Service |
| 143 | TCP | Internet Message Access Protocol (IMAP) |
| 161 | UDP | Simple Network Management Protocol (SNMP) |
| 162 | UDP | Simple Network Management Protocol trap |
| 389 | TCP | Lightweight Directory Access Protocol (LDAP) |
| 443 | TCP | Hypertext Transfer Protocol (HTTPS) over TLS/SSL |
| 465 | TCP | Authenticated SMTP (SMTPS) |
| 514 | TCP&UDP | Remote Shell (TCP) or Syslog (UDP) |
| 636 | TCP | Lightweight Directory Access Protocol over TLS/SSL (LDAPS) |
| 993 | TCP | Internet Message Access Protocol over TLS/SSL (IMAPS) |
| 995 | TCP | Post Office Protocol 3 over TLS/SSL (POP3S) |

If you think trying to remember all of those port numbers is a hassle, you're not alone.
In an attempt to simplify ports, there's somewhat of an ad hoc standard for assigning
names each of the well-known port numbers. Each Linux system maintains a list of its network port names in the /etc/services file. Network applications can read this file when working with network ports and use the name instead of the port number. This approach of displaying port names instead of numbers is commonly used in network troubleshooting tools.

Now that you've seen the basics of how Linux uses networking to transfer data between systems, our next section dives into the details of how to configure these features in your Linux system.

## Configuring Network Features

There are five main pieces of information you need to configure in your Linux system to interact on a network:

- The host address
- The network subnet address
- The default router (sometimes called the gateway)
- The system host name
- A DNS server address for resolving host names

We have three ways to configure this information in Linux systems:

- Manually editing network configuration files
- Using a graphical tool included with your Linux distribution
- Using command-line tools

The following sections walk through each of these methods.

## Network Configuration Files

Linux systems that utilize the systemd initialization method normally use the systemd-networkd daemon to detect network interfaces and automatically create entries for them in the network configuration files. You can modify those files manually to tweak or change network settings if necessary.

Unfortunately, though, no single standard configuration file exists that all distributions use. Instead, different distributions use different configuration files to define the network settings. Table 8.2 shows the most common network configuration files that you'll run into.

### TABLE 8.2 Linux network configuration files

| Distribution | Network configuration location |
|---|---|
| Debian-based | /etc/network/interfaces file |
| Red Hat-based | /etc/sysconfig/network-scripts directory |
| OpenSUSE | /etc/sysconfig/network file |

Although each of the Linux distributions uses a different method of defining the network settings, they all have similar features. Most configuration files define each of the required network settings as separate values in the configuration file. Listing 8.1 shows an example from a Debian-based Linux system.

### Listing 8.1: Sample Debian network static configuration settings

```
auto eth0
iface eth0 inet static
  address 192.168.1.77
  netmask 255.255.255.0
  gateway 192.168.1.254
iface eth0 inet6 static
  address 2003:aef0::23d1::0a10:00a1
  netmask 64
  gateway 2003:aef0::23d1::0a10:0001
```

The example shown in Listing 8.1 assigns both an IP and an IPv6 address to the wired network interface designated as eth0.

Listing 8.2 shows how to define the IP network settings automatically using a DHCP server on the network.

### Listing 8.2: Sample Debian network DHCP configuration settings

```
auto eth0
iface eth0 inet dhcp
iface eth0 inet6 dhcp
```

If you just want to assign an IPv6 link local address, which uniquely identifies the device on the local network, but not retrieve an IPv6 address from a DHCP server, replace the inet6 line with this:

```
iface eth0 inet6 auto
```

The auto attribute tells Linux to assign the link local address, which allows the Linux system to communicate with any other IPv6 device on the local network but not a global address.

Since version 17.04, the Ubuntu distribution has deviated from the standard Debian method and utilizes the Netplan tool to manage network settings. Netplan uses simple YAML text files in the /etc/netplan folder to define the network settings for each network interface installed on the system. By default, Netplan passes the network settings off to the Network Manager tool, so you don't need to worry about how the Netplan configuration files are set.

For Red Hat–based systems, you'll need to define the network settings in two separate files. The first file defines the network and netmask addresses in a file named after the network interface name (such as ifcfg–enp0s3). Listing 8.3 shows an example from a CentOS Linux system.

### Listing 8.3: Sample CentOS network interface configuration settings

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

### Listing 8.4: Sample CentOS network file configuration settings

```
NETWORKING=yes
HOSTNAME=mysystem
GATEWAY=192.168.1.254
IPV6FORWARDING=yes
IPV6_AUTOCONF=no
IPV6_AUTOTUNNEL=no
IPV6_DEFAULTGW=2003:aef0::23d1::0a10:0001
IPV6_DEFAULTDEV=eth0
```

Notice that the Red Hat network configuration file also defines the host name assigned to the Linux system. For other types of Linux systems, storing the host name in the /etc/hostname file has become somewhat of a de facto standard. However, some Linux distributions use /etc/HOSTNAME instead.

If you're working with a Linux system that uses the systemd initialization method, use the hostnamectl program to set the host name value. The hostnamectl program also allows you to set what's called a "pretty" name for the system, which is used by some utilities on the local system to provide a more detailed description of the device, such as "Rich's laptop."

You will also need to define a DNS server so that the system can use DNS host names. For systemd systems, the DNS server is generated by the systemd-resolved program. For legacy SysVinit systems, that's handled in the /etc/resolv.conf configuration file:

```
domain mydomain.com
search mytest.com
nameserver 192.168.1.1
```

The domain entry defines the domain name assigned to the network. By default, the system will append this domain name to any host names you specify. The search entry defines any additional domains used to search for host names. The nameserver entry is where you specify the DNS server assigned to your network. Some networks can have more than one DNS server; just add multiple nameserver entries in the file.

To help speed up connections to commonly used hosts, you can manually enter their host names and IP addresses into the /etc/hosts file on your Linux system. The /etc/nsswitch.conf file defines whether the Linux system checks this file before or after using DNS to look up the host name.

## Graphical Tools

The Network Manager tool is a popular program used by many Linux distributions to provide a graphical interface for defining network connections. The Network Manager tool starts automatically at boot time and appears in the system tray area of the desktop as an icon.
