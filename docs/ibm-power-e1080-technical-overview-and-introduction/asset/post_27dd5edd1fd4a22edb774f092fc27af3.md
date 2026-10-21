
| Feature code | Description | Order type |
|---|---|---|
| EDAX | 512 GB Base Memory Activation Linux only - Conversion | Both |
| ELME | 512 GB Power Linux Memory Activations for HEX | Both |
| EMAZ | 1 GB Memory activation for HEX | Both |
| EMBK | 500 GB DDR4 Mobile Memory Activation for HEX/80H | Both |
| EMBZ | 512 GB Memory Activations for HEX | Both |
| EMQZ | 100 GB of #EMAZ Memory activation for HEX | Both |

## 1.5.4 System node PCIe features

Each system node provides eight PCIe Gen 5 hot-plug enabled slots; therefore, a two-system nodes server provides 16 slots, a three-system nodes server provides 24 slots, and a four-system nodes system provides 32 slots.

Table 1-12 lists all the supported PCIe adapter feature codes inside the Power E1080 server node drawer.

Table 1-12 PCIe adapters supported on Power E1080 server node

| Feature code | CCIN | Description | OS support |
|---|---|---|---|
| EN1A | 578F | PCIe Gen 3 32 Gb 2-port Fibre Channel Adapter | AIX, IBM i, and Linux |
| EN1B | 578F | PCIe Gen 3 LP 32 Gb 2-port Fibre Channel Adapter | AIX, IBM i, and Linux |
| EN1C | 578E | PCIe Gen 3 16 Gb 4-port Fibre Channel Adapter | AIX, IBM i, and Linux |
| EN1D | 578E | PCIe Gen 3 LP 16 Gb 4-port Fibre Channel Adapter | AIX, IBM i, and Linux |
| EN1F | 579A | PCIe Gen 3 LP 16 Gb 4-port Fibre Channel Adapter | AIX, IBM i, and Linux |
| EN1H | 579B | PCIe Gen 3 LP 2-Port 16 Gb Fibre Channel Adapter | AIX, IBM i, and Linux |
| EN1K | 579C | PCIe Gen 4 LP 32 Gb 2-port Optical Fibre Channel Adapter | AIX, IBM i, and Linux |
| EN2A | 579D | PCIe Gen 3 16 Gb 2-port Fibre Channel Adapter | AIX, IBM i, and Linux |
| EN2B | 579D | PCIe Gen 3 LP 16 Gb 2-port Fibre Channel Adapter | AIX, IBM i, and Linux |
| 5260 | 576F | PCIe2 LP 4-port 1 GbE Adapter | AIX, IBM i, and Linux |
| 5899 | 576F | PCIe2 4-port 1 GbE Adapter | AIX, IBM i, and Linux |
| EC2T | 58FB | PCIe Gen 3 LP 2-Port 25/10 Gb NIC&ROCE SR/Cu Adapterᵃ | AIX, IBM i, and Linux |
| EC2U | 58FB | PCIe Gen 3 2-Port 25/10 Gb NIC&ROCE SR/Cu Adapterᵃ | AIX, IBM i, and Linux |
| EC67 | 2CF3 | PCIe Gen 4 LP 2-port 100 Gb ROCE EN LP adapter | AIX, IBM i, and Linux |
| EN0S | 2CC3 | PCIe2 4-Port (10 Gb+1 GbE) SR+RJ45 Adapter | AIX, IBM i, and Linux |
| EN0T | 2CC3 | PCIe2 LP 4-Port (10 Gb+1 GbE) SR+RJ45 Adapter | AIX, IBM i, and Linux |
| EN0X | 2CC4 | PCIe2 LP 2-port 10/1 GbE BaseT RJ45 Adapter | AIX, IBM i, and Linux |

a. Requires SFP to provide 10 Gb, 2 Gb, or 1 Gb BaseT connectivity

## 1.5.5 System node disk and media features

At the time of this writing, the Power E1080 server node supports up to four 7 mm NVMe U.2 drives that are plugged into the 4-bay NVMe carrier backplane (feature code EJBC). Each system node requires one backplane, even if no NVMe U.2 drives are selected.

Each NVMe U.2 drive can be independently assigned to different LPARs for hosting the operating system and to start from them. They also can be used for non-data intensive workloads. NVMe U.2 drives are concurrently replaceable.

Table 1-13 lists the available NVMe drive feature codes for the Power E1080 and the operating system support.

Table 1-13 NVMe features

| Feature code | CCIN | Description | OS support |
|---|---|---|---|
| EC5J | 59B4 | Mainstream 800 GB SSD NVMe U.2 module | AIX and Linux |
| EC5K | 59B5 | Mainstream 1.6 TB SSD NVMe U.2 module | AIX and Linux |
| EC5L | 59B6 | Mainstream 3.2 TB SSD NVMe U.2 module | AIX and Linux |
| EC7Q | 59B4 | 800 GB Mainstream NVMe U.2 SSD 4k for AIX/Linux | AIX and Linux |

