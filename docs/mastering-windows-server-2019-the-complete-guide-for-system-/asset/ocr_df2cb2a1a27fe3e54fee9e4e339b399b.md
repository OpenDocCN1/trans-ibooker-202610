One more click of the **Next** button, and we can now see our option for which **Cluster operation mode** we want to run. Depending on your network configuration, choose between **Unicast**, **Multicast**, and **IGMP multicast**:

![](bbox=[0.13, 0.14, 0.87, 0.47])

Figure 12.6: Choosing a cluster operation mode

The following screenshot of our NLB wizard allows you to configure port rules. By default, there is a single rule that tells NLB to load balance any traffic coming in on any port, but you can change this if you want. I don't see a lot of people in the field specifying rules here to distribute specific ports to specific destinations, but one neat feature in this screenshot is the ability to disable certain ranges of ports.