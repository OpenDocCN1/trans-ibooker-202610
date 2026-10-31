# How to do it...

The Metasploit Framework has several modules for information gathering. In this recipe, you will learn how to use some of these modules. However, I recommend that you explore all the auxiliary modules available in the framework.

# DNS Record Scanner and Enumerator

The DNS Record Scanner and Enumerator auxiliary module can be used to gather information about a domain from a given DNS server by performing various DNS queries, such as zone transfers, reverse lookups, SRV record brute forcing, and other techniques.

+   1. To run the auxiliary module, we use the use command followed by the module we want to use, in this case, auxiliary/gather/enum_dns. Then we can use the info command to display information about the module, such as the authors, basic options, and description, as shown here:

![](bbox=[0.13, 0.42, 0.87, 0.77])