# IPv6

In this chapter, you will

+   - Learn about the need for IPv6
+   - Learn about IPv6 enhancements and addressing
+   - Learn how to configure IPv6 on an interface
+   - Learn how to configure routing with IPv6

This chapter introduces you to the next generation of TCP/IP: IP version 6 (IPv6). Because of the many deficiencies found in IPv4, as well as the poor scalability for hierarchical addressing in IPv4, IPv6 was developed to meet the rapidly growing needs of small companies, corporations, and the explosive growth of the Internet, especially in emerging markets such as China and India. In this chapter you’ll discover why IPv6 is necessary, the different kinds of IPv6 addresses, routing with IPv6, and how to configure a basic IPv6 setup. The CCNP certification goes into much more depth than what you’ll find in this chapter.

# Necessity of IPv6

An IPv4 address is 32 bits in length; that means there are 2³² actual IP addresses, which equals 4.3 billion addresses. Not all of these are usable, however: only 3.7 billion of these are actually usable. Many addresses are reserved, such as research (239–254), broadcast (255), multicast (224–239), private (10, 172.16, and 192.168), and loopback addresses (127). And, of course, many of the usable addresses are already assigned, leaving very, very few addresses for new growth.