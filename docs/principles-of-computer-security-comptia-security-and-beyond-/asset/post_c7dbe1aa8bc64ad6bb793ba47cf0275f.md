
each statement has eight matches. The first statement was matched from the two sets of four ICMP Echo Requests sent from PC1 (10.1.0.1), while the second statement was matched from the two sets of four ICMP Echo Requests sent from PC0 (10.1.0.100).

+   h. On Router2, in global configuration mode, enter **no access-list 1** to remove the ACL.

+   i. In interface configuration mode of GigabitEthernet0/0/0, enter **no ip access-group 1 out** to remove the application of the ACL to interface g0/0/0.

60 MINUTES

## Lab Exercise 10.03: Extended ACLs on Routers

## Learning Objectives

In this lab exercise, you’ll go further in your exploration of filtering packets. At the end of this lab exercise, you’ll be able to

+   Configure extended ACLs
+   Test extended ACLs
+   Modify extended ACLs

## Lab Materials and Setup

The materials you need for this lab are

+   The *Principles of Computer Security: CompTIA Security+ and Beyond* textbook
+   A web browser with an Internet connection
+   Completed lab exercises in Cisco Packet Tracer from the previous chapter
+   One or more Cisco Ethernet switches with at least two PCs (optional, as an alternative to Packet Tracer)

## Let’s Do This!

Extended ACLs can filter by three or four criteria: source IP, destination IP, protocol, and port, with port being the only optional parameter. This gives you more granular control over the rules.

Let’s say we want to block HostA from accessing the Apache Web Server on ServerA, but not any other services on ServerA, as shown in Figure 10-2.

![](img/6e921d997751f0a0f87e5170ac9657a1_308_0.png)

FIGURE 10-2 Extended ACL topology example

We need an extended ACL in this case. Let’s walk through an ACL that’s extended. The extended ACL statement starts off the same way as a standard ACL statement, with access-list.

For an extended ACL, the number of the ACL falls within the inclusive range of 100 to 199. In this case, we’re using 100. Next we have, just like in a standard ACL, the keyword permit or deny, depending on what we’re trying to do:

```
access-list 100 deny
```

That’s where the similarities end. At this point in the extended ACL statement, we need to specify a protocol. Common entries are TCP, UDP, ICMP, and IP. In our statement, we’re selecting TCP, which will only match traffic encapsulated in a TCP header, at Layer 4:

```
access-list 100 deny tcp
```

After that, there are two IP addresses preceded by the keyword host, as shown next. The first represents the source; the second represents the destination. Of course, each of those could’ve been written with the IP address first, followed by a wildcard mask of 0.0.0.0, as mentioned earlier. Extended ACL statements cannot leave off the wildcard mask for a host, even though standard ACL statements can.

```
access-list 100 deny tcp host 129.21.19.75 host
129.21.1.40
```

At this point, this ACL statement will in fact block all connection-oriented TCP-based traffic from the source 129.21.19.75 to the destination 129.21.1.40. This is not what we want, though. We just want to block the source from accessing the web server, but not any other service running on the machine. Maybe there’s an FTP server or an SSH server (FTP and SSH both use TCP at Layer 4). The source should be able to access these services.

Now we’re going to add the operator eq (equal to), in this case to achieve our desired result. Following eq is 80, as shown next. This means only filter traffic from source 129.21.19.75 to destination 129.21.1.40 if the TCP segment has a destination port of 80, meaning the traffic is destined for the web server on 129.21.1.40.

```
access-list 100 deny tcp host 129.21.19.75 host
129.21.1.40 eq 80
```

That might be a perfectly configured statement, but at this point our ACL blocks everything. Remember that if packets don’t match any ACL statements, they are denied, which means the router will filter/drop these packets. We’ve got to override the implicit statement at the end of every ACL for packets not meeting any explicitly configured statements.

The way we do that with an extended ACL is as follows: the syntax starts off with access-list 100, as explained before, followed with permit, as
explained before. Now, however, instead of just the word **any**, like we saw with the standard ACL, we input **ip any any**, as shown here:

```
access-list 100 deny tcp host 129.21.19.75 host
129.21.1.40 eq 80
access-list 100 permit ip any any
```

Remember that an extended ACL’s only optional parameter is **port**, where the first statement is represented by **eq 80**. We need to include a protocol in every extended ACL statement. To include all protocols in the TCP/IP suite, simply enter **ip**. Anything encapsulated in an IP header matches **ip**.

What about the two instances of **any**? The first represents any source, and the second represents any destination. Now packets not meeting the first statement will match this second statement and will be filtered.

Extended ACLs should be applied as close to the source as possible. In our earlier example, that would mean the ACL would be applied as an inbound ACL on the right interface of RouterC. Why tie up bandwidth and processing power all the way from RouterC to RouterB if traffic from HostA will be blocked anyway? The logic is to filter it as early as possible. This couldn’t be done with the standard ACL, because the only criteria specified is the source IP address. However, if we apply an inbound ACL to the right interface of RouterC, blocking traffic from HostA to our web server, with the destination port of 80, all other traffic from HostA will be unfiltered.

This lab exercise continues with the topology established in the previous chapter. If you haven’t completed that chapter, you must go back and complete it before attempting the lab exercises in this chapter. Furthermore, some of the instructions assume you have learned how to move around the different modes of the routers as well as navigate the devices in the Packet Tracer topology from the previous chapter.

### Note

**Press ENTER after each command.**

1a–1e

## Step 1 Configure and apply an extended ACL that blocks specific traffic

the destination port wasn’t 80.

## Lab Analysis

+   1. What are reasons for configuring port security?
+   2. What are the actions port security can take when violations occur?
+   3. What do standard ACLs filter by?
+   4. What do extended ACLs filter by?
+   5. Where should standard ACLs be placed?
+   6. Where should extended ACLs be placed?
+   7. How is the purpose of a wildcard mask different from that of a subnet mask?
+   8. What do inbound and outbound mean from an ACL perspective and how are they different from their normal usage?

## Key Term Quiz

Use the terms from the list to complete the sentences that follow.

interface

MAC address

port

source IP address

+   1. Port security on switches can prevent a CAM overflow attack, making sure that only one ___________ is heard on a switch port.
+   2. Standard ACLs filter by __________.
+   3. Extended ACLs can, but do not have to, filter by __________.
+   4. There can be one inbound and one outbound ACL per __________.

## Chapter 11
Authentication and Remote Access

## Lab Exercises

+   11.01 Dictionary Attacks on Linux Passwords with John the Ripper
+   11.02 Brute Force Attacks on Linux Passwords with crunch and John the Ripper
+   11.03 Dictionary Attacks and Brute Force Attacks on Windows Passwords with Mimikatz, crunch, and John the Ripper
+   11.04 Rainbow Table Attacks on Windows Passwords with ophcrack
+   Lab Analysis
+   Key Term Quiz

One of the biggest takeaways from Verizon’s 2020 Data Breach Investigations Report (DBIR) was that over 80 percent of hacking-related breaches involve brute force or the use of stolen credentials.

## Cross-Reference

The Verizon DBIR was introduced and examined in Chapter 1.

Although they’re not the best choice for authentication and gaining remote access to systems and networks, passwords (something you know) are still more heavily used than security tokens/key fobs or smart cards (something you have) and biometrics (something you are).

Attackers can use a technique called password guessing in which they manually enter passwords at a login prompt to gain access to an account
