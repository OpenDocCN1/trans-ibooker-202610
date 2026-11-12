4. To enable IPv6 routing on VAN-R1, type the following commands:

```
VAN-R1>enable
VAN-R1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-R1(config)#ipv6 unicast-routing
```

5. To assign an IPv6 address to Gig0/0 and Serial0/3/0, use the following commands:

```
VAN-R1(config)#interface g0/0
VAN-R1(config-if)#ipv6 address 2001:aaaa:bbbb:1::1/64
VAN-R1(config-if)#no shutdown
VAN-R1(config-if)#interface s0/3/0
VAN-R1(config-if)#ipv6 address 2001:aaaa:bbbb:2::1/64
VAN-R1(config-if)#no shutdown
```

Note the IP addresses of each interface.

6. To verify the IPv6 addresses on each of the interfaces, use the show ipv6 interface brief command:

```
VAN-R1(config-if)#exit
VAN-R1(config)#exit
VAN-R1#show ipv6 interface brief
GigabitEthernet0/0                 [up/up]
  FE80::209:7CFF:FE04:AB01
  2001:AAAA:BBBB:1::1
Serial0/3/0                        [up/up]
  FE80::209:7CFF:FE04:AB01
  2001:AAAA:BBBB:2::1
```

Note the global unicast address and the link-local address assigned to each of the interfaces.

7. Switch to the console port of the LA-R1 router and navigate to global configuration mode.

8. Enable IPv6 routing on the LA-R1 router.

9. Configure the IPv6 address of 2001:aaaa:bbbb:2::2/64 on the serial interface of the LA-R1 router.

10. Configure the IPv6 address of 2001:aaaa:bbbb:3::1/64 on the Gigabit interface of the LA-R1 router.

11. The following summarizes all of the commands you should have used: