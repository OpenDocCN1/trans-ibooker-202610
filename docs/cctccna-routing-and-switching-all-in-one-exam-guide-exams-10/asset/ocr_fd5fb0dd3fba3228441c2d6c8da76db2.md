The following shows the more common commands used to configure a DHCP server on a Cisco IOS device:

```
IOS(config)# service dhcp
IOS(config)# ip dhcp pool pool_name
IOS(dhcp-config)# network network_number [mask | /prefix-length]
IOS(dhcp-config)# domain-name domain
IOS(dhcp-config)# dns-server address [address2... address8]
IOS(dhcp-config)# default-router address
IOS(dhcp-config)# lease {days [hours] [minutes] | infinite}
IOS(dhcp-config)# exit
IOS(config)# ip dhcp excluded-address low-address [high-address]
```

In the example,

+   - **service dhcp** Enables the DHCP server and/or relay features on your IOS device. Without this command, the IOS device will not act as a server or relay agent. As of IOS 12.2, this command is enabled by default.
+   - **ip dhcp pool** Creates a name for the DHCP server address pool and places you in DHCP pool subcommand mode.
+   - **network** Specifies the subnet network number and mask of the DHCP address pool. Also, *prefix-length* specifies the number of bits that make up the address prefix. The prefix is an alternative way of specifying the network mask of the client. The prefix length must be preceded by a forward slash (/).
+   - **domain-name** Specifies the domain name to be assigned to the client.
+   - **dns-server** Specifies the IP address of a DNS server that is available to a DHCP client. One IP address is required; however, you can specify up to eight IP addresses in one command line.
+   - **default-router** Specifies the IP address of the default gateway to be assigned to the DHCP client.
+   - **lease** Specifies the duration of the lease. IP addresses are leased to hosts, typically on a temporary basis. When a client’s address lease time reaches half the life of the lease, it will attempt to renew the existing addressing information it obtained previously. If unsuccessful in its renewal attempt, the device will continue to