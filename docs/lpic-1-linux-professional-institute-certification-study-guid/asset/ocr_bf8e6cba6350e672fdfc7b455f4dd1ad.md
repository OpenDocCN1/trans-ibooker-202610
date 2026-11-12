Various /proc files are available for different system features, including the interrupt
requests (IRQs), input/output (I/O) ports, and direct memory access (DMA) channels in use
on the system by hardware devices. This section discusses the files used to monitor these
features and how you can access them.

# Interrupt Requests

Interrupt requests (IRQs) allow hardware devices to indicate when they have data to send
to the CPU. The PnP system must assign each hardware device installed on the system a
unique IRQ address. You can view the current IRQs in use on your Linux system by looking-
ing at the /proc/interrupts file using the Linux cat command, as shown in Listing 3.2.

# Listing 3.2: Listing system interrupts from the /proc directory

```
$ cat /proc/interrupts
           CPU0
0:     36   IO-APIC   2-edge      timer
1:    297   IO-APIC   1-edge      i8042
8:     0   IO-APIC   8-edge      rtc0
9:     0   IO-APIC   9-fasteoi   acpi
12:   396   IO-APIC  12-edge      i8042
14:     0   IO-APIC  14-edge     ata_piix
15:   914   IO-APIC  15-edge     ata_piix
18:     2   IO-APIC  18-fasteoi  vboxvideo
19:  4337   IO-APIC  19-fasteoi  enp0s3
20:  1563   IO-APIC  20-fasteoi  vboxguest
21: 29724   IO-APIC  21-fasteoi  ahci[0000:00:0d.0], snd_intel8x0
22:    27   IO-APIC  22-fasteoi  ohci_hcd:usb1
NMI:     0   Non-maskable interrupts
LOC:  93356   Local timer interrupts
SPU:     0   Spurious interrupts
PMI:     0   Performance monitoring interrupts
IWI:     0   IRQ work interrupts
RTR:     0   APIC ICR read retries
RES:     0   Rescheduling interrupts
CAL:     0   Function call interrupts
TLB:     0   TLB shootdowns
TRM:     0   Thermal event interrupts
THR:     0   Threshold APIC interrupts
DFR:     0   Deferred Error APIC interrupts
MCE:     0   Machine check exceptions
MCP:     3   Machine check polls
HYP:     0   Hypervisor callback interrupts
```