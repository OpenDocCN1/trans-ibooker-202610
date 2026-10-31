# There's more...

Let's look further into the Nmap scans and see how we can club different scan types into one.

# Operating system and version detection

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