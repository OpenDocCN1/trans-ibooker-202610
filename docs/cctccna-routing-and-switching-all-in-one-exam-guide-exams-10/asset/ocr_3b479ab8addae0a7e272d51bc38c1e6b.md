![](bbox=[0.17, 0.07, 0.8, 0.35])

```
VAN-SW1(config)#interface vlan1
VAN-SW1(config-if)#ip address 192.168.35.98 255.255.255.224
VAN-SW1(config-if)#no shutdown
VAN-SW1(config-if)#exit
VAN-SW1(config)#ip default-gateway 192.168.35.65
```

```
VAN-SW1(config)#interface vlan1
VAN-SW1(config-if)#ip address 192.168.35.92 255.255.255.224
VAN-SW1(config-if)#no shutdown
VAN-SW1(config-if)#exit
VAN-SW1(config)#ip default-gateway 192.168.35.65
```

```
VAN-SW1(config)#interface g0/0
VAN-SW1(config-if)#ip address 192.168.35.92 255.255.255.224
VAN-SW1(config-if)#no shutdown
VAN-SW1(config-if)#exit
VAN-SW1(config)#ip default-gateway 192.168.35.65
```

```
VAN-SW1(config)#interface vlan1
VAN-SW1(config-if)#ip address 192.168.35.98 255.255.255.224
VAN-SW1(config)#ip default-gateway 192.168.35.65
```

# LA-SW1 Configuration Commands

2. Looking at the command on the left side of the following exhibit, draw a line to connect it to the description of what that command does.