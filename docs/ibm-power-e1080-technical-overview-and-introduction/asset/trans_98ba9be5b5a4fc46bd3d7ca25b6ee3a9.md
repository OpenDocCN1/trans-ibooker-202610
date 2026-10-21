

Figure 2-25 shows the rear view of the PCIe Gen3 I/O Expansion Drawer.

![](img/7c8373945565ba432037f60bbf8458eb_113_0.png)

Figure 2-25 Rear view of the PCIe Gen3 I/O Expansion Drawer

## 2.7.3 PCIe Gen3 I/O Expansion Drawer optical cabling

I/O drawers are connected to the adapters in the system node by any of the following data transfer cables:

- 2.0 m CXP 16x Active Optical Cable (AOC) Pair (#ECCR)
- 10.0 m CXP 16x Active Optical Cable (AOC) Pair (#ECCY)
- 20.0 m CXP 16x Active Optical Cable (AOC) Pair (#ECCZ)

> **Cable lengths:** Use the 2.0 m cables for intra-rack installations. Use the 10.0 m or 20.0 m cables for inter-rack installations.

Older cables #ECC6, #ECC8, and #ECC9 that are used in Power System E980 are not supported or orderable on Power System E1080. They are supported only as model conversions and announced as RPO.

A minimum of one PCIe Gen 4 Optical Cable Adapter for PCIe Gen 3 Expansion Drawer (#EJ24) is required to connect to the PCIe Gen 3 6-slot Fanout Module in the I/O expansion drawer. The top port of the Fanout Module must be cabled to the top port of the #EJ24 port. Also, the bottom two ports must be cabled together.

To perform the cabling correctly, complete the following steps:

- 1. Connect an AOC to connector T0 on the PCIe Gen 4 cable adapter (#EJ24) in your server.
- 2. Connect the other end of the optical cable to connector T1 on one of the PCIe Gen 3 6-slot Fanout Modules in your I/O expansion drawer.
- 3. Connect another AOC cable to connector T1 on the PCIe Gen 4 cable adapter (#EJ24).
- 4. Connect the other end of the cable to connector T2 on the same PCIe Gen 3 6-slot Fanout Module in I/O expansion drawer.
- 5. Repeat the steps 1 to 4 for the other PCIe Gen 3 6-slot Fanout Module in the I/O expansion drawer, if required.

**Drawer connections:** Each Fanout Module in a PCIe Gen 3 Expansion Drawer (#EMX0) can be connected to only a single PCIe Gen 4 Cable Adapter (#EJ24). However, the two Fanout Modules in a single I/O expansion drawer can be connected to different system nodes in the same server.

Figure 2-26 shows the connector locations of the PCIe Gen3 I/O Expansion Drawer.

![](img/7c8373945565ba432037f60bbf8458eb_114_0.png)

Figure 2-26 Connector locations for the PCIe Gen3 I/O Expansion Drawer

Table 2-25 lists the PCIe slots in the PCIe Gen3 I/O Expansion Drawer.

Table 2-25 PCIe slot locations and descriptions for the PCIe Gen3 I/O Expansion Drawer

| Slot | Location code | Description |
|---|---|---|
| Slot 1 | P1-C1 | PCIe Gen 3, x16 |
| Slot 2 | P1-C2 | PCIe Gen 3, x8 |
| Slot 3 | P1-C3 | PCIe Gen 3, x8 |
| Slot 4 | P1-C4 | PCIe Gen 3, x16 |
| Slot 5 | P1-C5 | PCIe Gen 3, x8 |
| Slot 6 | P1-C6 | PCIe Gen 3, x8 |
| Slot 7 | P2-C1 | PCIe Gen 3, x16 |
| Slot 8 | P2-C2 | PCIe Gen 3, x8 |
| Slot 9 | P2-C3 | PCIe Gen 3, x8 |
| Slot 10 | P2-C4 | PCIe Gen 3, x16 |
| Slot 11 | P2-C5 | PCIe Gen 3, x8 |
| Slot 12 | P2-C6 | PCIe Gen 3, x8 |

Consider the following points:

- All slots support full-length, regular-height adapters or short (low-profile) adapters with a regular-height tailstock in single-wide, Gen3 BSCs.
- Slots C1 and C4 in each PCIe Gen 3 6-slot Fanout Module are x16 PCIe Gen 3 buses, and slots C2, C3, C5, and C6 are x8 PCIe buses.
- All slots support enhanced error handling (EEH).
- All PCIe slots are hot-swappable and support concurrent maintenance.

Typical Slot priorities are listed in Table 2-26.

Table 2-26 PCIe adapter slot priorities for the PCIe Gen3 I/O Expansion Drawer

| Feature code | Description | Slot priorities |
|---|---|---|
| EMX0 | Typical Slot priorities | 1, 7, 4, 10, 2, 8, 3, 9, 5, 11, 6, 12 |

For more information about slot priorities for individual adapters, see IBM Documentation.

If the EMX0 PCIe Gen 3 Expansion Drawer is configured with two PCIe Gen 3 6-slot Fanout Modules, distribute the PCIe adapters across both I/O modules whenever possible

Slots 1, 7, 4, and 10 support SR-IOV capabilities. However, if the total amount of physical memory is less than 128 GB, Slot 4 (P1-C4) and Slot 10 (P2-C4) are not SR-IOV capable.

Figure 2-27 shows the connection schematics between system node and PCIe Gen3 I/O Expansion Drawer.

![](img/7c8373945565ba432037f60bbf8458eb_116_0.png)

Figure 2-27 Connection schematics between system node and Expansion Drawer

## General rules for the PCIe Gen3 I/O Expansion Drawer configuration

The PCIe Gen 4 Cable Adapter (#EJ24) can be in any of the PCIe adapter slots in a Power E1080 server node. However, we advise that you first populate the PCIe adapter slots as per the slot priorities that are discussed in this section. If an I/O expansion drawer is present, the PCIe Gen 4 cable adapter (#EJ24) must be given the highest priority.

Each processor module drives two PCIe Gen5 slots, and all slots are equal regarding their bandwidth characteristics. If you first use the slots following the slot priorities, you ensure that one PCIe Gen5 slot per processor module is populated before you use the second PCIe Gen5 slot of the processor modules.

Table 2-27 lists the PCIe adapter slot priorities in the Power E1080 server. If the sequence is chosen as listed in the slot priority column, the adapters are assigned to the SCM in alignment with the internal enumeration order: SCM0, SCM1, SCM2, and SCM3.

Table 2-27 PCIe adapter slot priorities

| Feature code | Description | Slot priorities |
|---|---|---|
| EJ24 | PCIe Gen 4 Optical Cable Adapter for PCIe Gen 3 Expansion Drawer | 1, 7, 3, 6, 2, 8, 4, 5 |

The following figures show several examples of supported configurations. (For simplification, we do not show every possible combination of the I/O expansion drawer to server attachments.)

Figure 2-28 shows an example of a Power 1080 that features a single system node and four PCIe Gen3 I/O Expansion Drawers. Each lane between the #EJ24 adapter and I/O drawer fanout module represent an CXP 16x AOC pair.

![](img/7c8373945565ba432037f60bbf8458eb_117_0.png)

Figure 2-28 Single system node to four PCIe Gen3 I/O Expansion Drawer connections

Figure 2-29 shows an example of a Power E1080 having two system nodes and four PCIe Gen3 I/O Expansion Drawers. Each lane between the #EJ24 adapter and I/O drawer fanout module represent an CXP 16x AOC pair.

![](img/7c8373945565ba432037f60bbf8458eb_118_0.png)

Figure 2-29 Dual system node to four PCIe Gen3 I/O Expansion Drawer connections

## 2.7.4 PCIe Gen3 I/O Expansion Drawer SPCN cabling

No system power control network (SPCN) is used to control and monitor the status of power and cooling within the I/O drawer. SPCN capabilities are integrated into the optical cables.

## 2.8 External disk subsystems

This section describes the following external disk subsystems that can be attached to the Power E1080 server:

- EXP24SX SAS Storage Enclosures
- IBM System Storage

## 2.8.1 IBM EXP24SX SAS Storage Enclosure

The EXP24SX SAS storage enclosure (#ESLS) is the only DASD drawer that is available for Power E1080 that provides extra disk drives. The ESLS supports SSDs and HDDs. The ESLS storage enclosures are connected to system units by using a serial-attached SCSI (SAS) port in the SAS adapters.

The EXP24SX is a storage expansion enclosure with 24 2.5-inch small form factor (SFF) SAS bays. It supports 10K and 15K SAS HDD having 512 and 4K format drives and enterprise and mainstream class SAS SSDs.

The enclosures can be split logically into one, two, or four independent groups:

- One set of 24 bays (mode 1) (D1-D24),
- Two sets of 12 bays (mode 2), (D1-D12) (D13-D24)or
- Four sets of 6 bays (mode 4).(D1-D6) (D7-D12) (D13-D18) (D19-D24)

The front view of the ESLS storage enclosure with drive locations is shown in Figure 2-30.

![](img/7c8373945565ba432037f60bbf8458eb_119_0.png)

Figure 2-30 Front view of the ESLS storage enclosure with mode groups & drive locations

The SAS storage enclosures support the following operating systems:

- AIX
- IBM i
- Linux
- VIOS

