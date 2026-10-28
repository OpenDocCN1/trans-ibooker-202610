
## FIGURE 13-1 Internet Control Message Protocol (ICMP) sniffed in Snort

d. Enter the following command:

```
sudo snort -v -i lo
```

To see the difference, open up a new terminal, ping the loopback address (127.0.0.1), and observe the output in the terminal in which Snort is sniffing.

You can break out with CTRL-C.

3a–3c

## Step 3 Run Snort, generating upper layer data, in addition to Layer 3 and Layer 4 header information.

a. Enter the following command:

```
sudo snort -vd
```

The following is from the Snort man page:

-d Dump the application layer data when displaying packets in verbose or packet logging mode.

After running Snort with the -v and -d options together (-vd), stop Snort with CTRL-C.

Snort runs and shows the upper layer data, in addition to information from Layer 3 and Layer 4 (when TCP or UDP is used), as shown in Figure 13-2.

```
WARNING: No preprocessors configured for policy 0.
07/29-14:27:25.257680 A0:AF:BD:BB:10:52 -> 00:0C:29:F6:47:73 type:0x800 len:0x4A
192.168.1.108 -> 192.168.1.126 ICMP TTL:128 TOS:0x0 ID:45524 IpLen:20 DgmLen:60
Type:8 Code:0 ID:1 Seq:10 ECHO
=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+

WARNING: No preprocessors configured for policy 0.
07/29-14:27:25.257702 00:0C:29:F6:47:73 -> A0:AF:BD:BB:10:52 type:0x800 len:0x4A
192.168.1.126 -> 192.168.1.108 ICMP TTL:64 TOS:0x0 ID:55319 IpLen:20 DgmLen:60
Type:0 Code:0 ID:1 Seq:10 ECHO REPLY
=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+

WARNING: No preprocessors configured for policy 0.
07/29-14:27:26.264515 A0:AF:BD:BB:10:52 -> 00:0C:29:F6:47:73 type:0x800 len:0x4A
192.168.1.108 -> 192.168.1.126 ICMP TTL:128 TOS:0x0 ID:45525 IpLen:20 DgmLen:60
Type:8 Code:0 ID:1 Seq:11 ECHO
=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+

WARNING: No preprocessors configured for policy 0.
07/29-14:27:26.264537 00:0C:29:F6:47:73 -> A0:AF:BD:BB:10:52 type:0x800 len:0x4A
192.168.1.126 -> 192.168.1.108 ICMP TTL:64 TOS:0x0 ID:55403 IpLen:20 DgmLen:60
Type:0 Code:0 ID:1 Seq:11 ECHO REPLY
=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+

WARNING: No preprocessors configured for policy 0.
07/29-14:27:26.656873 44:4A:DB:88:BA:20 -> 01:00:5E:00:00:FB type:0x800 len:0x82
192.168.1.140:5353 -> 224.0.0.251:5353 UDP TTL:255 TOS:0x0 ID:44194 IpLen:20 DgmLen:116
Len: 88
=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+

WARNING: No preprocessors configured for policy 0.
07/29-14:27:26.656903 44:4A:DB:88:BA:20 -> 33:33:00:00:00:FB type:0x86DD len:0x96
fe80::40a:5819:38f2:a9dd:5353 -> ff02::fb:5353 UDP TTL:255 TOS:0x0 ID:0 IpLen:40 DgmLen:
Len: 88
=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+

WARNING: No preprocessors configured for policy 0.
07/29-14:27:27.270258 A0:AF:BD:BB:10:52 -> 00:0C:29:F6:47:73 type:0x800 len:0x4A
192.168.1.108 -> 192.168.1.126 ICMP TTL:128 TOS:0x0 ID:45526 IpLen:20 DgmLen:60
```

FIGURE 13-3 Snort showing Layer 2 information

c. Enter the following command:

```
sudo snort -vde
```

After running Snort with the -v, -d, and -e options together (-vde),

a. Enter the following command:
`sudo snort -l .`

## Note

The dot at the end of the command represents the current directory.

