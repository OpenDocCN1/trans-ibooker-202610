are connected to my system (10.0.0.2 and 10.0.0.3) and that they have authenticated with the administrator account:

```
C:\>net session
Computer              User name              Client Type      Opens Idle time
-------------------------------------------------------------------------------
\\10.0.0.2            ADMINISTRATOR                         1 00:00:00
\\10.0.0.3            ADMINISTRATOR                         1 00:01:46
The command completed successfully.
C:\>
```

# tasklist

One of my favorite commands, tasklist, monitors the processes running on the system. When you use the netstat command mentioned earlier with the -o switch, you get the process ID of the program that opened the port. You can use that information with the tasklist command to find out the EXE filename of that process ID. The following is a partial display of the output of tasklist:

```
C:\Users\Administrator>tasklist
Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ===========
System Idle Process              0 Services                 0          24 K
System                           4 Services                 0          64 K
smss.exe                       308 Services                 0         968 K
csrss.exe                      500 Services                 0       4,460 K
wininit.exe                    576 Services                 0       4,924 K
csrss.exe                      584 Console                  1      10,996 K
services.exe                   640 Services                 0       9,376 K
winlogon.exe                   664 Console                  1       6,276 K
```

on the Job

> A few years back, when troubleshooting a system that had been infected with a virus, I had to use the tasklist and taskkill commands to end the process that caused the problem because the Task Manager program had been disabled by the virus.

# taskkill

When monitoring a system, if you notice a process running in memory that may be the cause of a performance or security issue, you can use the taskkill command to end the process. The following shows how you can kill a process by the process ID number or executable filename:

```
C:\Users\Administrator>taskkill /IM notepad.exe /F
SUCCESS: The process "notepad.exe" with PID 3456 has been terminated.
```