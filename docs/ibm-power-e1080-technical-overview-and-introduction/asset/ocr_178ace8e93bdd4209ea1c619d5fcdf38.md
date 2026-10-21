Important Power10 processor characteristics are listed in 2-1.

# Table 2-1 Summary of the Power10 processor chip and processor core technology

| Technology | Power10 processor |
|---|---|
| Processor die size | 602 mm² |
| Fabrication technology | ► CMOSᵃ 7-nm lithography
► 18 layers of metal |
| Maximum processor cores per chip | 15 |
| Maximum execution threads per core / chip | 8 / 120 |
| Maximum L2 cache core | 2 MB |
| Maximum On-chip L3 cache per core / chip | 8 MB / 120 MB |
| Number of transistors | 18 billion |
| Processor compatibility modes | Support for Power ISAᵇ of POWER8 and POWER9 |

a. Complimentary metal-oxide-semiconductor (CMOS)
b. Power instruction set architecture (Power ISA)

The Power10 processor is packaged as single-chip module (SCM) for exclusive use in the Power E1080 servers. The SCM contains the Power10 processor plus more logic that is needed to facilitate power supply and external connectivity to the chip. It also holds the connectors to plug SMP cables directly onto the socket to build 2-, 3-, and 4-node Power E1080 servers.

Figure 2-3 shows the logical diagram of the Power10 SCM.

![](bbox=[0.22, 0.53, 0.895, 0.835])

# Figure 2-3 Power10 single chip module