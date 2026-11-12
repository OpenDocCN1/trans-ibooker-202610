# Listing 2.42: Viewing effective and real username processes with the ps command

```
$ ps -u Christine -U Christine
PID TTY          TIME CMD
7802 ?        00:00:00 systemd
7803 ?        00:00:00 (sd-pam)
7876 ?        00:00:00 sshd
7877 pts/0    00:00:00 bash
7888 pts/0    00:00:00 ps
$
```

# Viewing Processes with top

The ps command is a great way to get a snapshot of the processes running on the system, but sometimes you need to see more information. For example, if you're trying to find trends about processes that are frequently swapped in and out of memory, it's hard to do that with the ps command.

The top command can solve this problem. It displays process information similar to the ps command, but it does it in real-time mode. Figure 2.2 shows a snapshot of the top command in action.

# FIGURE 2.2 The output of the top command

![](bbox=[0.265, 0.495, 0.722, 0.748])

The first section of the top output shows general system information. The first line shows the current time, how long the system has been up, the number of users logged in, and the load average on the system.

The load average appears as three numbers: the 1-minute, 5-minute, and 15-minute load averages. The higher the values, the more load the system is experiencing. It's not uncommon for the 1-minute load value to be high for short bursts of activity. If the 15-minute load value is high, your system may be in trouble.