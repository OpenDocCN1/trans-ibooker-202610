| USER | TTY | FROM | LOGIN@ | IDLE | JCPU | PCPU | WHAT |
|---|---|---|---|---|---|---|---|
| Christin | pts/0 | 192.168.0.101 | 16:14 | 2.00s | 0.14s | 0.00s | screen |
| Christin | pts/5 | :pts/0:S.0 | 17:09 | 2.00s | 0.07s | 0.00s | w |
| $ | | | | | | | |

Notice the output from the screen -ls command displays an Attached window. The ID number in this case is 9150 (which also happens to be its PID). The w command's output shows two logged-in users; one is using the screen command, and the other is using the w command. The first user issued the screen command that created a second user process. The FROM column for the second user's process shows that this user was employing the pts/0 terminal and is now residing in screen window 0 (s.0) on the pts/5 terminal.

> A pts terminal is a pseudo-terminal. The /# after pts indicates which pseudo-terminal the user is employing. For example, pts/5 means that pseudo-terminal #5 is in use. You'll often see these terminal types when using a terminal emulator within a GUI, but they may also be used when using OpenSSH to reach and log into a Linux system, as is the case in this section.

The neat thing about the screen window is that you can issue a command within the window, detach from the window, and come back to it later, causing no ill effects on the command running in that window. To detach from a screen window, press Ctrl+A and then the D key. The screen -ls command will then display your detached window session. An example is shown in Listing 2.44.

# Listing 2.44: Displaying a detached screen window with the screen -ls command

```
[detached from 9394.pts-0.Ubuntu1804]
$
$ screen -ls

There is a screen on:
        9394.pts-0.Ubuntu1804      (04/16/2019 05:42:45 PM)   (Detached)
1 Socket in /run/screen/S-Christine.
$
```

To reattach to the screen, you need to employ the screen -r screen-id command. Using the window screen in Listing 2.44, you would type screen -r 9394 at the command line.

You can also split a window screen up into multiple windows, called focuses. To do this and to control the screen window(s), press the Ctrl+A key combination (called a prefix shortcut) and follow it with an additional key or key combination. A few of these additional key or key combinations are shown in Table 2.10.