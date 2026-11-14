
PS > `[ADSI] "WinNT://./Administrator"` | `Format-List *`

UserFlags                  : {66113}
MaxStorage                 : {-1}
PasswordAge                : {19550795}
PasswordExpired            : {0}
LoginHours                 : {255 255 255 255 255 255 255 255 255 255 255
                             255 255 255 255 255 255 255 255 255 255}
FullName                   : {}
Description                : {Built-in account for administering the computer/domain}
BadPasswordAttempts        : {0}
LastLogin                  : {5/21/2007 3:00:00 AM}
HomeDirectory              : {}
LoginScript                : {}
Profile                    : {}
HomeDirDrive               : {}
Parameters                 : {}
PrimaryGroupID             : {513}
Name                       : {Administrator}
MinPasswordLength          : {0}
MaxPasswordAge             : {3710851}
MinPasswordAge             : {0}
PasswordHistoryLength      : {0}
AutoUnlockInterval         : {1800}
LockoutObservationInterval : {1800}
MaxBadPasswordsAllowed     : {0}
RasPermissions             : {1}
objectSid                  : {1 5 0 0 0 0 0 5 21 0 0 0 121 227 252 83 122
                             130 50 34 67 23 10 50 244 1 0 0}

Or, as [Example P-7](#preface02.xhtml#working_with_com_objects_in_powershell) shows, you can even use PowerShell for scripting traditional COM objects.

### Example P-7. Working with COM objects in PowerShell

```
PS > $firewall = New-Object -com HNetCfg.FwMgr
PS > $firewall.LocalPolicy.CurrentProfile

Type                                         : 1
FirewallEnabled                              : True
ExceptionsNotAllowed                         : False
NotificationsDisabled                        : False
UnicastResponsesToMulticastBroadcastDisabled : False
RemoteAdminSettings                          : System.__ComObject
IcmpSettings                                 : System.__ComObject
GloballyOpenPorts                            : {Media Center Extender Service,
                                               Remote Media Center Experience,
                                               Adam Test Instance, QWAVE...}
Services                                     : {File and Printer Sharing,
                                               UPnP Framework, Remote Desktop}
AuthorizedApplications                       : {Remote Assistance, Windows
                                               Messenger, Media Center, Trillian...}
```

# Namespace Navigation Through Providers

Another avenue PowerShell offers for working with the system is *providers*. PowerShell providers let you navigate and manage data stores using the same techniques you already use to work with the filesystem, as illustrated in [Example P-8](#preface02.xhtml#navigating_the_filesystem).

### Example P-8. Navigating the filesystem

```
PS > Set-Location c:\
PS > Get-ChildItem

    Directory: C:\

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----           5/8/2007   8:37 PM                Blurpark
d----           5/15/2016  4:32 PM                Chocolatey
d----            3/8/2020 12:45 PM                DXLab
d----           4/30/2020  7:00 AM                Go
d----            4/2/2016  3:05 PM                Intel
d-r--          12/15/2020  1:41 PM                Program Files
d-r--          11/28/2020  5:06 PM                Program Files (x86)
d----           5/12/2019  6:37 PM                Python27
d----           3/25/2018  1:11 PM                Strawberry
d----          12/16/2020  8:13 AM                temp
d-r--           8/11/2020  5:02 PM                Users
da---          12/16/2020 10:51 AM                Windows
```

This also works on the registry, as shown in [Example P-9](#preface02.xhtml#navigating_the_registry).

### Example P-9. Navigating the registry

```
PS > Set-Location HKCU:\Software\Microsoft\Windows\
PS > Get-ChildItem

    Hive: HKEY_CURRENT_USER\Software\Microsoft\Windows

Name                           Property
----                           --------
CurrentVersion
DWM                            Composition                  : 1
                               ColorPrevalence              : 0
                               ColorizationColor            : 3290322719
                               EnableAeroPeek               : 1
                               AccentColor                  : 4280243998
                               EnableWindowColorization     : 1
Shell
TabletPC
Windows Error Reporting

PS > Set-Location CurrentVersion\Run
PS > Get-ItemProperty .

(...)
OneDrive            : "C:\Users\lee\AppData\Local\Microsoft\OneDrive\OneDrive.exe"
    /background
OpenDNS Updater     : "C:\Program Files (x86)\OpenDNS Updater\OpenDNSUpdater.exe"
    /autostart
Ditto               : C:\Program Files\Ditto\Ditto.exe
(...)
```

And it even works on the machine's certificate store, as [Example P-10](#preface02.xhtml#navigating_the_certificate_store) illustrates.

### Example P-10. Navigating the certificate store

```
PS > Set-Location cert:\CurrentUser\Root
PS > Get-ChildItem

    Directory: Microsoft.PowerShell.Security\Certificate::CurrentUser\Root

Thumbprint                                Subject
----------                                -------
CDD4EEAE6000AC7F40C3802C171E30148030C072  CN=Microsoft Root Certificate...
BE36A4562FB2EE05DBB3D32323ADF445084ED656  CN=Thawte Timestamping CA, OU...
A43489159A520F0D93D032CCAF37E7FE20A8B419  CN=Microsoft Root Authority, ...
9FE47B4D05D46E8066BAB1D1BFC9E48F1DBE6B26  CN=PowerShell Local Certifica...
7F88CD7223F3C813818C994614A89C99FA3B5247  CN=Microsoft Authenticode(tm)...
245C97DF7514E7CF2DF8BE72AE957B9E04741E85  OU=Copyright (c) 1997 Microso...
(...)
```

# Much, Much More

As exciting as this guided tour was, it barely scratches the surface of how you can use PowerShell to improve your productivity and systems management skills. For more information about getting started in PowerShell, see [Chapter 1](#ch01.xhtml#the_windows_powershell_interactive_shell).

# Part II. Fundamentals

- [Chapter 1, *The PowerShell Interactive Shell*](#ch01.xhtml#the_windows_powershell_interactive_shell)
- [Chapter 2, *Pipelines*](#ch02.xhtml#pipelines)
- [Chapter 3, *Variables and Objects*](#ch03.xhtml#variables_and_objects)
- [Chapter 4, *Looping and Flow Control*](#ch04.xhtml#looping_and_flow_control)
- [Chapter 5, *Strings and Unstructured Text*](#ch05.xhtml#strings_and_unstructured_text)
- [Chapter 6, *Calculations and Math*](#ch06.xhtml#calculations_and_math)
- [Chapter 7, *Lists, Arrays, and Hashtables*](#ch07.xhtml#lists_arrays_and_hashtables)
- [Chapter 8, *Utility Tasks*](#ch08.xhtml#utility_scripting)

# Chapter 1. The PowerShell Interactive Shell

## 1.0 Introduction

Above all else, the design of PowerShell places priority on its use as an efficient and powerful interactive shell. Even its scripting language plays a critical role in this effort, as it too heavily favors interactive use.

What surprises most people when they first launch PowerShell is its similarity to the command prompt that has long existed as part of Windows. Familiar tools continue to run. Familiar commands continue to run. Even familiar hotkeys are the same. Supporting this familiar UI, though, is a powerful engine that lets you accomplish once cumbersome administrative and scripting tasks with ease.

This chapter introduces PowerShell from the perspective of its interactive shell.

## 1.1 Install PowerShell Core

### Problem

You want to install the most recent version of PowerShell on your Windows, Mac, or Linux system.

### Solution

Visit the [Microsoft website](https://microsoft.com/PowerShell) to find the installation instructions for the operating system and platform you want to install on. For the most common:

#### Windows
