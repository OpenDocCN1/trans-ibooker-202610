Once you set up the global properties for OSPFv3, you must place interfaces into the local process. Unlike OSPFv2, where you used the **network** command within the OSPFv2 routing process configuration, IPv6’s configuration is like RIPng, where you perform the configuration under an interface:

```
IOS(config)# interface type [slot_#]/port_#
IOS(config-if)# ipv6 ospf process_ID area area_number
```

# Simple OSPFv3 Configuration Example

Here’s an example configuration placing two interfaces in area 0:

```
IOS(config)# ipv6 unicast-routing
IOS(config)# ipv6 router ospf 1
IOS(config-router)# router-id 0.0.0.1
IOS(config-router)# exit
IOS(config)# interface g1/0/1
IOS(config-if)# ipv6 ospf 1 area 0
IOS(config-if)# exit
IOS(config)# interface g1/0/2
IOS(config-if)# ipv6 ospf 1 area 0
IOS(config-if)# exit
```

As you can see, the configuration is fairly simple. Remember that your Cisco device does not need a global IPv6 address on an interface in order to participate in OSPFv3—it needs only a link-local address, which it will automatically acquire, assuming IPv6 is globally enabled and the interface is enabled.

# OSPFv3 Verification

Once you’ve configured OSPFv3, you can verify the operation of OSPFv3 by using **show** commands similar to those used in OSPFv2. To display general information about OSPFv3 routing processes, use the **show ipv6 ospf** command. Here’s an example: