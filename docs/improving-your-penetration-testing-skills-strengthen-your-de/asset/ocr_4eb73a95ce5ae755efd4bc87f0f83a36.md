2. To run the WinRM Command Runner auxiliary module, we need to set the targets IP address, the Windows command to run, the username Administrator, and password vagrant:

```
msf > use auxiliary/scanner/winrm/winrm_cmd
msf auxiliary(winrm_cmd) > set CMD hostname
CMD => hostname
msf auxiliary(winrm_cmd) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(winrm_cmd) > set USERNAME Administrator
USERNAME => Administrator
msf auxiliary(winrm_cmd) > set PASSWORD vagrant
PASSWORD => vagrant
msf auxiliary(winrm_cmd) > run

[+] vagrant-2008R2

[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(winrm_cmd) >
```

Looking at the output of the module, we can see that we can run remote commands on the target machine.

# Integrating with Nessus

So far, we have learned the basics of port scanning, along with the practical implementation with Nmap. Port scanning has been extended to several other tools which further enhances the process of scanning and information gathering. In the next few recipes, we will cover those tools which scan the target for available services and open ports and then try to determine the type of vulnerability that may exist for that particular service or port. Let's begin our journey to vulnerability scanning.

Nessus is one of the most widely used vulnerability scanners. It scans the target for a range of vulnerabilities and produces a detailed report for it. Nessus is a very helpful tool to use for penetration testing. Either you can use the GUI version of Nessus, or you can use it from the Metasploit console. In this book, we will primarily focus on using Nessus with msfconsole.