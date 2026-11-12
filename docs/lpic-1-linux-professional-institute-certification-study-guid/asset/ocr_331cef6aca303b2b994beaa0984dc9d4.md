![](bbox=[0.145, 0.112, 0.347, 0.265])

Securing your systems is a crucial task. The variety of computer attacks and security breaches escalates every day. Thus, the need to improve your servers' security grows as well.

System administrators must know how to set at least a basic level of host security. This includes auditing the network services their systems offer, reviewing system configurations to ensure that local security policies are properly enforced, and employing appropriate techniques to secure both data and communications. We'll cover those topics and more in this chapter on system security.

# ## Administering Network Security

Imagine a home with a door to the outside from every room in the house. That would be a lot of locks to check before leaving the house or going to bed. In the world of network security, having services on your system that are not used is similar to having unnecessary doors to the outside. A term used in cybersecurity that applies to this scenario is attack surface. An attack surface is all the various points where a malicious person may try to gain access to something for nefarious reasons.

Continuing the analogy, envision that the multidoor home has a few doors with old locks that no longer properly work and need to be replaced or repaired. This situation is similar to older security software on your system that needs to be updated, reconfigured, or even ousted so that more modern and secure applications can take its place.

In this section, we'll take a look at auditing your system for unused services in order to minimize its attack surface. We'll also look at some older network security technology and how it should be handled.

# ### Disabling Unused Services

One method for minimizing your system's attack surface is by disabling unused services (daemons) on your system. Having less software running lessens the various targets a malicious actor (another name for an attacker) can leverage.

While it is tempting to think you know every service running on your Linux system, to be sure, it's always a good idea to run a thorough audit. In this section, we'll look at various examinations you can conduct to see what services your system is offering.

# ### Discovering Open Ports with nmap

The Network Mapper (nmap) utility is often used for penetration testing (the practice of testing a system, its network, and its apps to find computer security weaknesses that a malicious attacker could exploit). However, it is also very useful for network service auditing.