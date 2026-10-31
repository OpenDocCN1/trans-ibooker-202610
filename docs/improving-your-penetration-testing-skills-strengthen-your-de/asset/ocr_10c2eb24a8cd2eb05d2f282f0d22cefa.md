# Getting ready

Metasploit has a built-in auxiliary module specifically for sweeping SNMP devices. One must understand it before performing an attack. First, read-only and read-write community strings play an important role in the sort of information that can be mined or altered on the devices themselves. The Management Information Base (MIB) interface allows us to query the device and extract information.

![](bbox=[0.14, 0.26, 0.22, 0.34])

If dealing with Windows-based devices configured with SNMP, often at times with the RO/RW community strings, we can extract patch levels, services running, last reboot times, usernames on the system, routes, and various other aspects that worth hack value.

When querying through SNMP, there is the MIB API. This interface allows us to query the device and extract information. Metasploit comes loaded with a list of default MIBs in its database; they are used to query the device for more information, depending on whether the bar of access is obtained.

# How to do it...

+   1. The SNMP Community Login Scanner auxiliary module logs into SNMP devices using common community names:

```
msf > use auxiliary/scanner/snmp/
msf > use auxiliary/scanner/snmp/snmp_login
msf auxiliary(snmp_login) > set RHOSTS 192.168.216.10,129
RHOSTS => 192.168.216.10,129
msf auxiliary(snmp_login) > run

[+] 192.168.216.10:161 - Login Successful: public (Access level: read-only); Proof (sysDescr.0): Hardware: Intel64 Family 6 Model 70 Stepping 1 AT/AT COMPATIBLE - Software: Windows Version 6.1 (Build 7601 Multiproc essor Free)
[*] Scanned 1 of 2 hosts (50% complete)
[*] Scanned 2 of 2 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(snmp_login) >
```