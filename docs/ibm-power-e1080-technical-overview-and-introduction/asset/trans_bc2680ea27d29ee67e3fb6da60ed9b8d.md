

Regular and PEP 2.0 associated activations for Power E1080 are listed in Table 1-9. The Order type table column includes the following designations:

**Initial**
Denotes the orderability of a feature for *only* the new purchase of the system.

**MES**
Denotes the orderability of a feature for *only* the MES upgrade purchases on the system.

**Both**
Denotes the orderability of a feature for new *and* MES upgrade purchases.

**Supported**
Denotes that feature is not orderable, but is supported. That is, the feature can be migrated only from existing systems.

## Table 1-9 Processor activation features

| Feature code | Description | Order type |
|---|---|---|
| EPS0 | 1 core Base Proc Act (Pools 2.0) for #EDP2 any OS (from Static) | MES |
| EPS1 | 1 core Base Proc Act (Pools 2.0) for #EDP3 any OS (from Static) | MES |
| EPS2 | 1 core Base Proc Act (Pools 2.0) for #EDP4 any OS (from Static) | MES |
| EPS5 | 1 core Base Proc Act (Pools 2.0) for #EDP2 Linux (from Static) | MES |
| EPS6 | 1 core Base Proc Act (Pools 2.0) for #EDP3 Linux (from Static) | MES |
| EPS7 | 1 core Base Proc Act (Pools 2.0) for #EDP4 Linux (from Static) | MES |
| EPSK | 1 core Base Proc Act (Pools 2.0) for #EDP2 any OS (from Prev) | MES |
| EPSL | 1 core Base Proc Act (Pools 2.0) for #EDP3 any OS (from Prev) | MES |
| EPSM | 1 core Base Proc Act (Pools 2.0) for #EDP4 any OS (from Prev) | MES |
| EDP2 | 40-core (4x10) Typical 3.65 to 3.90 GHz (max) Power10 Processor with 5U system node drawer | Both |
| EDP3 | 48-core (4x12) Typical 3.60 to 4.15 GHz (max) Power10 Processor with 5U system node drawer | Both |
| EDP4 | 60-core (4x15) Typical 3.55 to 4.00 GHz (max) Power10 Processor with 5U system node drawer | Both |
| ED2Z | Single 5250 Enterprise Enablement | Both |
| ED30 | Full 5250 Enterprise Enablement | Both |
| EDPB | 1 core Processor Activation for #EDBB/#EDP2 | Both |
| EDPC | 1 core Processor Activation for #EDBC/#EDP3 | Both |
| EDPD | 1 core Processor Activation for #EDBD/#EDP4 | Both |
| EDPZ | Mobile processor activation for HEX/80H | Both |
| EPDC | 1 core Base Processor Activation (Pools 2.0) for EDP2 any OS | Both |
| EPDD | 1 core Base Processor Activation (Pools 2.0) for EDP3 any OS | Both |
| EPDS | 1 core Base Processor Activation (Pools 2.0) for EDP4 any OS | Both |
| EPDU | 1 core Base Processor Activation (Pools 2.0) for EDP2 Linux only | Both |
| EPDW | 1 core Base Processor Activation (Pools 2.0) for EDP3 Linux only | Both |
| EPDX | 1 core Base Processor Activation (Pools 2.0) for EDP4 Linux only | Both |
| ELCL | Power Linux processor activation for #EDP2 | Both |
| ELCM | Power Linux processor activation for #EDP4 | Both |
| ELCQ | Power Linux processor activation for #EDP3 | Both |

## 1.5.3 Memory features

This section describes the memory features that are available on a Power E1080 server. Careful selection of these features helps the user to configure their system with the correct amount of memory that can meet the demands of memory intensive workloads. On a Power E1080 server, the memory features can be classified into the following feature categories:

- Physical memory
- Memory activation

These features are described next.

### Physical memory features

Physical memory features that are supported on Power E1080 are the next generation differential dual inline memory modules, called DDIMM (see 2.3, “Memory subsystem” on page 72). DDIMMs that are used in the E1080 are Enterprise Class 4U DDIMMs.

The memory DDIMM features are available in 32-, 64-, 128-, and 256-GB capacity. Among these DDIMM features, 32 GB and 64 GB DDIMMs run at 3200 MHz frequency and 128 GB and 256 GB DDIMMs run at 2933 MHz frequency.

Each system node provides 64 DDIMM slots that support a maximum of 16 TB memory and a four system node E1080 can support a maximum of 64 TB memory. DDIMMs are ordered by using memory feature codes, which include a bundle of four DDIMMs with the same capacity.

Consider the following points regarding improved performance:

