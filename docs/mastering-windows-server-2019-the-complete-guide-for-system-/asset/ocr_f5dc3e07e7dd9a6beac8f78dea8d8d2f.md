As we look over the parts and pieces of AD, it will become clearer what information is stored within and why it is so important. If you are new to IT and do not yet have a good grasp of AD, make sure to learn this technology! AD is the hinge upon which almost everything in a Windows Server world revolves. Is it possible to utilize Windows servers without them being connected and joined to Active Directory? Yes. Is it likely you will ever find this scenario at play in the real world? No. In this chapter, we will learn the following:

+   - What is a domain controller?
+   - Creating your first domain
+   - Multiple domain controllers for redundancy
+   - Active Directory Users and Computers
+   - Active Directory Domains and Trusts
+   - Active Directory Sites and Services
+   - Active Directory Administrative Center
+   - Read-only domain controllers
+   - Group Policy

# What is a domain controller?

If we are going to discuss the core infrastructure services that you need to piece together your Microsoft-driven network, there is no better place to start than with the domain controller. A Domain Controller, commonly referred to as a DC, is simply a server that is hosting Active Directory. It is a central point of contact, a central "hub" so to speak, that is accessed prior to almost any communication that takes place between a client and server in your network. Perhaps the easiest way to describe it is as a storage container for all identification that happens on the network. Usernames, passwords, computer accounts, groups of computers, servers, groups and collections of servers, security policies, file replication services, and many more things are stored within and managed by DCs. If you are not planning to have a domain controller be one of the first servers in your Microsoft-centric network, you might as well not even start building that network. DCs are essential to the way that our computers and devices communicate with each other and with the server infrastructure inside our companies.