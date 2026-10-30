
To get a better feel for the range of Arm systems that ESXi supports, let’s compare the hardware of the Raspberry Pi 4 Model B with its four cores and 8GB of RAM with the Lenovo ThinkSystem HR350A, a 2U rack server designed for datacenter usage that, at the time of this writing, is being demoed to select customers. It has a 32-core 64-bit ARMv8 CPU running at 3.3GHz and has support for 512GB of RAM. For storage, it supports two NVMe drives in the chassis and six external storage bays.

## Supercomputers

In June of 2020, when the annual TOP500 supercomputer rankings were released by TOP500.org, Japan’s Fugaku Arm-based supercomputer was at the top of the list. To achieve the number one spot, it performed 416 petaflops on the High-Performance Linpack (HPL) benchmark, beating the previous holder of the title by an outstanding 280%.

Fugaku runs an unmodified version of Red Hat Enterprise Arm Linux. It has 158,976 Fujitsu A64FX processors running at 2.2GHz. Each processor contains 48 Arm cores for a total of slightly more than 7.5 million cores. The A64FX also includes specialized vector units and high-bandwidth memory (HBM) to achieve this level of performance.

Japan is not the only country using A64FX processors in supercomputers. US-based Sandia National Laboratories announced that they have begun the installation of a Fujitsu PRIMEHPC FX700 system. The EUC is also seeing the benefits of the Arm architecture, and its members are working on a supercomputer based on an EUC-developed Arm processor.

## ESXi on Arm and the Future of the Datacenter

The future of Arm in the datacenter looks promising as new products and topologies that use existing products are beginning to emerge. Many of the products will need to be monitored and managed, and VMware will use its incredible presence in the datacenter to do just that – and will back much of this on VMware’s ability to have ESXi run on Arm.

## SmartNIC/DPU

At VMworld 2020, VMware announced Project Monterey. This was an underreported announcement, but one with a major impact. As VMware Chief Executive Pat Gelsinger said during VMworld, Monterey is a “major rearchitecture of vSphere itself.” The importance of this rearchitecture cannot be understated as vSphere is the umbrella under which ESXi, vCenter Server (its management plane), and basically all of VMware’s virtualized datacenter platforms (both on-premise and cloud-based) all reside.

Kit Colbert wrote a blog at the end of September of 2020 that clarified some details pertaining to Project Monterey. Specifically, he specified that Monterey uses SmartNICs to offload a traditional server’s network and network storage workload from a server’s CPU (Figure 2-6) to a SmartNIC’s CPU (Figure 2-7).

![](img/430161835ae267467687add4a1cdc429_42_0.png)

## Figure 2-6. NIC in Traditional Server

The obvious workloads for SmartNICs are ones that relate to the operation and security of general network information. By using SmartNICs, these services can be performed without impacting the server’s CPU as only the network information that is relevant to that server is processed by the server’s CPU (Figure 2-7).

![](img/430161835ae267467687add4a1cdc429_42_1.png)

## Figure 2-7. SmartNIC in Server

limited flexibility, and it’s difficult to add additional functionality to them if needed. SoCs are extremely flexible, but they lack the speed of ASICs and FPGAs.

To provide both flexibility and performance, some SmartNICs use SoCs for control-plane functions due to their inherent flexibility and FPGAs or ASICs for data-plane functions for increased performance. The primary advantage of using an FPGA over an ASIC is that an FPGA can be reprogrammed when a new functionality is developed and/or needed, whereas ASICs are hardwired and can be more difficult to update (Figure 2-12).

![](img/430161835ae267467687add4a1cdc429_45_0.png)

## Figure 2-12. Arm/FPGA SmartNIC

In a server with a SmartNIC, there are two instances of ESXi running on it: one for the server’s main CPU(s) and VMs and another for the SmartNIC and the VMs that handle the offload work that the SmartNIC is taking on (Figure 2-13).

## Computational Storage

