| Feature code | CCIN | Description | OS support |
|---|---|---|---|
| EC2U | 58FB | PCIe Gen 3 2-Port 25/10 Gb NIC&ROCE SR/Cu Adapterᵃ | AIX, IBM i, and Linux |
| EC67 | 2CF3 | PCIe Gen 4 LP 2-port 100 Gb ROCE EN LP adapter | AIX, IBM i, and Linux |
| EN0S | 2CC3 | PCIe2 4-Port (10 Gb+1 GbE) SR+RJ45 Adapter | AIX, IBM i, and Linux |
| EN0T | 2CC3 | PCIe2 LP 4-Port (10 Gb+1 GbE) SR+RJ45 Adapter | AIX, IBM i, and Linux |
| EN0X | 2CC4 | PCIe2 LP 2-port 10/1 GbE BaseT RJ45 Adapter | AIX, IBM i, and Linux |
| a. Requires SFP to provide 10 Gb, 2 Gb, or 1 Gb BaseT connectivity |

a. Requires SFP to provide 10 Gb, 2 Gb, or 1 Gb BaseT connectivity

# 1.5.5 System node disk and media features

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

# 1.5.6 System node USB features

The Power E1080 supports one stand-alone external USB drive that is associated to feature code EUA5. The feature code includes the cable that is used to the USB drive to the preferred front accessible USB port on the SCU.

The Power E1080 server node does not offer an integrated USB port. The USB 3.0 adapter feature code EC6J is required to provide connectivity to an optional external USB DVD drive and requires one system node or I/O expansion drawer PCIe slot. The adapter connects to