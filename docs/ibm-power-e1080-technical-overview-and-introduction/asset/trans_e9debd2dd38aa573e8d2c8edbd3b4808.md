

Figure 2-15 shows a comparison between two scenarios: a fully activated system versus a system with CUoD resources being activated along the project timeline.

![](img/7c8373945565ba432037f60bbf8458eb_93_0.png)

Figure 2-15 Active cores scenarios comparison during a project lifecycle

## 2.4.5 Elastic Capacity on-Demand (Temporary)

With the Elastic CoD offering, you can temporarily activate and deactivate processor cores and memory units to help meet the demands of business peaks, such as seasonal activity, period-end, or special promotions. When you order an Elastic CoD feature, you receive an enablement code that allows a system user to make requests for more processor and memory capacity in increments of one processor day or 1 GB memory day. The system monitors the amount and duration of the activations. With Power E1080 server, only the prepaid Elastic Capacity on-Demand option is available with feature code 4586-COD in econfig or directly from ESS.

Processors and memory can be activated and turned off an unlimited number of times when more resources are needed.

This offering provides a system administrator an interface at the HMC to manage the activation and deactivation of resources. Before temporary capacity is used on your server, you must enable your server. To enable your server, an enablement feature (MES only) must be ordered and the required contracts must be in place. The 90-day enablement feature is not available for the Power E1080 processors.

For more information about enablement and usage of Elastic CoD, see Power Systems Capacity on Demand.

## 2.4.6 IBM Power Enterprise Pools 1.0 and Mobile Capacity on-Demand

Although static activations are valid for a single system, some customers might benefit from moving processor and memory activations to different servers because of workload rebalance or disaster recovery.

IBM Power Enterprise Pools 1.0 is a technology for dynamically sharing processor and memory activations among a group (or pool) of IBM Power Systems servers. By using Mobile Capacity on-Demand (CoD) activation codes, the systems administrator can perform tasks without contacting IBM.

With this capability, you can move resources between Power E1080 and Power E980 systems and have unsurpassed flexibility for workload balancing and system maintenance.

Note: Only two consecutive generation of Power servers are supported in the same Power Enterprise Pools 1.0; therefore, Power E1080 can be mixed with Power E980 servers only.

The supported Power Enterprise Pool 1.0 members are listed by pool type in Table 2-9.

Table 2-9 Supported Power Enterprise Pool 1.0 members by pool type

| Power Enterprise Pool type | Pool members |
|---|---|
| Midrange Power Enterprise Pool 1.0 | 770+, E870, E870C, E880C |
| High-end Power Enterprise Pool 1.0 | 780+, 795, E880, E870C, E880C |
| Power Enterprise Pool 1.0 | E870, E880, E870C, E880C, E980 |
| Power Enterprise Pool 1.0 (with Power E1080) | E980, E1080 |

A pool can support systems with different clock speeds or processor generations.

Mobile Capacity on Demand (Mobile CoD) features the following basic rules:

- The Power E1080 server requires a minimum of 16 static processor activations.
- The Power E980 server requires a minimum of eight static processor activations.
- For all systems, 50% of the installed memory must have activations, and a minimum of 25% of those must be static activations.

An HMC can manage multiple IBM Power Enterprise Pools and systems that are not part of an IBM Power Enterprise Pool. Systems can belong to only one IBM Power Enterprise Pool at a time. Powering down an HMC does not limit the assigned resources of participating systems in a pool, but does limit the ability to perform pool change operations.

After an IBM Power Enterprise Pool is created, the HMC can be used to perform the following tasks:

- Mobile CoD processor and memory resources can be assigned to systems with inactive resources. Mobile CoD resources remain on the system to which they are assigned until they are removed from the system.
- New systems can be added to the pool and existing systems can be removed from the pool.
- New Mobile CoD processor and memory resources can be added to the pool.
- Pool information can be viewed, including pool resource assignments, compliance, and history logs.

For the mobile activation features to be configured, an IBM Power Enterprise Pool and the systems that are going to be included as members of the pool must be registered with IBM. Also, the systems must have #EB35 for mobile enablement configured, and the required contracts must be in place.

