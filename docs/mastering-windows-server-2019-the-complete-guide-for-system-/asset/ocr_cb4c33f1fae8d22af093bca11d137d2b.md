In the more recent versions of Windows (particularly in Win10), you don't usually get the prompt asking what kind of network it is; instead, you get that prompt asking whether or not you want to allow your computer to communicate with other devices on the new network. Effectively, this is still the same prompt, and the decision you make at that prompt will assign your connection to either the public or private firewall profile.

Each network connection gets assigned its own profile definition. You could certainly have more than one firewall profile active at the same time on the same system. For example, my RA1 server is connected to both the corporate network as well as the public internet. Inside WFAS, you can see that both the Domain Profile and Public Profile are active:

![](bbox=[0.13, 0.3, 0.87, 0.7])

Figure 9.10: Multiple NICs can mean multiple firewall profiles

Alternatively, if you open up Network and Sharing Center on this server, we can also see the profiles listed here, and you can easily tell which NIC is using which profile: