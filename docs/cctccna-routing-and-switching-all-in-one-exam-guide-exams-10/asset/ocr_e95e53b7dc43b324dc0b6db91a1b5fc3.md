```
Router>enable
Router#show inventory
NAME: "2811 chassis", DESCR: "2811 chassis, Hw Serial#: FTX1022A1ZJ, Hw
Revision: 52.57"
PID: CISCO2811        , VID: V02 , SN: FTX1022A1ZJ

NAME: "c2811 Motherboard with 2FE and integrated VPN", DESCR: "c2811
Motherboard with 2FE and integrated VPN"
PID: CISCO2811        , VID: V02 , SN: FOC1020257J

NAME: "9 Port FE Switch", DESCR: "9 Port FE Switch"
PID: HWIC-D-9ESW      , VID: VN/A, SN: FOC100131K6

NAME: "WAN Interface Card - DSU/CSU T1 Fractional (V2)", DESCR: "WAN
Interface Card - DSU/CSU T1 Fractional (V2)"
PID:                  , VID: 1.2, SN: FOC10174ES5
```

Notice in the **show inventory** command output that this is a 2800 series router that contains nine Fast Ethernet ports. You can also see that there is a WAN interface card—an internal CSU/DSU device. To see if there is a serial port created by the card, use the **show ip interface brief** command as follows:

```
Router#show ip interface brief
Interface              IP-Address      OK? Method Status    Protocol
(output cut for briefness)
Serial0/2/0            13.0.0.1        YES manual up        up
Vlan1                  unassigned      YES NVRAM   up        down
```

Notice the serial interface of S0/2/0. If you want to view details of that interface, use the **show interface s0/2/0** command:

```
Router#show interface s0/2/0
Serial0/2/0 is up, line protocol is up
  Hardware is GT96K with integrated T1 CSU/DSU
  MTU 1500 bytes, BW 1544 Kbit, DLY 20000 usec,
    reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation HDLC, loopback not set
  Keepalive set (10 sec)
(output cut for briefness)
```

In this output you can see that the layer 1 and layer 2 components are fine (they are up). The serial port exists due to an integrated CSU/DSU, and the