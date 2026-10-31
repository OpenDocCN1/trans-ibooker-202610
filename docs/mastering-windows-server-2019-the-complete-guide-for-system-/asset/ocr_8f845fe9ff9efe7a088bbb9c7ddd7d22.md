+   - **WAC**: WAC now includes tools and functionality for defining and managing S2D clusters. This will make adoption much easier for folks who are not overly familiar with PowerShell.
+   - **Improved capability**: We can now host four petabytes per cluster, compared to one petabyte in Server 2016. Keep in mind, though, that it is recommended to maintain a maximum of 400TB per server within the cluster.
+   - **Improved speed**: While S2D has been fast since the very first version, we have some efficiency improvements in Server 2019. At last year's Ignite conference, Microsoft showcased an 8-node S2D cluster that was capable of achieving 13,000,000 IOPS. Holy moly!

# Summary

Redundancy is a critical component in the way that we plan infrastructure and build servers in today's world. Windows Server 2019 has some powerful capabilities built right into it that you can utilize in your own environments, starting today! I hope that by gleaning a little more information about both NLB and failover clustering, you will be able to expand the capabilities of your organization by employing these techniques and stretching the limits of your service uptime. Even for a small-medium business, there are some great replication options in Windows Server, made possible with a limited set of servers. You could use Storage Replica to replace many other third-party options that currently exist in these environments, giving you redundant file servers in a quick-and-easy fashion. For any enterprise-class organization that hasn't tested the waters with S2D yet, what's stopping you? Let's get serious about resiliency for your Hyper-V infrastructure. HCI will change the way that you work and give you some peace of mind that you didn't think was possible in a world aiming for 99.999% uptime.

# Questions

+   1. Which technology is more appropriate for making web server traffic redundant – Network Load Balancing or failover clustering?
+   2. In Network Load Balancing, what do the acronyms DIP and VIP stand for?
+   3. What are the three NLB modes?
+   4. In Windows Server 2019, is Network Load Balancing a role or a feature?
+   5. What roles are most often used with failover clustering?
+   6. What type of small device can now be used as a cluster quorum witness (this is brand new as of Server 2019)?
+   7. True or False – Storage Spaces Direct requires the use of SSD hard drives.