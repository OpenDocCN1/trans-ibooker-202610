> **Important Note**
> 
> The ARP table gets reset after a certain period of time, so just sending one packet to spoof is not going to work properly. To be able to successfully spoof for longer periods, you need to constantly send these fake manufactured packets so that ARP tables don't get reset after a certain time.

# # Building an ARP spoof program

In this section, we will learn how to build an ARP spoof program. Before we move on, let's take a look at the ARP tables again in both Kali as well as the Windows. The ARP table in Kali Linux is as follows:

```
```text
(venv) —(kali㉿kali)-[~/packet-book-code/example2-introduction-scapy]
└─$ arp -a
? (192.168.74.2) at 00:50:56:ff:74:8b [ether] on eth0
? (192.168.74.254) at 00:50:56:f8:e6:bc [ether] on eth0
? (192.168.74.129) at 00:0c:29:be:47:14 [ether] on eth0
```
```

![](bbox=[0.149, 0.329, 0.851, 0.416])

The ARP table in Windows looks like this. Take a look at the highlighted fields:

```
```text
C:\Users\fahad-sarwar>arp -a

Interface: 192.168.74.129 --- 0xc
  Internet Address      Physical Address      Type
  192.168.74.2          00-50-56-ff-74-8b     dynamic
  192.168.74.128        00-0c-29-90-79-02     dynamic
  192.168.74.255        ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static
```
```

![](bbox=[0.189, 0.48, 0.81, 0.704])

As you can see, they have the correct MAC addresses for the router located at 192.168.74.2. Kali is located at 192.168.74.128, while Windows 10 is located at 192.168.74.129.

To spoof these devices, we will take on this problem step by step. First, we will tackle spoofing the victim machine with the MAC address of the router.