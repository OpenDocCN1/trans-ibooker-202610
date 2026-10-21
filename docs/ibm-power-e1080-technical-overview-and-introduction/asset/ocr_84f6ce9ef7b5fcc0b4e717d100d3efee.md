# 2.6.1 LAN adapters

The supported LAN adapters are listed in Table 2-16.

Table 2-16 LAN Adapters

| Feature code | CCIN | Description | Min | Max | OS support | Order type |
|---|---|---|---|---|---|---|
| 5260 | 576F | PCIe2 LP 4-port 1 GbE Adapter | 0 | 32 | AIX, IBM i, and Linux | Supported |
| 5899 | 576F | PCIe2 4-port 1 GbE Adapter | 0 | 192 | AIX, IBM i, and Linux | Supported |
| EC2T | 58FB | PCIe Gen 3 LP 2-Port 25/10 Gb NIC&ROCE SR/Cu Adapterᵃ | 0 | 32 | AIX, IBM i, and Linux | Both |
| EC2U | 58FB | PCIe Gen 3 2-Port 25/10 Gb NIC&ROCE SR/Cu Adapterᵃ | 0 | 192 | AIX, IBM i, and Linux | Both |
| EC67 | 2CF3 | PCIe Gen 4 LP 2-port 100 Gb ROCE EN LP Adapter | 0 | 24 | AIX, IBM i, and Linux | Both |
| EC77 | 2CFA | PCIe4 LP 2-port 100 Gb Crypto Connect-x6 DX QFSP56 | 0 | 24 | AIX, IBM i, and Linux | Both |
| EN0S | 2CC3 | PCIe2 4-Port (10 Gb+1 GbE) SR+RJ45 Adapter | 0 | 192 | AIX, IBM i, and Linuxᵇ | Supported |
| EN0T | 2CC3 | PCIe2 LP 4-Port (10 Gb+1 GbE) SR+RJ45 Adapter | 0 | 32 | AIX, IBM i, and Linuxᵃ | Supported |
| EN0W | 2CC4 | PCIe2 2-port 10/1 GbE BaseT RJ45 Adapter | 0 | 192 | AIX, IBM i, and Linuxᵇ | Both |
| EN0X | 2CC4 | PCIe2 LP 2-port 10/1 GbE BaseT RJ45 Adapter | 0 | 32 | AIX, IBM i, and Linuxᵃ | Both |
| EC2R | 58FA | PCIe3 LP 2-Port 10 Gb NIC&ROCE SR/Cu Adapter | 0 | 32 | AIX, IBM i, and Linux | Supported |
| EC2S | 58FA | PCIe3 2-Port 10 Gb NIC&ROCE SR/Cu Adapter | 0 | 192 | AIX, IBM i, and Linux | Supported |

a. Requires SFP to provide 10 Gb, 2 Gb, or 1 Gb BaseT connectivity
b. IBM i supported through VIOS

# 2.6.2 Fibre Channel adapters

The supported Fibre Channel adapters are listed in Table 2-17.

Table 2-17 Fibre Channel adapters

| Feature code | CCIN | Description | Min | Max | OS support | Order type |
|---|---|---|---|---|---|---|
| EN1A | 578F | PCIe Gen 3 32 Gb 2-port Fibre Channel Adapter | 0 | 192 | AIX, IBM i, and Linux | Both |