A new term is starting to emerge to describe the ability of a VM/device/server/hypervisor to process data directly on the same device as the storage – *computational storage*. Arm’s Director of Storage Solutions, Neil Werdmuller, recently wrote:

> > *Computational storage is emerging as a critical piece of the data storage puzzle because it puts processing power directly on the storage device, giving companies secure, quick and easy access to vital information.*

I have seen a few computational storage appliances in use, including Dell PowerStore products, and other Arm-based computational storage appliances.

## Dell PowerStore Storage Appliances

Computational storage, based on vSphere, has already appeared in x86 systems. It is interesting to see how it was implemented as it may give insight into how it will be implemented on Arm systems.

In 2020, Dell Technologies released a new midrange storage platform – PowerStore. This product line is based on the latest technology as it supports NVMe drives, and its operating system (PowerOS) uses containers to deliver its features. Physically, it is a 2U rack appliance, with four x64 Intel processors. Another unique feature is that one of its varieties, the X Series, runs two separate instances of ESXi (Figure 2–14).

![](img/430161835ae267467687add4a1cdc429_48_0.png)

**Figure 2-14.** Dell PowerStore Storage Appliance

In a traditional datacenter, both physical and virtual machines connect to a storage array to process data. The systems are connected over fiber or Ethernet, usually through a switch. Although these networks are very fast, even a nanosecond of delay becomes unacceptable for some workloads (Figure 2-15).

![](img/430161835ae267467687add4a1cdc429_48_1.png)

**Figure 2-15.** Delay When Connecting Servers to Storage

By having the hypervisor on the same physical system as the storage, the network and its latency are both removed (Figure 2-16).

![](img/430161835ae267467687add4a1cdc429_49_0.png)

Figure 2-16. Reduced Delay When VMs are on Storage

At the time of this writing, the PowerStore X Series currently uses x64 processors, but there are also companies using Arm processors on their Arm-based storage arrays.

## Arm-Based Computational Storage

Computational storage appliances can combine storage and VMs on the same physical server. This assumes that Arm processors can effectively deliver storage - and they can. In fact, two companies that provide storage arrays that can be powered by Arm processors are SoftIron and QNAP.

## SoftIron

SoftIron’s enterprise-grade storage arrays were designed from the metal up to the use Arm processors. SoftIron leverages Arm processors’ lower cost, power efficiency, and SoC architecture to streamline the transfer of data

from physical storage devices. SoftIron’s flagship product Ceph is an open source SDS (software-defined storage) that delivers block, file, and object storage. It is a mature product that was first released in 2012 and since then has been extensively developed by all the major commercial Linux distributions (e.g., Canonical, Red Hat, and SUSE) as well as Intel, Cisco, and SanDisk.

SoftIron has compared the performance of its Arm storage solution with x64 storage. Indeed, it has impressive performance improvements over x64 storage especially where object sizes are 64 – 4096K. To their credit, SoftIron does concede that x64-based systems currently do perform better with 40Gbe and faster networks. Their arrays not only provide storage, but they also perform data transcoding directly on the device (they have not specified if they use VMs, containers, or another mechanism to do this, but computationally intensive activities are performed on the array). One needs to remember that SoftIron uses an older A1100 Arm processor on their devices and that this may account for the performance of the device.

## QNAP

QNAP is a well-regarded manufacturer of NAS appliances, ranging from a single-disk appliance for home use to a 24-bay active-active dual-controller unit designed for enterprise use. QNAP uses either x64 or Arm processors depending on the appliance. Some of their Arm-based appliances, such as the TS-1635AX NAS (Figure 2-17), supply storage as well as containers and VMs. This device uses Linux, rather than ESXi, to manage VMs and containers. Due to the market that it is addressing, and cost that ESXi would likely impose (VMware has yet to reveal any pricing for ESXi on Arm if it becomes a product), it is unlikely that QNAP would run ESXi on this class of storage device. I included this device as it is an operation array that has a hypervisor on it.
