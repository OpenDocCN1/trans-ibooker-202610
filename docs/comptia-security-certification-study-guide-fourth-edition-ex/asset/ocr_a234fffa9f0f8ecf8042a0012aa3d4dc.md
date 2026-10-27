# FIGURE 20-6

Using nmap to perform a port scan on a system

```
bt ~ # nmap -sT 10.0.0.1
Starting Nmap 4.60 ( http://nmap.org ) at 2011-03-22 19:30 GMT
Interesting ports on 10.0.0.1:
Not shown: 1698 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
88/tcp   open  kerberos-sec
110/tcp  open  pop3
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
636/tcp  open  ldapssl
1025/tcp open  NFS-or-IIS
1026/tcp open  LSA-or-nterm
3268/tcp open  globalcatLDAP
3269/tcp open  globalcatLDAPssl
MAC Address: 00:0C:29:4E:76:03 (VMware)
Nmap done: 1 IP address (1 host up) scanned in 15.442 seconds
bt ~ #
```

# FIGURE 20-7

Using nmap to find out the version of the software running on a port

```
bt ~ # nmap -sV 10.0.0.1
Starting Nmap 4.60 ( http://nmap.org ) at 2011-03-22 19:33 GMT
Interesting ports on 10.0.0.1:
Not shown: 1697 closed ports
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           Microsoft ftpd
25/tcp   open  smtp          Microsoft ESMTP 6.0.3790.0
53/tcp   open  domain        Microsoft DNS
80/tcp   open  http          Microsoft IIS-webserver 6.0
88/tcp   open  kerberos-sec  Microsoft Windows kerberos-sec
110/tcp  open  pop3          Microsoft Windows 2003 POP3 Service 1.0
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn
389/tcp  open  ldap          Microsoft LDAP server
445/tcp  open  microsoft-ds  Microsoft Windows 2003 microsoft-ds
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft LDAP server
1025/tcp open  msrpc         Microsoft Windows RPC
1026/tcp open  msrpc         Microsoft Windows RPC
3268/tcp open  ldap          Microsoft LDAP server
3269/tcp open  ssl/ldap      Microsoft LDAP server
3389/tcp open  ms-term-serv?
MAC Address: 00:0C:29:4E:76:03 (VMware)
Service Info: Host: 2003SERVERA.certworld.loc; OSs: Windows, Windows 2000
```