
PORT STATE SERVICE
22/tcp open ssh
135/tcp open msrpc
139/tcp open netbios-ssn
....
49158/tcp open unknown
49159/tcp open unknown
MAC Address: 00:0C:29:38:B3:A9 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 3.25 seconds

As we can see, we have passed the -sT parameter, which denotes that we want to perform a TCP connect scan. A TCP connect scan is based on a three-way handshake process, hence, the returned results of this scan are considered accurate.

When using Nmap without specifying the port range, Nmap scans the most common 1,000 ports for each protocol.

3. The SYN scan [-sS] is considered a stealth scanning technique, as it never forms a complete connection between the target and the scanner. Hence, it is also called half-open scanning. Let's analyze a SYN scan on the target:

```
msf > nmap -sS 192.168.216.10 -p 22-5000
[*] exec: nmap -sS 192.168.216.10 -p 22-5000

Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-19 09:00 EDT
Nmap scan report for 192.168.216.10
Host is up (0.00063s latency).
Not shown: 4967 closed ports
PORT STATE SERVICE
22/tcp open ssh
135/tcp open msrpc
...
3920/tcp open exasoftport1
4848/tcp open appserv-http
MAC Address: 00:0C:29:38:B3:A9 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 685.27 seconds
msf >
```

The -sS parameter will instruct Nmap to perform a SYN scan on the target machine. The output of both TCP connect and the SYN scan are similar in most of the cases, but the only difference lies in the fact that SYN scans are difficult to detect by firewalls and **Intrusion Detection Systems (IDS)**. However, modern firewalls are capable enough to catch SYN scans, as well. The -p parameter shows the range of port numbers that we want to scan. Using -p 0-65535, or -p - for short, will scan all the available ports.

4. The UDP scan [-sU] is the scanning technique to identify open UDP ports on the target. 0-byte UDP packets are sent to the target machine and the recipient of an ICMP port unreachable message shows that the port is closed; otherwise, it is considered open. It can be used in the following manner:

```
msf > nmap -sU 192.168.216.10
[*] exec: nmap -sU 192.168.216.10

Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-19 09:09 EDT
Nmap scan report for 192.168.216.10
Host is up (0.00064s latency).
Not shown: 994 closed ports
PORT STATE SERVICE
137/udp open netbios-ns
138/udp open|filtered netbios-dgm
500/udp open|filtered isakmp
4500/udp open|filtered nat-t-ike
5353/udp open|filtered zeroconf
5355/udp open|filtered llmnr
MAC Address: 00:0C:29:38:B3:A9 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 319.56 seconds
msf >
```

The previous command will check whether the most common 1,000 ports for the UDP protocol on 192.168.56.102 are open or not.

## How it works...

We have analyzed three different types of Nmap scans that can be very helpful during penetration testing. Nmap provides lots of different modes for scanning the target machine. Here, we will focus on three scan types, namely, the TCP connect scan, the SYN stealth scan, and the UDP scan. The different scan options of Nmap can also be combined in a single scan in order to perform a more advanced and sophisticated scan over the target. Let's move ahead and start the scanning process.

During a penetration test, the scanning process can provide lots of useful results. Since the information collected here will form the basis of penetration testing, proper knowledge of scan types is highly recommended. Let's now take a deeper look into each of these scan techniques we just learned.

The TCP connect scan is the most basic scanning technique in which a full connection is established with the port under test. It uses the operating system's network functions to establish connections. The scanner sends a SYN packet to the target machine. If the port is open, it returns an ACK message back to the scanner. The scanner then sends an ACK packet back to the target showing the successful establishment of a connection. This is called a three-way handshake process. The connection is terminated as soon as it is opened. This technique has its benefits, but it is easily traceable by firewalls and IDS.

A SYN scan is another type of TCP scan, but it never forms a complete connection with the target. It doesn't use the operating system's network functions; instead, it generates raw IP packets and monitors for responses. If the port is open, then the target will respond with an ACK message. The scanner then sends a reset connection (RST) message and ends the connection. Hence, it is also called half-open scanning. This is considered as a stealth scanning technique as it can avoid raising a flag in some misconfigured firewalls and IDS.

UDP scanning is a connectionless scanning technique; hence, no notification is sent back to the scanner, whether the packet has been received by the target or not. If the port is closed, then an ICMP port unreachable message is sent back to the scanner. If no message is received, then the port is reported as open. This method can return false results as firewalls can block the data packets and, therefore, no response message will be generated and the scanner will report the port as open.

## There's more...

Let's look further into the Nmap scans and see how we can club different scan types into one.

## Operating system and version detection

There are some advanced options provided by Nmap, apart from port scanning. These options can help us gain more information about our target. One of the most widely used options is **operating system identification [-O]**. This can help us in identifying the operating system running on the target machine.

An operating system detection scan output is shown as follows:

```
msf > nmap -O 192.168.216.129
[*] exec: nmap -O 192.168.216.129

Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-19 09:28 EDT
Nmap scan report for 192.168.216.129
Host is up (0.0012s latency).
Not shown: 977 closed ports
PORT STATE SERVICE
21/tcp open ftp
22/tcp open ssh
23/tcp open telnet
...

Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Network Distance: 1 hop

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 3.91 seconds
msf >
```

As we can see, Nmap has successfully detected the operating system of the target machine. This can ease our task of finding the right exploits, in accordance with the operating system of the target.

The other widely used Nmap option is **version detection [-sV]** of different open ports on the target. It can be mixed with any of the scan types that we saw previously, to add an extra bit of information of what version of services are running on the open ports of the target:

```
msf > nmap -sV 192.168.216.129
[*] exec: nmap -sV 192.168.216.129

Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-19 09:30 EDT
Nmap scan report for 192.168.216.129
Host is up (0.00049s latency).
Not shown: 977 closed ports
PORT STATE SERVICE VERSION
...

irc.Metasploitable.LAN; OSs: Unix, Linux; CPE:
cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.57 seconds
msf >
```

As we can see, an extra column of versions has been added in our scan output, which reports about the different versions of services running on the target machine.

## Increasing anonymity

Sometimes it is essential to perform scans in an anonymous manner. The firewall and IDS logs can reveal your IP address if you perform a scan without using security measures. One such feature is provided in Nmap, called **decoy (-D)**.

The decoy option does not prevent your IP address from getting recorded in the log file of firewalls and IDS, but it does make the scan look scary. It adds other torrents in the log files, thus creating an impression that there are several other attackers scanning the machine simultaneously. So, if you add two decoy IP addresses, the log file will show that the request packets were sent from three different IP addresses; one will be yours and the other two will be the fake addresses added by you:

```
msf > nmap -sT 192.168.216.10 -D 192.168.216.13,192.168.216.25
```
