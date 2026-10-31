Looking at the output, we were able to enumerate the Jenkins version, host operating system, and installation details.

# WinRM scanning and brute forcing

Windows Remote Management (WinRM) is the Microsoft implementation of the WS-Management Protocol, a standard Simple Object Access Protocol (SOAP)-based, firewall-friendly protocol that allows hardware and operating systems, from different vendors, to interoperate.

# Getting ready

The WinRM Authentication Method Detection auxiliary module sends a request to an HTTP/HTTPS service to see if it is a WinRM service. If it is a WinRM service, it also gathers the authentication methods supported.

Now that we know that the target system has WinRM enabled, we can start scanning to see if we can leverage WinRM and compromise the system.

Using the credentials found with the SMB Login Check Scanner auxiliary module, we can test if we can run Windows commands using the WinRM service, using the WinRM Command Runner auxiliary module.

# How to do it...

+   1. To use the WinRM Authentication Method Detection auxiliary module, set the target address range in RHOSTS and type run:

```
msf > use auxiliary/scanner/winrm/winrm_auth_methods
msf auxiliary(winrm_auth_methods) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(winrm_auth_methods) > run

[+] 192.168.216.10:5985: Negotiate protocol supported
[+] 192.168.216.10:5985: Basic protocol supported
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(winrm_auth_methods) >
```