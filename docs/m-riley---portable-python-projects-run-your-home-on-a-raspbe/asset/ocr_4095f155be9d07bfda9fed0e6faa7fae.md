# Setup

Here’s what you need to build this project.

# Hardware

+   - Irdroid transceiver[^69]
+   - Consumer electronic appliance or device with an IR receiver and associated remote control

# Software

+   - Linux Infrared Remote Control (LIRC)[^70]
+   - pushbullet[^71] Python library

# Services

+   - IFTTT[^72]
+   - Pushbullet[^73]

This project builds upon the work we did in Chapter 8, *Voice Wake on LAN*. As such, if you haven’t already created accounts on IFTTT and Pushbullet, please revisit the instructions how to do so in the last project. In this project, we’ll attach an Irdroid infrared transceiver to the Pi’s USB port and then add a few lines of Python code to the previously completed voicewol.py script. Doing so will smarten up the ability to interact with a chosen IR-controlled device, using just your voice.

Before attaching the Irdroid IR transceiver to the Pi, install the Linux Infrared Remote Control (LIRC) service and helper apps for the Pi:

```
$ sudo apt install lirc
```

Next, plug the Irdroid transceiver into one of the powered Pi’s available USB ports, as shown in the next photo. You should see Irdroid’s internal blue LED