The following is from the Snort man page:

-l log-dir

Set the output logging directory to *log-dir*. All plain text alerts and packet logs go into this directory. If this option is not specified, the default logging directory is set to /var/log/snort.

You don’t need **-v**, **-d**, or **-e**, contrary to documentation out there, including Snort’s own documentation.

b. Send a ping from the Windows 10 VM or host machine to the Ubuntu VM running Snort.

Stop Snort with CTRL-C.

2a, 2b

## Step 2 Examine the logged packets.

a. Enter the following command:
`ls`
See the name of the log file generated.

b. Enter the following command:
`sudo wireshark snort.log.159604382`

Wireshark and packet sniffing in general were introduced in Chapter 12 and will be explored further in this and future chapters. For now, simply open up the logged packets in Wireshark. In the display filter bar (where it says “Apply a display filter…”), type **icmp** and press ENTER to see just the pings from Step 1b.

60 MINUTES

## Lab Exercise 13.04: Snort Network Intrusion Detection System Mode

In Network Intrusion Detection System (NIDS) mode, Snort monitors network traffic and analyzes the traffic against a user-defined ruleset that’s stored in a configuration file. Snort will then perform specific actions, based on what has been identified. In this mode, Snort logs and generates alerts for packets matching certain rules.

Snort consists of many components. First is a packet decoder, which determines the protocols used in the frame, like Ethernet at Layer 2, IP at Layer 3, and TCP or UDP at Layer 4. The decoder saves this information with the upper layer data, like DHCP, DNS, FTP, HTTP, SSH, TLS, and their sizes. The decoder also looks for errors, or anomalies, in the fields of these headers.

If Snort is running in inline IPS mode, rules configured in the snort.conf configuration file can even cause packets to be dropped if they’re malicious or malformed. For example, if the Ethernet Type field indicates an IPv4 packet is encapsulated inside of a frame, but the size of the IP header that was captured is less than the 20-byte minimum length for an IPv4 header, Snort will generate an alert and move the packet out of the decoding phase.

The second component consists of multiple preprocessors that arrange and modify packets before they are sent to the detection engine for analysis. Some preprocessors detect some basic anomalies by defragmenting packets that attackers have fragmented, for deception purposes. They can also perform HTTP URL decoding, if attackers used hexadecimal characters in the URL for deception purposes. Preprocessors are specialized. Some can detect and log port scanning activities, while others can detect anomalies in ARP frames to identify ARP spoofing. A preprocessor can arrange a string so that it is detectable by the IDS. A string of scripts\ransomware, representing a directory, can be arranged many different ways; for example, all of the following directory references will get malware to the same scripts\ransomware directory on a Windows system regardless of the number

highest priority rule is used. There are some factors that could influence how the time-critical detection engine behaves, including the number of rules, the power of the machine on which Snort is running, the speed of the internal bus used in the Snort machine, and the load on the network.

The fourth component, the logging and alerting system, will hear from the detection engine about an intrusion and generate a log entry and alert.

The fifth component, output modules or plug-ins, controls the type of output produced by the logging and alerting system. Options include generating log reports, logging alert reports in a file, sending SNMP (Simple Network Management Protocol) traps, logging to a MySQL database, sending a message to a syslog server, generating Extensible Markup Language (XML) output, modifying configurations on routers and firewalls, and sending Server Message Block (SMB) messages.

## Learning Objectives

In this lab exercise, you’ll use Snort in its most beneficial and effective way. At the end of this lab exercise, you’ll be able to

- Edit the Snort configuration file
- Create your own custom Snort configuration file
- Explore the various rules that come with Snort
- Configure your own custom Snort rules
- Implement Snort as an IDS
- Analyze the results

## Lab Materials and Setup

The materials you need for this lab are

- The *Principles of Computer Security: CompTIA Security+ and Beyond* textbook
- A web browser with an Internet connection
- The Ubuntu VM and Snort you installed in Lab Exercise 13.01
- The Windows 10 VM you installed in Chapter 1 or the Windows 10
