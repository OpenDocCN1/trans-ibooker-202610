
## DNSRecon

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

## Brute force DNS records using Nmap

Nmap comes with a script to query the DNS server for additional hosts using a brute forcing technique. It makes use of the vhosts-defaults.lst and vhosts-full.lst dictionary files, which contain a large list of common hostnames that have been collected over the years by the Nmap development team. The files can be located at /usr/share/nmap/nselib/data/. Nmap sends a query to the DNS server for each entry in that file to check whether there are any A records available for that hostname in the DNS zone.

As shown in the following screenshot, the brute force script returned a positive result. It identified a few hosts in the DNS zone by querying for their A records:

![](img/6b6e557533100c0e47951d0e5e14c4ee_106_0.png)

## Using search engines and public sites to gather information

Modern search engines are a valuable resource for public information gathering and passive reconnaissance. Generalist engines such as Google, Bing, and DuckDuckGo allow us to use advanced search filters to look for information in a particular domain, certain file types, content in URLs, and specific text patterns. There are also specialized search engines, such as Shodan, that let you search for hostnames, open ports, server location, and specific response headers in a multitude of services.

## Google dorks

The **Google dorks** technique, also known as *Google hacking*, started as an abuse of Google's advanced search options, and it was later extended to other search engines that also included similar options. It searches for specific strings and parameters to get valuable information from an organization or target. Here are some examples that can be useful for a penetration tester:

- PDF documents in a specific site or domain can be searched for, like this:
  ```
  site:example.com filetype:pdf
  ```
- References to email addresses of a specific domain, excluding the domain's site can be searched for:
  ```
  "@example.com" -site:example.com
  ```
- Administrative sites with the word admin in the title or the URL in example.com can be searched for:
  ```
  intitle:admin OR inurl:admin site:example.com
  ```
- You can also look for a specific error message indicating a possible SQL injection vulnerability:
  ```
  "SQL Server Driver][SQL Server]Line 1: Incorrect syntax near"
  site:example.com
  ```

There are thousands of possible useful search combinations in Google and other search engines. Offensive Security, the creators of Kali Linux, also maintain a public database for search strings that may yield useful results for a penetration tester, which is available at: https://www.exploit-db.com/google-hacking-database/.

## Shodan

Shodan (https://shodan.io) is a different kind of search engine; it helps you to look for devices connected to the internet instead of content in web pages. Like Google, it has operators and a specific syntax to execute advanced and specific searches. This screenshot shows a search for all hostnames related to google.com:

![](img/6b6e557533100c0e47951d0e5e14c4ee_108_0.png)

A hostname search example using Shodan

To take advantage of Shodan's advanced search features, one needs to first create an account. Free accounts yield a limited number of results, and some options are restricted though still very useful. Shodan can be used to find the following:

- Servers exposed to the internet belonging to some domain can be found like this:
  ```
  hostname:example.com
  ```
- Specific types of devices, such as CCTV cameras or **Industrial Control Systems (ICS)**, can be found by specifying the Server parameter:
  ```
  Server: SQ-WEBCAM
  ```
- Specific open ports or services can be found, for example, web servers using common ports:
  ```
  port:80,443,8080
  ```
- Hosts in a specific network range can be found like this:
  ```
  net:192.168.1.1/24
  ```

A useful reference on Shodan search options and operators can be found at:
https://pen-testing.sans.org/blog/2015/12/08/effective-shodan-searches.

## theHarvester

**theHarvester** is a command-line tool included in Kali Linux that acts as a wrapper for a variety of search engines and is used to find email accounts, subdomain names, virtual hosts, open ports / banners, and employee names related to a domain from different public sources (such as search engines and PGP key servers). In recent versions, the authors added the capability of doing DNS brute force, reverse IP resolution, and **Top-Level Domain (TLD)** expansion.

In the following example, theharvester is used to gather information about zonetransfer.me:

```
root@kali:~# theharvester -b all -d zonetransfer.me

**************************************************
*                                                *
*   TheHarvester                                *
*                                                *
*   TheHarvester Ver. 2.7                       *
*   Coded by Christian Martorella               *
*   Edge-Security Research                      *
*   cmartorella@edge-security.com               *
**************************************************

Full harvest..
[-] Searching in Google..
        Searching 0 results...
        Searching 100 results...
[-] Searching in PGP Key server..
[-] Searching in Bing..
        Searching 50 results...
        Searching 100 results...
[-] Searching in Exalead..
        Searching 50 results...
        Searching 100 results...
        Searching 150 results...

[+] Emails found:

pippa@zonetransfer.me
pixel-1506786993611511-web-@zonetransfer.me
pixel-1506786996891728-web-@zonetransfer.me
xss.zonetransfer.me@xss.zonetransfer.me

[+] Hosts found in search engines:
----------------------------------------
[-] Resolving hostnames IPs...
127.0.0.1:asfdbbox.zonetransfer.me
4.23.39.254:office.zonetransfer.me
207.46.197.32:owa.zonetransfer.me
54.206.51.177:staging.zonetransfer.me
217.147.177.157:testing.zonetransfer.me
217.147.177.157:www.zonetransfer.me
[+] Virtual hosts:
```
