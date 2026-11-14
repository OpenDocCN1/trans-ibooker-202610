
To clear a classic event log entirely, use the `Clear-EventLog` cmdlet. For modern event logs, use the `cl` parameter of `wevtutil.exe`. If you want to save the contents of the event log before clearing it, see [Recipe 23.6](#ch23.xhtml#back_up_an_event_log).

If you want to remove an event log entirely, see [Recipe 23.7](#ch23.xhtml#create_or_remove_an_event_log).

## See Also

- [Recipe 3.8, “Work with .NET Objects”](#ch03.xhtml#work_with_net_objects)
- [Recipe 23.6, “Back Up an Event Log”](#ch23.xhtml#back_up_an_event_log)
- [Recipe 23.7, “Create or Remove an Event Log”](#ch23.xhtml#create_or_remove_an_event_log)

## 23.11 Access Event Logs of a Remote Machine

### Problem

You want to access event log entries from a remote machine.

### Solution

To access event logs on a remote machine, use the `-ComputerName` parameter of any of the `EventLog` cmdlets:

```
PS > Get-WinEvent System -ComputerName LEE-DESK | Group-Object Source

Count Name                      Group
----- ----                      -----
   91 Print                     {LEE-DESK, LEE-DESK, LEE-DESK, LEE-DESK...
  640 TermServDevices           {LEE-DESK, LEE-DESK, LEE-DESK, LEE-DESK...
  148 W32Time                   {LEE-DESK, LEE-DESK, LEE-DESK, LEE-DESK...
  100 WMPNetworkSvc             {LEE-DESK, LEE-DESK, LEE-DESK, LEE-DESK...
  856 Service Control Manager   {LEE-DESK, LEE-DESK, LEE-DESK, LEE-DESK...
  123 Tcpip                     {LEE-DESK, LEE-DESK, LEE-DESK, LEE-DESK...
(...)
```

To use the graphical event log viewer to browse event logs on a remote machine, use the `Show-EventLog` cmdlet:

```
Show-EventLog Computername
```

### Discussion

The `-ComputerName` parameter of the `*-EventLog` cmdlets makes it easy to manage event logs of remote computers. Using these cmdlets, you can create event logs, remove event logs, write event log entries, and more.

If you want to use a graphical user interface to work with event logs on a remote machine in a more ad hoc way, use the `Show-EventLog` cmdlet. If the Remote Eventlog Management firewall rule is enabled on the remote computer (and you have the appropriate permissions), PowerShell launches the Windows Event Viewer targeted to that machine (see [Figure 23-1](#ch23.xhtml#event_viewer_remote_machine)).

By default, the Windows Event Viewer tries to use the credentials of your current account to connect to the remote computer. If you need to connect as another account, click the “Connect to Another Computer” action on the righthand side of the Event Viewer window that opens. In that window, specify both the remote computer name and new user information.

![](Images/wps4_2301.png)

For information about how to get event logs, see [Recipe 23.1](#ch23.xhtml#list_all_event_logs). For more information about how to create or delete event logs, see [Recipe 23.7](#ch23.xhtml#create_or_remove_an_event_log). For more information about how to write event log entries, see [Recipe 23.8](#ch23.xhtml#write_to_an_event_log).

### See Also

- [Recipe 23.1, “List All Event Logs”](#ch23.xhtml#list_all_event_logs)
- [Recipe 23.7, “Create or Remove an Event Log”](#ch23.xhtml#create_or_remove_an_event_log)
- [Recipe 23.8, “Write to an Event Log”](#ch23.xhtml#write_to_an_event_log)

# Chapter 24. Processes

## 24.0 Introduction

Working with system processes is a natural aspect of system administration. It’s also the source of most of the regular expression magic that make system administrators proud. After all, who wouldn’t boast about this Unix one-liner to stop all processes using more than 100 MB of memory:

```
ps -el | awk '{ if ( $6 > (1024*100)) { print $3 } }' | grep -v PID | xargs kill
```

While helpful, it also demonstrates the inherently fragile nature of pure text processing. For this command to succeed, it must:

- Depend on the `ps` command to display memory usage in column 6
- Depend on column 6 of the `ps` command’s output to represent the memory usage in kilobytes
- Depend on column 3 of the `ps` command’s output to represent the process ID
- Remove the header column from the `ps` command’s output

While the `ps` command has parameters that simplify some of this work, this form of “prayer-based parsing” is common when manipulating the output of tools that produce only text.

Since PowerShell’s `Get-Process` cmdlet returns information as highly structured .NET objects, fragile text parsing becomes a thing of the past:

```
Get-Process | Where-Object { $_.WorkingSet -gt 100mb } | Stop-Process -WhatIf
```

If brevity is important, PowerShell defines aliases to make most commands easier to type:

```
gps | ? WS -gt 100mb | kill -WhatIf
```

In addition to simple process control, PowerShell also offers commands for starting processes, customizing their execution environment, waiting for processes to exit, and more.

## 24.1 List Currently Running Processes

### Problem

You want to see which processes are running on the system.

### Solution

To retrieve the list of currently running processes, use the `Get-Process` cmdlet:

```
PS > Get-Process

Handles  NPM(K)  PM(K)  WS(K)  VM(M)  CPU(s)     Id ProcessName
-------  ------  -----  -----  -----  ------     -- -----------
    274       6   1328   3940     33           1084 alg
     85       4   3816   6656     57    5.67   3460 AutoHotkey
     50       2   2292   1980     14  384.25   1560 BrmfRsmg
     71       3   2520   4680     35    0.42   2592 cmd
    946       7   3676   6204     32            848 csrss
     84       4    732   2248     22           3144 csrss
     68       4    936   3364     30    0.38   3904 ctfmon
    243       7   3648   9324     48    2.02   2892 Ditto
(...)
```

### Discussion

The `Get-Process` cmdlet retrieves information about all processes running on the system. Because these are rich .NET objects (of the type `System.Diagnostics.Process`), advanced filters and operations are easier than ever before.

For example, to find all processes using more than 100 MB of memory:

```
PS > Get-Process | Where-Object { $_.WorkingSet -gt 100mb }

Handles  NPM(K)  PM(K)  WS(K)  VM(M)  CPU(s)    Id ProcessName
-------  ------  -----  -----  -----  ------    -- -----------
   1458      29  83468 105824    273  323.80  3992 BigBloatedApp
```

To group processes by company:

```
PS > Get-Process | Group-Object Company

Count Name                      Group
----- ----                      -----
   39                           {alg, csrss, csrss, dllhost...}
    4                           {AutoHotkey, Ditto, gnuserv, mafwTray}
    1 Brother Industries, Ltd.  {BrmfRsmg}
   19 Microsoft Corporation     {cmd, ctfmon, EXCEL, explorer...}
    1 Free Software Foundation  {emacs}
    1 Microsoft (R) Corporation {FwcMgmt}
(...)
```

Or perhaps to sort by start time (with the most recent first):

```
PS > Get-Process | Sort-Object -Descending StartTime | Select-Object -First 10

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
   1810      39    53616      33964   193   318.02   1452 iTunes
    675       6    41472      50180   146    49.36    296 powershell
   1240      35    48220      58860   316   167.58   4012 OUTLOOK
    305       8     5736       2460   105    21.22   3384 WindowsSearch...
    464       7    29704      30920   153     6.00   3680 powershell
   1458      29    83468     105824   273   324.22   3992 iexplore
    478       6    24620      23688   143    17.83   3548 powershell
    222       8     8532      19084   144    20.69   3924 EXCEL
     14       2      396       1600    15     0.06   2900 logon.scr
    544      18    21336      50216   294   180.72   2660 WINWORD
```
