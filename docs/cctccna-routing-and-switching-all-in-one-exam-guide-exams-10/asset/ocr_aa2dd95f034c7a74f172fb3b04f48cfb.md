For LAN-based interfaces, the speed of the interface becomes the bandwidth value, where the bandwidth is measured in kilobits per second (Kbps). However, for serial interfaces, the bandwidth defaults to 1554 Kbps, or the speed of a T1 link. This is true no matter what the physical clock rate is on the interface (which you learned about in Chapter 7). To change the bandwidth value for an interface, use the **bandwidth** command:

```
Router(config)# interface serial [slot_#/port_#]
Router(config-if)# bandwidth rate_in_Kbps
```

Here’s an example in which a serial interface clocked at 56,000 bps should have its bandwidth value changed to 56 Kbps:

```
Router(config)# interface serial 0
Router(config-if)# bandwidth 56
```

![](bbox=[0.14, 0.42, 0.24, 0.51])

**EXAM TIP** Remember for the exam that the **bandwidth** command does not change the clock rate on an interface; the **clock rate** command does this. The **bandwidth** command affects only routing protocols that use bandwidth as a metric.

# Viewing the Routing Table

Once you have assigned the IP addresses to each interface, you will need to ensure that routing is enabled on the router by typing the following commands:

```
ROUTERA> enable
ROUTERA# configure terminal
ROUTERA(config)# ip routing
```