regional (e.g. one country), have lower bandwidth than Tier 1 ISPs, and deliver content to customers through Tier 3 ISPs. Tier 3 ISPs are the ones that charge your exorbitant fees for your home Internet every month.

![](bbox=[0.13, 0.18, 0.86, 0.48])

Figure 3-1. Simplified view of the Internet

There’s a lot more complexity to how the Internet works than described here. That level of networking and protocol complexity is beyond the scope of this chapter. From a distributed systems software perspective, we need to understand more about the ‘magic’ that enables all this hardware to route messages from say my cell phone, to my bank and back. This is where the IP protocol comes in.

# Communications Software

Software systems on the Internet communicate using the Internet Protocol suite.³ The Internet Protocol suite specifies host addressing, data transmission formats, message routing and delivery characteristics. There are four abstract layers, which contain related protocols that support the functionality required at that layer. These are, from lowest to highest:

+   1. The data link layer, specifying communication methods for data across a single network segment. This is implemented by the device drivers