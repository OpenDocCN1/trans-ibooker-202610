+   - Azure Firewall
+   - IP restrictions
+   - Network map and topology

Before starting, answer the following question:

Which of the following are considered an Azure network pattern?

+   A. Hybrid
+   B. Hub and spoke
+   C. DMZ
+   D. All the above
+   E. B and C

The answer is that all of these are patterns that can be implemented into the Azure networking architecture.

# Traffic Filtering with NSG, ASG, and NVA

When I read the word filter, my first visualization is coffee. That’s kind of old-school because these days the beans are ground and the coffee is made without a filter in these ultramodern mechanized appliances. But many may relate to the brown paper that formed a barrier when placed into a coffee machine, followed by some previously ground coffee beans that hot water flowed over, brewing an element of satisfaction. The same could be said about the filtering of network traffic, in that instead of the barrier being brown paper, it’s an NSG, an ASG, or an NVA.

An NSG was introduced earlier in Figure 3.10. Table 3.12 describes the properties of an NSG. Note that there is a limit of 5,000 NSGs per Azure subscription.

# TABLE 3.12 NSG Property Descriptions

| Property | Description |
|---|---|
| Priority | The order in which the rule is applied. Rule #1 processes before rule #2, and processing stops after the first match. |
| Name | The name of the NSG; it must be unique. |
| Port | Can be a single port or a range of ports |
| Protocol | TCP, UDP, or Any |
| Source | Private IP CIDR range, specific IP(s), service tag, or ASG |
| Destination | Private IP CIDR range, specific IP(s), service tag, or ASG |
| Action | Allow or Deny |