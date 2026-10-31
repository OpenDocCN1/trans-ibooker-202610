# DNSRecon

DNSRecon is another useful tool included in Kali Linux. It lets you gather DNS information through a number of techniques including zone transfer, dictionary requests, and Google search. In the following screenshot, we will do an enumeration by zone transfer (-a), reverse analysis of the IP address space obtained by Whois (-w), and Google search (-g) over zonetransfer.me:

```
root@kali:~# dnsrecon -a -w -g -d zonetransfer.me
[*] Performing General Enumeration of Domain: zonetransfer.me
[*] Checking for Zone Transfer for zonetransfer.me name servers
[*] Resolving SOA Record
[*] SOA nsztml.digi.ninja 81.4.108.41
[*] Resolving NS Records
[*] NS Servers found:
[*] NS nsztml.digi.ninja 81.4.108.41
[*] NS nsztm2.digi.ninja 167.88.42.94
[*] Removing any duplicate NS server IP Addresses...
[*] Trying NS server 167.88.42.94
[-] Zone Transfer Failed for 167.88.42.94!
[*] Port 53 TCP is being filtered
[*] Trying NS server 81.4.108.41
[+] 81.4.108.41 Has port 53 TCP Open
[+] Zone Transfer was successful!!
[*] SOA nsztml.digi.ninja 81.4.108.41
[*] NS nsztml.digi.ninja 81.4.108.41
[*] NS nsztm2.digi.ninja 167.88.42.94
[*] NS intsn1.zonetransfer.me 167.88.42.94
[*] NS intsn2.zonetransfer.me 167.88.42.94
[*] TXT google-site-verification=tyP28J7H4wJHA9fw2sHXMgCC0C16XBmoV104VLMeWxA
[*] TXT Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes
[*] TXT '<script>alert('Boo')</script>'
[*] TXT AbcDeFG
[*] TXT zonetransfer.me service provided by Robin Wood - robin@digi.ninja. See http://digi.ninja/projects/zonetransfer.me.php
[*] TXT ' ; } ; echo Shellshocked
[*] TXT ' or 1=1 --
[*] TXT Robin Wood
[*] PTR www.zonetransfer.me 217.147.177.157
[*] MX @zonetransfer.me ASPMX.L.GOOGLE.COM 74.125.203.27
[*] MX @zonetransfer.me ASPMX.L.GOOGLE.COM 2404:6800:4008:c07::1b
[*] MX @zonetransfer.me ALT1.ASPMX.L.GOOGLE.COM 74.125.198.27
[*] MX @zonetransfer.me ALT1.ASPMX.L.GOOGLE.COM 2607:f8b0:4003:c05::1b
[*] MX @zonetransfer.me ALT2.ASPMX.L.GOOGLE.COM 74.125.201.27
[*] MX @zonetransfer.me ALT2.ASPMX.L.GOOGLE.COM 2607:f8b0:4001:c01::1a
[*] MX @zonetransfer.me ASPMX2.GOOGLEMAIL.COM 74.125.198.26
[*] MX @zonetransfer.me ASPMX2.GOOGLEMAIL.COM 2607:f8b0:4003:c05::1a
[*] MX @zonetransfer.me ASPMX3.GOOGLEMAIL.COM 74.125.201.26
[*] MX @zonetransfer.me ASPMX3.GOOGLEMAIL.COM 2607:f8b0:4001:c01::1a
[*] MX @zonetransfer.me ASPMX4.GOOGLEMAIL.COM 173.194.219.27
[*] MX @zonetransfer.me ASPMX4.GOOGLEMAIL.COM 2607:f8b0:4002:c03::1a
[*] MX @zonetransfer.me ASPMX5.GOOGLEMAIL.COM 74.125.192.26
```