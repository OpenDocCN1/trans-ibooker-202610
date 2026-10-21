
![](img/7c8373945565ba432037f60bbf8458eb_14_0.png)

## Introduction to Power E1080

The Power E1080 is the newest addition to IBM Power family, the industry’s best-in-class server platform for security and reliability. The Power E1080 introduces the essential enterprise hybrid cloud platform, which is uniquely designed to help you securely and efficiently scale core operational and artificial intelligence (AI) applications anywhere in a hybrid cloud.

The Power E1080 simplifies end-to-end encryption and brings AI where your data is stored for faster insights. This configuration helps enable greater workload deployment flexibility and agility while accomplishing more work.

The Power E1080 can help you to realize the following benefits:

+   Protect trust from core to cloud
+   Protect data that is in-transit and at-rest with greatly simplified end-to-end encryption across hybrid cloud without affecting performance.

+   Enjoy enterprise quality of service
+   The Power E1080 can detect, isolate, and recover from soft errors automatically in the hardware without taking an outage or relying on an operating system to manage the faults.

+   Drive greater efficiency with sustainable and scalable compute
+   The processor performance, massive system throughput, and memory capacity qualify the Power E1080 server to be the perfect workload consolidation platform. This performance entails significant savings in floor space, energy consumption, and operational expenditure costs.

This chapter includes the following topics:

+   1.1, “System overview” on page 2

+   1.2, “System nodes” on page 6

+   1.3, “System control unit” on page 10

+   1.4, “Server specifications” on page 11

+   1.5, “System features” on page 15

+   1.6, “I/O drawers” on page 26

+   1.7, “System racks” on page 34

+   1.8, “Hardware management console overview” on page 42

## 1.1 System overview

The Power E1080, also referred to by its 9080-HEX machine type-model designation, represents the most powerful and scalable server in the IBM Power portfolio. It is comprised of a combination of CEC enclosures that are called *nodes* (or *system nodes*) and more units and drawers.

### 1.1.1 System nodes, processors, and memory

In this section, we provide a general overview of the system nodes, processors, and memory. For more information about the system nodes, see 1.2, “System nodes” on page 6.

A system node is an enclosure that provides the connections and supporting electronics to connect the processor with the memory, internal disk, adapters, and the interconnects that are required for expansion.

A combination of one, two, three, or four system nodes per server is supported.

Each system node provides four sockets for Power10 processor chips and 64 differential DIMM (DDIMM) slots for Double Data Rate 4 (DDR4) technology DIMMs.

Each socket holds one Power10 single chip module (SCM). An SCM can contain 10, 12, or 15 Power10 processor cores. It also holds the extra infrastructure logic to provide electric power and data connectivity to Power10 processor chip.

The processor configuration of a system node is defined by the selected processor feature. Each feature defines a set of four Power10 processors chips with the same core density (10, 12, or 15).

A 4-node Power E1080 server scales up to 16 processor sockets and 160, 192, or 240 cores, depending on the number of cores provided by the configured SCM type.

All system nodes with a Power E1080 server must be configured with the same processor feature.

Each system node can support up to a maximum of 16 TB of system memory by using the largest available memory DIMM density. A fully configured 4-node Power E1080 can support up to 64 TB of memory.

To support internal boot capability, each system node enables the use of up to four non-volatile memory express (NVMe) drive bays. More drive bays are provided through expansion drawers.

Each system node provides eight Peripheral Component Interconnect Express (PCIe) Gen 5 capable slots, with a maximum of 32 per Power E1080 server.

Any one-, two-, three-, or four-system node configuration requires the system control unit (SCU) to operate. The SCU provides system hardware, firmware, and virtualization control through redundant Flexible Service Processors (FSPs). Only one SCU is required and supported for every Power E1080 server. For more information about the system control unit, see 1.3, “System control unit” on page 10.

For more information about the environmental and physical aspects of the server, see 1.4, “Server specifications” on page 11.

### 1.1.2 Expansion drawers and storage enclosures

Capacity can be added to your system by using expansion drawers and storage enclosures.

An optional 19-inch PCIe Gen 3 4U I/O expansion drawer provides 12 PCIe Gen 3 slots. The I/O expansion drawer connects to the system node with a pair of PCIe x16 to CXP converter cards that are housed in the system node. Each system node can support up to four I/O expansion drawers, for a total of 48 PCIe Gen 3 slots. A fully configured Power E1080 can support a maximum of 16 I/O expansion drawers, which provides a total of 192 PCIe Gen 3 slots.

