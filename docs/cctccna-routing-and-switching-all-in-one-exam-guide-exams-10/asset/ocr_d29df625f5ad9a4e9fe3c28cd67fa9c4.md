Now that we have configured the IP address and encapsulation protocol on the serial interface, all we need to do is make sure that the interface is receiving the clocking speed, or timing, of the link. Setting the clocking speed is the responsibility of the data communications equipment (DCE). When connecting the router to the ISP, the ISP is the DCE end of the link, and as such the ISP sets the clocking speed of the link. In the ISP example, your router is the data terminal equipment (DTE), so you are not allowed to set the clock speed of the link.

Why am I mentioning this if it is set by the ISP? Well, if you are connecting two routers together over their serial ports with a back-to-back serial cable, one end of the cable is typically labeled “DCE” and the other end of the cable is labeled “DTE.” On the router that has the DCE end of the cable, you will need to set the clock rate with the following command:

```
IOS(config-if)#clock rate 64000
```

If you try to execute the **clock rate** command when you are configuring the DTE device, you will get a message that the command is performed on DCE devices only. No harm in trying, though.

The last command you configure on the serial interface is to enable the interface by using the **no shutdown** command (whether you are the DCE or DTE device):

```
IOS(config-if)#no shutdown
```

![](bbox=[0.14, 0.65, 0.22, 0.71])

**VIDEO 7.05.** The digital resources that accompany this book contain a multimedia demonstration of configuring a serial interface on the router.

If you are not sure which router has the DTE end of the cable and which has the DCE end, you can determine this with the **show controllers** command:

```
Router> show controllers serial [slot_#]/port_#
```