> **Note:** The following convention is used in the Order type column in the tables in this section:
> - Initial: Only available when ordered as part of a new system
> - MES: Only available as a Miscellaneous Equipment Specification (MES) upgrade
> - Both: Available with a new system or as part of an upgrade
> - Supported: Unavailable as a new purchase, but supported when migrated from another system or as part of a model conversion

Table 2-10 lists the mobile processor and memory activation features that are available for the Power E1080 server.

### Table 2-10 Mobile activation features

| Feature code | Description | Maximum | Orderᵃ type |
|---|---|---|---|
| EDAB | 100 GB Mobile Memory activation for HEX | 480 | Both |
| EMBK | 500 GB Mobile Memory activation for HEX | 130 | Both |
| EDPZ | Mobile processor activation for HEX | 224 | Both |

a. For more information about order types, see 2.4, “Capacity on-demand” on page 76.

For more information about IBM Power Enterprise Pools, see *Power Enterprise Pools on IBM Power Systems*, REDP-5101.

## 2.4.7 Utility Capacity on-Demand

Utility CoD is *not* supported on Power E1080 server.

## 2.4.8 Trial Capacity on-Demand

A *standard request* for Trial CoD requires you to complete a form that includes contact information and vital product data (VPD) from your Power E1080 server with inactive CoD resources.

A standard request activates eight processors or 64 GB of memory for 30 days. Subsequent standard requests can be made after each purchase of a permanent processor activation. An HMC is required to manage Trial CoD activations.

An *exception request* for Trial CoD requires you to complete a form that includes contact information and VPD from your Power E1080 server with inactive CoD resources. An exception request activates all inactive processors or all inactive memory (or all inactive processor and memory) for 30 days. An exception request can be made only once over the life of the machine. An HMC is required to manage Trial CoD activations.

To request either a Standard or an Exception Trial, see Power Systems Capacity on Demand: Trial Capacity on Demand.

## 2.4.9 Software licensing and CoD

For software licensing considerations for the various CoD offerings, see the most recent revision of the Power Systems Capacity on Demand User’s Guide.

## 2.5 Internal I/O subsystem

This section provides more information about the internal PCIe architecture of the Power E1080 system.

## 2.5.1 Internal PCIe Gen 5 subsystem and slot properties

The internal I/O subsystem on the Power E1080 server is connected to the PCIe Gen 5 controllers on a Power10 chip in the system. Each Power10 processor module has two PCIe host bridges (PHBs). The PHBs (PHB0 and PHB1) have 16 PCIe lanes each.

The PHBs provide different type of end point connections. Both the PHBs from Power10 sockets P0 and P1 and one PHB from sockets P2 and P3 connect directly to a PCIe Gen 4 x16 or PCIe Gen 5 x8 slot to provide six PCIe Gen 4 x16 or PCIe Gen 5 x8 slots per system node.

The second PHB on each Power10 socket P2 and P3 is split between a PCIe Gen 5 x8 slot and two Internal Non-Volatile Memory Express (NVMe) SSD PCIe Gen 4 x4 slots. The design overall provides each system node of Power E1080 with:

- Six PCIe Gen 4 x16 or PCIe Gen 5 x8
- Two PCIe Gen 5 x8
- Four Internal Non-Volatile Memory Express (NVMe) SSDs, each using a PCIe Gen 4 x4 connection

Bandwidths for the connections are listed in Table 2-11.

Table 2-11 Internal I/O connection speeds

| Connection | Type | Speed (duplex) |
|---|---|---|
| PCIe adapter slot | PCIe Gen4 x16 | 64 GBps |
| PCIe adapter slot | PCIe Gen5 x8 | 64 GBps |
| NVMe slot | PCIe Gen4 x4 | 16 GBps |

The following theoretical maximum I/O bandwidths in each system node are available:

- Six PCIe Gen 4 x16 / PCIe Gen 5 x8 slots at 64 GBps = 384 GBps
- Two PCIe Gen 5 x8 Slots at 64 GBps = 128 GBps
- Four NVMe slots at 16 GBps = 64 GBps

Total of 576 GBps = 384 GBps + 128 GBps + 64 GBps.