For more information, see 2.5.2, “Internal NVMe storage subsystem” on page 87.

For systems that are running IBM i, an expansion or storage drawer can meet the NVMe requirements.

## 1.5.6 System node USB features

The Power E1080 supports one stand-alone external USB drive that is associated to feature code EUA5. The feature code includes the cable that is used to the USB drive to the preferred front accessible USB port on the SCU.

The Power E1080 server node does not offer an integrated USB port. The USB 3.0 adapter feature code EC6J is required to provide connectivity to an optional external USB DVD drive and requires one system node or I/O expansion drawer PCIe slot. The adapter connects to the USB port in the rear of the SCU with the cable associated to feature code EC6N. Because this cable is 1.5 m long, in case of a Power E1080 with more than one system node, the USB 3.0 adapter can be used in the first or the second system node only.

The USB 3.0 adapter feature code EC6J supports the assignment to an LPAR and can be migrated from an operating LPAR to another, including the connected DVD drive. This design allows it to assign the DVD drive feature to any LPAR according to the need.

Dynamic allocation of system resources such as processor, memory, and I/O is also referred to as *dynamic LPAR* or *DLPAR*.

For more information about the USB subsystem, see 1.5.6, “System node USB features” on page 23.

## 1.5.7 Power supply features

Each Power E1080 server node has four 1950 W bulk power supply units that are operating at 240 V. These power supply unit features are a default configuration on every Power E1080 system node. The four units per system node do not have an associated feature code and are always auto-selected by the IBM configurator when a new configuration task is started.

Four power cords from the power distribution units (PDU) drive these power supplies, which connect to four C13/C14 type receptacles on the linecord conduit in the rear of the system. The power linecord conduit source power from the rear and connects to the power supply units in the front of the system.

The system design provides N+2 redundancy for system bulk power, which allows the system to continue operation with any two of the power supply units functioning. The failed units must remain in the system until new power supply units are available for replacement.

The power supply units are hot-swappable, which allows replacement of a failed unit without system interruption. The power supply units are placed in front of the system, which makes any necessary service that much easier.

Figure 1-7 shows the power supply units and their physical locations marked as E1, E2, E3, and E4 in the system.

![](img/7c8373945565ba432037f60bbf8458eb_37_0.png)

## 1.5.8 System node PCIe interconnect features

Each system node provides 8 PCIe Gen5 hot-plug enabled slots; therefore, a 2-node system provides 16 slots, a 3-node system provides 24 slots, and a 4-node system provides 32 slots.

Up to four I/O expansion drawer features #EMX0 can be connected per node to achieve the slot capacity that is listed in Table 1-14.

Table 1-14 PCIe slots availability for different system nodes configurations

| System node # | I/O expansion # | LP slots | Full height slots |
|---|---|---|---|
| 1 | 4 | 8 | 48 |
| 2 | 8 | 16 | 96 |
| 3 | 12 | 24 | 144 |
| 4 | 16 | 32 | 192 |

Each I/O expansion drawer consists of two Fanout Module feature #EMXH, each providing six PCIe slots. Each Fanout Module connects to the system by using a pair of CXP cable features. The CXP cable features are listed in Table 1-16 on page 27.

Table 1-15 Optical CXP cable feature

| Feature code | CCIN | Description | OS support | Order type |
|---|---|---|---|---|
| ECCR |  | 2 M Active Optical Cable Pair for PCIe Gen 3 Expansion Drawer |  | Both |
| ECCY |  | 10 M Active Optical Cable Pair for PCIe Gen 3 Expansion Drawer |  | Both |
| ECCZ |  | 20 M Active Optical Cable Pair for PCIe Gen 3 Expansion Drawer |  | Both |

The RPO-only cables in this list are not available for ordering new or MES upgrade, but for migrating from a source system. Select a longer length feature code for inter-rack connection between the system node and the expansion drawer.

The one pair of CXP optical cable connects to system node by using one 2-ports PCIe optical cable adapter feature EJ24, which is placed in the CEC.

Both CXP optical cable pair and the optical cable adapter features are concurrently maintainable. Therefore, careful balancing of I/O, assigning adapters through redundant EMX0 expansion drawers, and different system nodes can ensure high-availability for I/O resources that are assigned to partitions.

**Note:** At the time of this writing, feature EMXH is the only supported or orderable fanout module. Previous fanout module features EMXF and EMXG are not supported on Power E1080 system.

For more information about internal buses and the architecture of internal and external I/O subsystems, see 2.5, “Internal I/O subsystem” on page 83.
