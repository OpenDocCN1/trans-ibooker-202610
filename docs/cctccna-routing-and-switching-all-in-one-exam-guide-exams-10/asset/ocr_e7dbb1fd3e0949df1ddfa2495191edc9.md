subnet, the number of addresses is 64—this is known as the block size (or subnet size).

Keep in mind that if there are 6 bits for host bits (six 0s below in the fourth octet), that means the two remaining bits are subnet bits (highlighted in bold next). With this information, you can now document the subnet mask for subnet 3 as follows:

```
11111111.11111111.11111111.11000000 = 255.255.255.192
```

Now to show what address range this is, you can plot the network on a grid showing the different IP addresses used in the fourth octet. Note that subnet 3 will be using a block of 64 addresses, but you can use only 62 of them (2 are reserved). This means numbers 0–63 are the addresses for subnet 3, as shown in Figure 5-4. Notice that this is documented as 216.83.11.0/26. The /26 indicates that 26 bits in the subnet mask have been enabled, and it controls how many addresses are used in this subnet (64).

![](bbox=[0.12, 0.45, 0.88, 0.6])

# Figure 5-4 Plotting addresses for the largest VLSM subnet

Now look at the next highest number of hosts on a network, and you’ll see that subnet 1 requires 21 hosts (or 21 IP addresses). To calculate the subnet block for this, use the following formula:

```
2^5 = 32 − 2 = 30 hosts supported on subnet 1
```

The 30 hosts are a few more than the required 21, but this is okay, as described in previous examples. With this information, you can now document the subnet mask for subnet 1 as follows:

```
11111111.11111111.11111111.11100000 = 255.255.255.224
```