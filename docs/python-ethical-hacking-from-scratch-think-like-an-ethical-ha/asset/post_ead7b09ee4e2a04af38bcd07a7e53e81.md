
Now, if everything is done properly, you will see a folder created named venv.

Next, activate the virtual environment by running the following command:

```
source venv/bin/activate
```

Now, your virtual environment should be activated. Once it is activated, you can see the installed packages in the environment by typing the following command:

```
pip freeze
```

If you have any packages already installed in the environment, they will be listed down; otherwise, you will have nothing showing. Now, to install Scapy, write the following command:

```
pip3 install scapy
```

This should take some time to install. Once done, you can write the pip freeze command again to see the installed packages:

![](img/d31345bbfd15f63be2920f0de5fa30dc_90_0.png)

In this section, we have learned how we can install Scapy in our virtual environment and how to see whether it has been installed properly. Note that some functionalities of Scapy require the program to be run with higher privileges, otherwise they won’t work. In the next section, we will learn more about how Scapy is used and how we can manipulate network packets with Scapy.

## Understanding how Scapy works

In this part, we will learn about how Scapy works and how we can use it to create our own network manipulation tools. Let’s create a new file called main.py and open it. Once the file is open, we can import any Scapy module inside the file. In this section, we will create a small ping request to any website. Ping requests are usually used to test whether a device is available or not. A ping request (also called an echo request) uses an underlying ICMP application layer protocol. To import a package inside your program, write the following code:

```
from scapy.all import scapy
```

> **Important note**
> Note that in the latest version of Kali Linux, some dependencies have been changed and you may see an error related to missing files. To correct this issue, you can write the following command:
> `cd /usr/lib/x86_64-linux-gnu/sudo ln -s -f libc.a liblibc.a`

To send a ping request, you will need to create an IP layer packet, which will help you set the source and destination IP addresses. To import the IP layer, we can write the following command:

```
from scapy.all import IP
```

And lastly, to send and receive packets, we can use a function called sr. To import this function, use the following command:

```
from scapy.all import sr
```

This IP will be different for you depending on your system. You can find this IP using the sudo ifconfig command.

Then, we will define our source and destination IP:

```
src_ip = "192.168.74.128"
```

Then, we will define the destination IP. We want to create a ping request to a google.com server. You can either manually write the IP address of this server, which you can find by writing ping www.google.com in your terminal, or you can simply give www.google.com.

Scapy will automatically translate this address:

```
dest_ip = "www.google.com"
```

Now, we will create an ip_layer packet and print it out to see what it contains:

```
from scapy.all import ICMP
from scapy.all import IP
from scapy.all import sr

if __name__ == "__main__":
    src_ip = "192.168.74.128"
```

Next, to send an ICMP request, you can call the class to create an instance like this:

```
icmp_req = ICMP(id=100)
```

id=100 helps the protocol to trace packets. To see what fields are present inside this request, you can write the following command:

```
print(icmp_req.show())
```

The result will look something like this:

![](img/d31345bbfd15f63be2920f0de5fa30dc_93_0.png)

Figure 4.8 – ICMP packet contents

From here, you can see that the packet type is an echo request, which is used for testing the connection availability.

From our previous discussion, we know that the application layer resides on top of the IP layer, and we have created two layers up until now. Now, the next goal would be to combine these two layers into a single packet that can be sent over the network. To do this, we can write the following code:

```
packet = ip_layer / icmp_req
```

```
print(packet.show())
```

This will list out the combined packet. Note the / operator. This operator is used to combine different layers in Scapy. You start with the lower layer and keep on adding new layers with this / operator. The print result will show the result of the packet with the previous layers combined into one:

![](img/d31345bbfd15f63be2920f0de5fa30dc_94_0.png)

Figure 4.9 – Combined layers

Now, our request is ready to be sent. To send it, we can use the sr1 method we already imported:

```
response = sr1(packet, iface="eth0")
if response:
    print(response.show())
```

The response will look something like this:

![](img/d31345bbfd15f63be2920f0de5fa30dc_95_0.png)

Figure 4.10 – ICMP reply

You can see the type of response is echo-reply and the src field in the reply is the IP address of the server that replied to this ping request.

Now you have learned how to craft and send packets using Python. Theoretically, you can create any network application with Scapy.

