
If your system detects a wired network connection, the icon appears as a mini-network with blocks connected together. If your system detects a wireless network connection, the icon appears as an empty radio signal. When you click the icon, you’ll see a list of the available wireless networks detected by the network card (as shown in Figure 8.4).

### FIGURE 8.4 Network Manager showing a wireless network connection

![](img/ca7617340ddbe2360d36ce3428729fa9_488_0.png)

Click your access point to select it from the list. If your access point is encrypted, you’ll be prompted to enter the password to gain access to the network.

When your system is connected to a wireless access point, the icon appears as a radio signal. Click the icon, and then select Edit Connections to edit the network connection settings for the system, shown in Figure 8.5.

### FIGURE 8.5 The Network Manager edit configurations window

![](img/ca7617340ddbe2360d36ce3428729fa9_488_1.png)

You can select the network connection to configure (either wireless or wired), and then click the Edit button to change the current configuration.

The Network Manager tool allows you to specify all four of the network configuration values by using the manual configuration option or to set the configuration to use DHCP to determine the settings. Network Manager automatically updates the appropriate network configuration files with the updated settings.

## Command-Line Tools

If you’re not working with a graphical desktop client environment, you’ll need to use the Linux command-line tools to set the network configuration information. You have quite a few different command-line tools at your disposal. This section covers the ones you’re most likely to run into (and the ones you’ll most likely see on the LPIC-1 exam).

## Network Manager Command-Line Tools

The Network Manager tool also provides two different types of command-line tools:

- nmtui: Provides a simple text-based menu tool
- nmcli: Provides a text-only command-line tool

Both of these tools help guide you through the process of setting the required network information for your Linux system. The nmtui tool displays a stripped-down version of the graphical tool, where you can select a network interface and assign network properties to it, as shown in Figure 8.6.

### FIGURE 8.6 The Network Manager nmtui command-line tool

![](img/ca7617340ddbe2360d36ce3428729fa9_489_0.png)

The nmcli tool doesn't attempt to use any type of graphics capabilities—it just provides a command-line interface where you can view and change the network settings. By default, the command displays the current network devices and their settings, as shown in Listing 8.5.

### Listing 8.5: The default output of the nmcli command

```
$ nmcli
enp0s3: connected to Wired connection 1
"Intel 82540EM Gigabit Ethernet Controller (PRO/1000 MT Desktop Adapter)
ethernet (e1000), 08:00:27:2C:35:D2, hw, mtu 1500
ip4 default, ip6 default
inet4 192.168.1.77/24
route4 0.0.0.0/0
inet6 2600:1702:1ce0:eeb0::6d0/128
inet6 fe80::16d2:b8f:7f78:f3ed/64
route6 2600:1702:1ce0:eeb0::/60
route6 2600:1702:1ce0:eeb0::/64
route6 ::/0
route6 ff00::/8
route6 fe80::/64
route6 fe80::/64
...
```

The nmcli command uses command-line options to allow you to set the network settings:

```
# nmcli con add type ethernet con-name eth1 ifname enp0s3 ip4
10.0.2.10/24 gw4 192.168.1.254
```

## Legacy Tools

If your Linux distribution doesn't support one of the Network Manager tools, there are usually legacy tools, including utilities from the net-tools package, available in most Linux distributions. Here are a few of the basic command-line tools that you can use:

- ethtool: Displays Ethernet settings for a network interface
- ifconfig: Displays or sets the IP address and netmask values for a network interface
- iwconfig: Sets the SSID and encryption key for a wireless interface
- route: Sets the default router address

The ethtool command allows you to peek inside the network interface card Ethernet settings and change any properties that you may need to communicate with a network device, such as a switch.

By default, the ethtool command displays the current configuration settings for the network interface, as shown in Listing 8.6.

### Listing 8.6: Output from the ethtool command

```
$ ethtool enp0s3
Settings for enp0s3:
    Supported ports: [ TP ]
    Supported link modes:   10baseT/Half 10baseT/Full
                            100baseT/Half 100baseT/Full
                            1000baseT/Full
    Supported pause frame use: No
    Supports auto-negotiation: Yes
    Supported FEC modes: Not reported
    Advertised link modes: 10baseT/Half 10baseT/Full
                             100baseT/Half 100baseT/Full
                             1000baseT/Full
    Advertised pause frame use: No
    Advertised auto-negotiation: Yes
    Advertised FEC modes: Not reported
    Speed: 1000Mb/s
    Duplex: Full
    Port: Twisted Pair
    PHYAD: 0
    Transceiver: internal
    Auto-negotiation: on
    MDI-X: off (auto)
Cannot get wake-on-lan settings: Operation not permitted
    Current message level: 0x00000007 (7)
                           drv probe link
    Link detected: yes
$
```

