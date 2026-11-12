# # Working with Devices

Linux provides a wealth of command-line tools to use the devices connected to your system, as well as to monitor and troubleshoot the devices if you experience problems. This section walks through some of the more popular tools you'll want to know about when working with Linux devices.

# ## Finding Devices

One of the first tasks for a new Linux administrator is to find the different devices installed on the Linux system. Fortunately, there are a few command-line tools to help out with that.

The `lssdev` command-line command displays information about the hardware devices installed on the Linux system. It retrieves information from the `/proc/interrupts`, `/proc/ioports`, and `/proc/dma` virtual files and combines them together in one output, as shown in Listing 3.5.

# **Listing 3.5:** Output from the lssdev command

```
$ sudo lssdev
Device            DMA   IRQ   I/O Ports
...
acpi                9
ACPI                4000-4003 4004-4005 4008-400b 4020-4021
ahci                d240-d247 d248-d24b d250-d257 d258-d25b
ata_piix    14 15   0170-0177 01f0-01f7 0376-0376 03f6-03f6
cascade     4
dma                 0080-008f
dma1                0000-001f
dma2                00c0-00df
e1000               d010-d017
enp0s3      19
fpu                 00f0-00ff
i8042       1 12
Intel               d100-d1ff   d200-d23f
keyboard            0060-0060   0064-0064
ohci_hcd:usb1   22
PCI                 0000-0cf7 0cf8-0cff 0d00-ffff
pic1                0020-0021
pic2                00a0-00a1
piix4_smbus         4100-4108
rtc0        8       0070-0071
rtc_cmos            0070-0071
snd_intel8x0        21
timer       0
timer0              0040-0043
```