An optional EXP24SX SAS storage enclosure provides 24 2.5-inch small form factor (SFF) serial-attached SCSI (SAS) bays. It supports up to 24 hot-swap hard disk drives (HDDs) or solid-state drives (SSDs) in only 2U rack units of space in a 19-inch rack. The EXP24SX is connected to the Power E1080 server by using SAS adapters that are plugged into system node PCIe slots or I/O expansion drawer slots.

For more information about enclosures and drawers, see 1.6, “I/O drawers” on page 26.

For more information about IBM storage products, see this web page.

### 1.1.3 Hardware at-a-glance

The Power E1080 server provides the following hardware components and characteristics:

+   - 10-, 12-, or 15-core Power10 processor chips that are packaged in a single chip module per socket
+   - One, two, three, or four system nodes with four Power10 processor sockets each
+   - Redundant clocking in each system node
+   - Up to 60 Power10 processor cores per system node and up to 240 per system
+   - Up to 16 TB of DDR4 memory per system node and up to 64 TB per system
+   - 8 PCIe Gen 5 slots per system node and a maximum of 32 PCIe Gen 5 slots per system
+   - PCIe Gen 1, Gen 2, Gen 3, Gen 4, and Gen 5 adapter cards supported in system nodes
+   - Up to 4 PCIe Gen 3 4U I/O expansion drawers per system node providing a maximum of 48 additional PCIe Gen 3 slots
+   - Up to 192 PCIe Gen 3 slots using 16 PCIe Gen 3 I/O expansion drawers per system
+   - Up to over 4,000 directly attached SAS HDDs or SSDs through EXP24SX SFF drawers
+   - System control unit, which provides redundant Flexible Service Processors and support for the operations panel, the system VPD, and external attached DVD

The massive computational power, exceptional system capacity, and the unprecedented scalability of the Power E1080 server hardware are unfolded by unique enterprise class firmware and system software capabilities and features. The following important characteristics and features are offered by the IBM Power enterprise platform:

+   - Support for IBM AIX, IBM i, and Linux operating system environments
+   - Innovative dense math engine that is integrated in each Power10 processor-core to accelerate AI inferencing workloads
+   - Optimized encryption units that are implemented in each Power10 processor-core
+   - Dedicated data compression engines that are provided by the Power10 processor technology

+   - Hardware and firmware assisted and enforced security provide trusted boot and pervasive memory encryption support
+   - Up to 1,000 virtual machines (VMs) or logical partitions (LPARs) per system
+   - Dynamic LPAR support to modify available processor and memory resources according to workload, without interruption of the business
+   - Capacity on demand (CoD) processor and memory options to help respond more rapidly and seamlessly to changing business requirements and growth
+   - IBM Power System Private Cloud Solution with Dynamic Capacity featuring Power Enterprise Pools 2.0 that support unsurpassed enterprise flexibility for real-time workload balancing, system maintenance and operational expenditure cost management.

Table 1-1 compares important technical characteristics of the Power E1080 server with those of the Power System E980 server, based on IBM POWER9™ processor-based technology.

## Table 1-1 Comparison between the Power E980 and the Power E1080 server

| Features | Power E980 server | Power E1080 server |
|---|---|---|
| Processor | POWER9 | Power10 |
| Processor package | Single Chip Module | Single Chip Module |
| Cores per single chip module | 6, 8, 10, 11, 12 | 10, 12, 15 |
| Number of cores per system | Up to 192 cores | Up to 240 cores |
| Sockets per node | 4 | 4 |
| System configuration options | 1-, 2-, 3-, and 4-node systems | 1-, 2-, 3-, and 4-node systems |
| Maximum memory per node | 16 TB | 16 TB |
| Maximum memory per system | 64 TB | 64 TB |
| Maximum memory bandwidth per node | 920 GBps | 1636 GBps |
| Aggregated maximum memory bandwidth per system | 3680 GBps | 6544 GBps |
| Pervasive memory encryption | No | Yes |
| PCIe slots per node | Eight PCIe Gen 4 slots | Eight PCIe Gen 5 slots |
| I/O drawer expansion option | Yes | Yes |
| Acceleration ports | Yes<br>CAPI 2.0 & OpenCAPI 3.0ᵃ | Yes<br>OpenCAPI 3.0, 3.1 |
| PCIe hot-plug Support | Yes | Yes |
| I/O bandwidth per node | 545 GBps | 576 GBps |
| Integrated USB | USB 3.0 | Not available |
| Internal storage bays per node | Four NVMe PCIe Gen 3 baysᵇ | Four NVMe PCIe Gen 4 bays |
| Per lane bit rate between sockets | 25 Gbps | 32 Gbps |
