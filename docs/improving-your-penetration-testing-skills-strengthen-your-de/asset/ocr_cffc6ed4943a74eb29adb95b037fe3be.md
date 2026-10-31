Notice the publisher name shown here; since mshta.exe is a signed Windows application most users will trust it and will allow it to run:

![](bbox=[0.235, 0.175, 0.76, 0.405])

This is why using a custom URI crafted for the victim instead of a random one, can deliver better results.

# Backdooring executables using a MITM attack

In this recipe, you will learn how to backdoor executables using a man-in-the-middle (MITM) attack. When downloading software from online sources, you should always be careful and verify that the software you have downloaded has not been altered by an adversary in transit.

# Getting ready

In this recipe, we will use a MITM framework for MITM attacks to perform an ARP spoofing attack on the Windows 10 target machine, use SSLstrip to transparently hijack HTTP traffic on a network, and map HTTPS links into look-alike HTTP links and then backdoor executables sent over HTTP using the Backdoor Factory.