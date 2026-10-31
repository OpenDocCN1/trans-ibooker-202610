Fierce uses zone transfer, dictionary attacks, and reverse resolution to gather hostnames and subdomains along with the IP addresses of a domain, and it has the option to search for related names (for example, domain company.com, corpcompany.com, or webcompany.com). In the following example, we will use search to identify hostnames of google.com:

```
root@kali:~# fierce -dns google.com
DNS Servers for google.com:
    ns2.google.com
    ns4.google.com
    ns1.google.com
    ns3.google.com

Trying zone transfer first...
    Testing ns2.google.com
        Request timed out or transfer not allowed.
    Testing ns4.google.com
        Request timed out or transfer not allowed.
    Testing ns1.google.com
        Request timed out or transfer not allowed.
    Testing ns3.google.com
        Request timed out or transfer not allowed.

Unsuccessful in zone transfer (it was worth a shot)
Okay, trying the good old fashioned way... brute force

Checking for wildcard DNS...
Nope. Good.
Now performing 2280 test(s)...
216.58.203.100  academico.google.com
216.58.203.109  accounts.google.com
216.58.203.110  admin.google.com
216.58.203.110  ads.google.com
216.58.203.110  ai.google.com
216.58.203.110  alerts.google.com
216.58.203.100  ap.google.com
216.58.203.110  apps.google.com
216.58.203.100  asia.google.com
216.58.203.110  billing.google.com
216.58.203.105  blog.google.com
216.58.203.110  business.google.com
216.58.203.110  calendar.google.com
216.58.203.110  careers.google.com
216.58.203.110  catalog.google.com
216.58.203.110  chat.google.com
216.58.203.110  classroom.google.com
216.58.203.110  code.google.com
74.125.204.129  corp.google.com
216.58.203.110  d.google.com
216.58.203.110  designer.google.com
216.58.203.110  developer.google.com
216.58.203.110  developers.google.com
```