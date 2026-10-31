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