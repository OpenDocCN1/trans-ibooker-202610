The DDIMM densities that are supported in the Power E1080 are 32 GB, 64 GB,128 GB and 256 GB and any memory feature code provides four DDIMMs. The DDIMMs have their own voltage regulators and are n+1. redundant. Two PMICs plus two spares are used.

It takes two Power Management IC (PMICs) to supply all the voltage levels that are required by the DDIMM. On the DDIMM, four PMICs are used, which consist of two redundant pairs. If one PMIC in each of the redundant pairs is still functional, the DDIMM is not called for replacement.

As described in 1.5.3, “Memory features” on page 19, the Power E1080 requires the activation of 50% or more of the installed physical memory.

PowerVM hypervisor provides Active Memory Mirroring, which is designed to ensure that system operation continues (even in the unlikely event of an uncorrectable error occurring in main memory).

# 2.3.1 Memory bandwidth

The Power10 single chip module (SCM) supports 16 open memory interface (OMI) links and one differential DIMM (DDIMM) is driven by each OMI link in turn. One OMI link represents a bundle of 8 lanes and is as such capable to transfer 8 byte with one transaction.

The Power E1080 offers four different DDIMM sizes: 32 GB, 64 GB, 128 GB, and 256 GB. The 32 GB and 64 GB DDIMMs run at a data rate of 3200 Mbps; the 128 GB and 256 GB DDIMMs at a data rate of 2933 Mbps.

Table 2-6 lists the available DDIMM capacities and their related maximum theoretical bandwidth figures per OMI link and per Power10 SCM.

Table 2-6 Memory bandwidth of supported DDIMM sizes

| DDIMM capacity | Data rate | Maximum theoretical bandwidth per OMI link | Maximum theoretical bandwidth per SCMᵃ |
|---|---|---|---|
| 32 GB / 64 GB | 3200 Mbps | 25.6 GBps | 409.6 GBps |
| 128 GB / 256 GB | 2933 Mbps | 23.5 GBps | 375.4 GBps |

a. DDIMM modules attached to one SCM must be all of the same size

**Important:** For the best possible performance, it is generally recommended that memory be installed in all memory slots and evenly across all system node drawers and all SCMs in the system.

Balancing memory across the installed system planar cards enables memory access in a consistent manner and typically results in better performance for your configuration.

The Active Memory Expansion (AME) feature is an option that can increase the effective memory capacity of the system. AME uses the on-chip nest accelerator (NX) unit to compress or decompress memory content. This separately priced hardware feature is enabled at partition (LPAR) level. The related LPAR profile parameter allows it to configure the compression factor in accordance with memory capacity needs and performance trade-off consideration.