You can change features such as speed, duplex, and whether or not the network interface attempts to auto-negotiate features with the switch.

The ifconfig command is a legacy command for configuring network device settings. It allows you to set the network address and subnet mask for a network interface:

```
$ sudo ifconfig enp0s3 down 10.0.2.10 netmask 255.255.255.0
```

You can list all of the network interface settings on your system using the ifconfig command with no command-line options, as shown in Listing 8.7.

### Listing 8.7: Displaying network interface information

```
$ ifconfig
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500
    inet 192.168.1.77 netmask 255.255.255.0 broadcast 192.168.1.255
    inet6 2600:1702:1ce0:eeb0:66dc:cedc:10ff:9ee6 prefixlen 64 scopeid 0x0<global>
    inet6 fe80::16d2:b8f:7f78:f3ed prefixlen 64 scopeid 0x20<link>
    inet6 2600:1702:1ce0:eeb0:48e3:1865:5544:8200 prefixlen 64 scopeid 0x0<global>
    ether 08:00:27:2c:35:d2 txqueuelen 1000 (Ethernet)
    RX packets 293593 bytes 431675620 (431.6 MB)
    RX errors 0 dropped 0 overruns 0 frame 0
    TX packets 119754 bytes 9135701 (9.1 MB)
    TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING> mtu 65536
    inet 127.0.0.1 netmask 255.0.0.0
    inet6 ::1 prefixlen 128 scopeid 0x10<host>
    loop txqueuelen 1000 (Local Loopback)
    RX packets 1206 bytes 125586 (125.5 KB)
    RX errors 0 dropped 0 overruns 0 frame 0
    TX packets 1206 bytes 125586 (125.5 KB)
    TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0

$
```

Instead of using the long ifconfig command format to activate or deactivate a network interface, you can use the simpler ifup or ifdown command. Just add the interface name with the command to easily activate or deactivate the interface.

Before you can use the ifconfig command to assign an address to a wireless interface, you must assign the wireless SSID and encryption key values using the iwconfig command:

```
# iwconfig wlan0 essid "MyNetwork" key s:mypassword
```

The essid parameter specifies the access point SSID name, and the key parameter specifies the encryption key required to connect to it. Notice that the encryption key is preceded by an s:. That allows you to specify the encryption key in ASCII text characters; otherwise, you'll need to specify the key using hexadecimal values.

If you don't know the name of a local wireless connection, you can use the iwlist command to display all of the wireless signals your wireless card detects. Just specify the name of the wireless device, and use the scan option:

```
$ iwlist wlan0 scan
```

To set the default gateway, use the route command:

```
# route add default gw 192.168.1.254
```

You can also use the route command by itself to view the current default router configured for the system:

```
$ route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         192.168.1.254   0.0.0.0         UG    0      0        0 enp0s3
192.168.1.0     *               255.255.255.0   U     1      0        0 enp0s3
$
```

In this example, the default router defined for the Linux system is 192.168.1.254 and is available from the enp0s3 network interface. The output also shows that to get to the 192.168.1.0 network you don’t need a gateway, because that’s the local network the Linux system is connected to.

If your network is connected to multiple networks via multiple routers, you can manually create the routing table in the system by using the add or del command-line option for the route command. The format for that is

```
route [add] [del] target gw gateway
```

where _target_ is the target host or network and _gateway_ is the router address.

## The iproute2 Package

Most of the legacy command-line network tools have been replaced with the newer _iproute2_ package. The main utility in the iproute2 package is the ip command.

The ip command is more robust than the old ifconfig command, and it is becoming the more popular method for defining network settings from the command line. The ip utility uses several command options to display the current network settings or define new network settings. Table 8.3 shows these command options.

| Parameter | Description |
|---|---|
| address | Display or set the IPv4 or IPv6 address on the device |
| addrlabel | Define configuration labels |
| l2tp | Tunnel Ethernet over IP |
| link | Define a network device |
| maddress | Define a multicast address for the system to listen to |
