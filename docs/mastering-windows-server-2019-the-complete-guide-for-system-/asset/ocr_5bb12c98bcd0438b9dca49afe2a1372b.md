![](bbox=[0.13, 0.08, 0.87, 0.4])

Figure 4.33: IPAM's centralized interface

Management of DHCP from within IPAM is particularly useful, as classic administration of DHCP from within its own management console can get pretty messy in an environment with many different DHCP scopes. IPAM provides an interface from which you can quickly and easily see information about all scopes together, and even run some fun PowerShell cmdlets to quickly gather data that would have otherwise taken manual poking and prodding to discover in the past:

+   - Find-IpamFreeSubnet
+   - Add-IpamSubnet
+   - Find-IpamFreeRange (this one is nice because it helps you to quickly discover a range of available IP addresses)
+   - Add-IpamRange