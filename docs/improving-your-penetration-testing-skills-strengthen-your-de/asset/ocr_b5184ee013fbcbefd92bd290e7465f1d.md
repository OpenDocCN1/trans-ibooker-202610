Shell commands, such as `grep` or `cut`, are very useful for processing the output of command-line tools. In the preceding example, `cut` is used with a `|` (pipe) character to show only the first three elements that are separated by a `-d " "` (space) character from each line of the `dig` command's results. In this screenshot, the columns are separated by tab characters and information shown in the last column is separated by spaces.

You will often find that even though the primary DNS server blocks the zone transfer, a secondary server for that domain might allow it. The `dig google.com NS +noall +answer` command will display all of the name servers for that domain.

The attempt to perform a zone transfer from the DNS server of `facebook.com` failed, as the company have correctly locked down their DNS servers:

```
; <<>> DiG 9.10.3-P4-Ubuntu <<>> axfr facebook.com @A.NS.FACEBOOK.COM
;; global options: +cmd
; Transfer failed.
```

Performing a DNS lookup to search for an IP address is passive reconnaissance. However, the moment you do a zone transfer using a tool such as `dig` or `nslookup`, it turns into active reconnaissance.

# DNS enumeration

Finding a misconfigured server that allows anonymous zone transfers is very uncommon on real penetration testing projects. There are other techniques that can be used to discover hostnames or subdomains related to a domain, and Kali Linux includes a couple of useful tools to do just that.