The complete code mentioned previously to send a packet is shown next:

```
from scapy.all import ICMP
from scapy.all import IP
from scapy.all import sr

if __name__ == "__main__":
    src_ip = "192.168.74.128"
    dest_ip = "www.google.com"
    ip_layer = IP(src = src_ip, dst = dest_ip)
    icmp_req = ICMP(id=100)
```

```
packet = ip_layer / icmp_req
response = sr(packet, iface="eth0")
// to see available interfaces, write ifconfig command in
// terminal
if response:
    print(response.show())
```

The good thing about Scapy is that it lets you create raw_packets, which means that even packets with false information (malformed packets) can be created and there is no mechanism for checking whether the packet has correct values or not. You can change the src ip field from your computer and put the value of some other packet, and in some cases, the destination will have no way of knowing which PC actually generated these packets (idle scan). This way, you can spoof packets.

So, until now, we have learned about IP stack and header fields. We also learned about how to install Scapy and use it to create raw packets that can be sent over the network. Let's now take a look at few more helpful functions that will help us understand the workings of Scapy in a bit more detail.

If you want to see more details about a certain layer in Scapy and what options are available in the layer to modify, you can use the ls function in Scapy. To import this function, you can use this command:

```
from scapy.all import ls, IP
```

To get information about ip_layer, we can print ls like this:

```
dest_ip = "www.google.com"
ip_layer = IP(dst = dest_ip)
print(ls(ip_layer))
```

In the next screenshot, you will see the execution of the previously mentioned code. The screenshot shows the list of fields in the IP packet:

![](img/d31345bbfd15f63be2920f0de5fa30dc_97_0.png)

Figure 4.11 – The ls function

If you want to access the individual field of any layer, you can simply use the dot (.) operator. For example, if you want to print dst in ip_layer, you can write the following code:

```
ip_layer = IP(dst = dest_ip)
print("Destination = ", ip_layer.dst)
```

The result is as follows:

![](img/d31345bbfd15f63be2920f0de5fa30dc_97_1.png)

Figure 4.12 – Accessing individual fields

If you want to see a quick summary of the layer, you can call the summary method on the layer:

```
print("Summary = ", ip_layer.summary())
```

The summary result will be as follows:

![](img/d31345bbfd15f63be2920f0de5fa30dc_97_2.png)

Figure 4.13 – Layer summary

So now, we have familiarized ourselves with Scapy and how it works. We learned about creating basic packets and how to manipulate these packets. In the next section, we will move toward how to use Scapy for information gathering.

## Network scanner using Scapy

In this section, we will create a simple scanner, scan hosts in our local network, and find their MAC addresses. In order to create the scanner, we need to first understand what the Address Resolution Protocol (ARP) is and how it can be used for creating a network scanner.

### Address Resolution Protocol

ARP in its simplest form is a translation tool that helps us to translate IP addresses into MAC addresses. Whenever a device needs to communicate with a device within the same local network, it needs the device's MAC address. IP addresses are not used for local communication.

Let's say that device A wants to communicate with device B in a local network. In order to find the MAC address of device B, computer A will first look inside an internal list maintained by it called the ARP cache to see whether computer B's IP addresses are mapped to a physical MAC address inside its table. This is called an ARP table as well. You can check the ARP table on your PC by typing the arp -a command.

Here is the result of running the arp -a command on Kali Linux:

```
(venv) —(kali@kali)-[~/packt-book-code/example2-introduction-scapy]
└$ arp -a
? (192.168.74.2) at 00:50:56:ff:74:8b [ether] on eth0
? (192.168.74.254) at 00:50:56:f8:e6:bc [ether] on eth0
```

Figure 4.14 – ARP table

You can see that it lists out the IP addresses and corresponding MAC addresses associated with them. You can use the same command in Windows as well.

If the corresponding MAC address of the requested device is not present locally, device A will send out a broadcast request to the whole network to ask which device has the respective IP. In our case, it will be device B. Those devices that are not device B will ignore this request while device B will give out a reply with the corresponding MAC address of device B. This way, device A will get to know the MAC address of device B. Once both devices get to know each other, the communication between them can follow. Once device A gets the MAC address of device B, it will update its ARP table. Figure 4.15 shows how an ARP request is generated by the source device and how the destination device replies with the correct MAC address:
