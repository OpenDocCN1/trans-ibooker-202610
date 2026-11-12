is booted into RXBOOT, the prompt is **router (boot)>**. The configuration register in this case would be set to 2101.

+   - **2 to F** Having the boot field set to a value from 2 to F indicates the IOS image that should be loaded from flash memory. To boot an IOS image from flash memory, the configuration register would have a value of 2102 through to 210F.

# Looking at the Configuration Register Value

Once the router is booted up, you can view the current configuration register value with the **show version** command:

```
Router> show version
Cisco IOS Software, 1841 Software (C1841-ADVIPSERVICESK9-M),
    Version 12.4(6)T7, RELEASE SOFTWARE (fc5)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2007 by Cisco Systems, Inc.
Compiled Thu 29-Mar-07 03:28 by khuie

ROM: System Bootstrap, Version 12.4(13r)T, RELEASE SOFTWARE (fc1)

Router1 uptime is 3 days, 22 hours, 5 minutes
System returned to ROM by reload at 19:06:33 UTC Fri Dec 7 2007
System image file is "flash:c1841-advipservicesk9-mz.124-6.T7.bin"
.
.
.
125K bytes of non-volatile configuration memory.
32768K bytes of processor board System flash (Read/Write)
Configuration register is 0x2102
```

At the very bottom of the **show version** output, the current value of the configuration register is shown—in this case it is *0x2102*.

![](bbox=[0.14, 0.77, 0.24, 0.82])

**EXAM TIP** For the Cisco exams, remember that you can see the system image the router used upon bootup with the **show version**