- Plugging DDIMMs of same density provides the highest performance.
- Filling all the memory slots provides maximum memory performance.
- System performance improves when more quads of memory DDIMMs match.
- System performance also improves as the amount of memory is spread across more DDIMM slots.

For example, if 1TB of memory is required, 64 x 32 GB DDIMMs can provide better performance than 32 x 64 GB DDIMMs

Figure 1-6 shows a DDIMM memory feature.

![](img/7c8373945565ba432037f60bbf8458eb_33_0.png)

Figure 1-6 New DDIMM feature

Table 1-10 lists the available memory DDIMM feature codes for the Power E1080.

## Table 1-10 E1080 memory feature codes

| Feature code | CCIN | Description | OS support |
|---|---|---|---|
| EMC1 | 32AB | 128 GB (4x32 GB) DDIMMs, 3200 MHz, 16 GBIT DDR4 Memory | AIX, IBM i, and Linux |
| EMC2 | 32AC | 256 GB (4x64 GB) DDIMMs, 3200 MHz, 16 GBIT DDR4 Memory | AIX, IBM i, and Linux |
| EMC3 | 32AD | 512 GB (4x128 GB) DDIMMs, 2933 MHz, 16 GBIT DDR4 | AIX, IBM i, and Linux |
| EMC4 | 32AE | 1 TB (4x256 GB) DDIMMs, 2933 MHz, 16 GBIT DDR4 | AIX, IBM i, and Linux |

### Memory activation features

Software keys are required to activate part or all of the physical memory that is installed in the Power E1080 to be assigned to logical partitions (LPAR). Any software key is available when memory activation feature is ordered and can be ordered at any time during the life-cycle of the server to help the user scale up memory capacity without outages, unless more physical memory upgrade and activation are required.

A server administrator or user cannot control which physical memory DDIMM features are activated when memory activations are used.

The amount of memory to activate depends on the feature code ordered; for example, if an order contains two of feature code EDAB (100 GB DDR4 Mobile Memory Activation for HEX), these feature codes activate 200 GB of the installed physical memory.

Different types of memory activation features that are available for the Power E1080 server are known to the PowerVM hypervisor as total quantity for each type. The PowerVM hypervisor determines the physical DDIMM memory to be activated and assigned to the LPARs.

Similar to processor core activation features, different types of permanent memory activation features are offered on the Power E1080 server. For more information about the available types of activations, 1.5.2, “Processor features” on page 16.

Orders for memory activation features must consider the following rules:

- The system must have a minimum of 50% activated physical memory. It can be activated by using static or static and mobile memory activation features.
- The system must have a minimum of 25% of physical memory activated by using static memory activation features.
- When a Power E1080 is part of a PEP 2.0 environment, the server must have a minimum of 256 GB of base memory activations.

Consider the following examples:

- For a system with 4 TB of physical memory, at least 2 TB (50% of 4 TB) must be activated.
- When a Power E1080 is part of a PEP 1.0 environment, a server with 4 TB of physical memory and 3.5 TB of activated memory requires a minimum of 896 GB (25% of 3.5 TB) of physical memory activated by using static activation features.
- When a Power E1080 is part of a PEP 2.0 environment, a server with 4 TB of physical memory requires a minimum of 256 GB of memory activated with base activation features.

Table 1-11 lists the available memory activation feature codes for Power E1080. The Order type column indicates whether the feature code is available for an initial order only, or also with a MES upgrade on a existing server only, or both.

## Table 1-11 Memory activation features.

| Feature code | Description | Order type |
|---|---|---|
| EDAB | 100 GB DDR4 Mobile Memory Activation for HEX | Both |
| EDAG | 256 GB Base Memory Activation (Pools 2.0) | Both |
| EDAH | 512 GB Base Memory Activation (Pools 2.0) | Both |
| EDAL | 256 GB Base Memory Activation Linux only | Both |
| EDAM | 512 GB Base Memory Activation Linux only | Both |
| EDAP | 1 GB Base Memory activation (Pools 2.0) from Static | Both |
| EDAQ | 100 GB Base Memory activation (Pools 2.0) from Static | Both |
| EDAR | 512 GB Base Memory activation (Pools 2.0) from Static | Both |
| EDAS | 500 GB Base Memory activation (Pools 2.0) from Static | Both |
| EDAT | 1 GB Base Memory activation (Pools 2.0) MES only | Both |
| EDAU | 100 GB Base Memory activation (Pools 2.0) MES only | Both |
| EDAV | 100 GB Base Memory Activation (Pools 2.0) from Mobile | Both |
| EDAW | 500 GB Base Memory Activation (Pools 2.0) from Mobile | Both |

