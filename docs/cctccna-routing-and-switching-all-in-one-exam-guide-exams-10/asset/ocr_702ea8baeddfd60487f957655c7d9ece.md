In this chapter, you learned that from time to time you may need to divide a network range into multiple network blocks (subnets) to follow the physical structure of the network. Because the Cisco CCNA certification exam will have a number of questions that rely on your knowledge of subnetting, be sure to study this chapter well.

To subnet a network, you will need to know how many networks or hosts per network are required. You can then convert host bits in the subnet mask to become subnet bits, essentially altering your subnet mask and creating more networks while supporting less hosts per network.

The Variable Length Subnet Mask (VLSM) feature enables you to have a different subnet mask for each subnet on the network; this lets you make better use of addresses.

# Quick Review

# Understanding Subnetting

+   - The purpose of subnetting is to break one network ID into multiple subnetworks (subnets) so that you can follow the physical structure of the network.
+   - With subnetting, you take host bits from the subnet mask and turn them into network bits—thus creating more networks but fewer machines per network.
+   - To determine how many bits to take from the host ID portion of the subnet mask, use the formula 2^{masked bits} − 2 = number of networks.
+   - For each subnet created, you must calculate the network ID, first valid address, last valid address, broadcast address, and new subnet mask.

# Subnetting Shortcuts

+   - Look at the value of the last bit that was used to subnet and use that as your increment value.
+   - Using the increment value, list out all network IDs, and then add one to that to get the first valid address of each network.