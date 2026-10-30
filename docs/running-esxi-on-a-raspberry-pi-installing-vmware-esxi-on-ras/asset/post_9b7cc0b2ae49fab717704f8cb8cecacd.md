
![](img/430161835ae267467687add4a1cdc429_51_0.png)

**Figure 2-17.** *QNAP 1635AX NAS*

Given that we already have Arm systems that can provide storage and VMs on the same appliance, and that Dell PowerStore runs ESXi on its x64-powered storage arrays, it is not a far reach to assume that in the near future we will see VMs being managed via ESXi on Arm.

## Limitations of Using a Pi for ESXi

While being able to run ESXi on a low-priced device such as a Pi is a boon for enterprises and enthusiasts alike, it does come with some limitations in terms of software and hardware compatibility.

ESX’s biggest limitation is the fact that it is a Fling; as such, VMware has not made any commitments with regard to eventual production or even its continued availability – or to continue to support the Pi if it does go into production. Furthermore, because VMware does not officially support it, anyone troubleshooting will need to work through issues on their own or consult one of the online support communities.

## Executables

A key point to remember is that ESXi on Arm will only run VMs that have OSs that are compiled for Arm processors, which represents a very small subset of all the software able to run on x86 systems. You will not be able to run Windows desktop or server OSs on it. Android devices run on Arm processors; however, they will not run as a guest on ESXi on Arm.

One potential avenue to address this is through emulation software. While per-core licensing makes this troublesome, there are successful examples of this. Qualcomm showed their emulation technology for Windows devices. Perhaps the best-known example at this point is the Apple M1 Rosetta software that allows programs compiled for x86 to run on Apple silicon Macs. There are many challenges running this in the VMware ecosystem, but something like this could happen.

There are many great and unique projects that use the Pi, but if you plan on running them as VMs on the device, it is unlikely to run successfully or smoothly as the ESXi hypervisor abstracts the hardware away from it. That said, popular versions of Linux for Arm64 run without any issues, including Ubuntu, CentOS, Fedora, and Debian, as well as lesser-known ones such as Photon OS. Raspberry Pi OS will run on it as well as FreeBSD. The ESXi on Arm community is strong, and more and more OSs are becoming available to run on it every week.

You will not be able to vMotion a running VM from an ESXi server running on an x64 server to an ESXi on Arm host, nor will you be able to start an x64 VM on an Arm server. VMware offers many pre-built virtual applications (vApps), but as these are all (so far) designed for x64 servers, they will not run on Arm servers.

## Pi Hardware

There are a myriad of third-party hardware attachments for the Pi that people use for all sorts of interesting purposes. These range from small LED displays to GPS receivers. However, it is extremely unlikely that any of these tools will work on a Pi with ESXi installed on it since the required drivers and connectors aren’t exposed to ESXi or passed through to the VMs that run on it.

## Overcommitment of Physical Resources

ESXi supports the *overcommitment* of physical resources (e.g., CPU, RAM, and storage), meaning that the running VMs are configured to “have” more physical resources dedicated to them than exist on the actual physical machine.

For example, if five VMs with 2GB of RAM, two vCPUs, and 100GB of storage were running on a Pi with 8GB of RAM, four CPU cores, and 256GB of storage, the VMs in this scenario would be overcommitted (Figure 2-18). In many cases, this will work fine as the physical resources will not be accessed at the same time. In the rare occurrence that all of the VMs do, in fact, try to access their maximum allocated resources simultaneously, then the hypervisors have various means to elegantly deal with them.

![](img/430161835ae267467687add4a1cdc429_54_0.png)

**Figure 2-18. VM Overcommitment**

## Storage

The storage for ESXi that is used for and by the VMs on the Pi is limited to being connected via the USB ports or accessed via the network. Both the USB port and onboard NIC are limited by the bus that they run on. I have tested a fair number of storage devices on a Pi, and the best performance I have seen is a transfer rate of 100MBps.

I have also seen issues when copying files between two storage devices connected to the Pi’s USB ports. My grossly simplified block diagram of the Pi 4B+ (Figure 2-19) shows that all the USB ports go through the same bus, and I believe this is where the bottleneck lies.

**Figure 2-19. RPi 4B+ Block Diagram**

![](img/430161835ae267467687add4a1cdc429_55_0.png)

When adding a new virtual disk for a VM (Figure 2-20), you have three different options: Thick Provision Lazy Zeroed, Thick Provision Eager Zeroed, and Thin Provision.

![](img/430161835ae267467687add4a1cdc429_55_1.png)

**Figure 2-20. Allocating Disk Space**

Thick Provision Lazy Zeroed will reserve space on the underlying storage and only zero out any data that may have previously resided on that space immediately before writing to it. In contrast, Thick Provision Eager Zeroed will reserve the space on the underlying storage and zero out any data that may have previously resided on that space before the first write to it is made.

Thin Provision can be thought of as overcommitting storage. The VM does not reserve space; you only set the maximum space that it can consume. Space is allocated and zeroed out on demand as the space is required. Thin provisioning allows unused storage to be used by VMs that require it.

Let’s assume that you have a 750GB physical disk and you have three VMs with 300GB thin-provisioned virtual disks on them for a total of 900GB of thin-provisioned storage; however, each of these VMs is only actually using 100GB of space on the disk for a total of 300GB of actual-used disk space. You also have another VM that has a 200GB thick-provisioned disk that is only using 50GB of the disk, but (as it is thick-provisioned) the entire 200GB is allocated and unavailable for other usage. In this example, what we have is a total of 1,100GB of provisioned space on a 750GB physical disk (Figure 2-21).

![](img/430161835ae267467687add4a1cdc429_56_0.png)

**Figure 2-21.** *Provisioned Disk Space*

If all the memory on a system has been reserved by VMs, other VMs will not be able to be started (Figure 2-23).

![](img/430161835ae267467687add4a1cdc429_58_0.png)

Figure 2-23. All Memory Reserved

## CPU

Given that the Pi has a limited number of cores and that there are limited CPU cycles to work with, this in turn limits the number of VMs that can be efficiently run on it. ESXi itself will only consume a few cycles in most cases.

If you overcommit the CPUs on your VMs, the hypervisor will time-slice the CPUs in an attempt to ensure that the VMs get some CPU cycles. For critical VMs, you can reserve and give the VMs exclusive access to CPU cycles using the vSphere Client (Figure 2-24).

![](img/430161835ae267467687add4a1cdc429_59_0.png)

**Figure 2-24.** Reserving CPU Cycles

In the case that all the CPU cycles on a system have been reserved by VMs, other VMs will not be able to be started. An example of this is shown in Figure 2-25 where only three VMs are able to start.

![](img/430161835ae267467687add4a1cdc429_59_1.png)

**Figure 2-25.** All CPU Cycles Reserved

In a VDI environment, it is common to have up to five virtual CPUs for every physical CPU.

## ESXi on Arm on Pi Resource Usage Test

To check the resource usage of ESXi running on my Pi, I used ControlUp (Figure 2-26) and then the vSphere Client (Figure 2-27) without any running VMs. I found that it was using 18% of the RAM (~1.35GB) and virtually no CPU cycles, leaving 6.6GB RAM for the VMs running on it.

![](img/430161835ae267467687add4a1cdc429_60_0.png)

**Figure 2-26.** *ESXi Resource Usage Test – ControlUp*

![](img/430161835ae267467687add4a1cdc429_60_1.png)

**Figure 2-27.** *Resource Usage Test – vSphere Client*
