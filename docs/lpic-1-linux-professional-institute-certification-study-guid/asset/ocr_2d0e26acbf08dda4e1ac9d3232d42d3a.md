a crucial system file), but often it allows you to peek inside what a script is doing without actually terminating the process.

The Ctrl+Z key combination generates a SIGTSTP signal, stopping any processes running in the shell. Stopping a process is different than terminating the process, as stopping the process leaves the program still in memory and able to continue running from where it left off. In the next section, “Job Control,” you'll learn how to restart a process that's been stopped. When you use the Ctrl+Z key combination, the shell informs you that the process has been stopped:

```
$ sleep 100
```

```
[1]+ Stopped sleep 100
$
```

The number in the square brackets indicates the job number for the process in the shell. If you have a stopped job assigned to your shell session, Bash will warn you if you try to exit the shell:

```
$ exit
logout
There are stopped jobs.
$
```

You can view the stopped job by using the ps command:

```
$ ps au
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
rich 20560 0.0 1.2 2688 1624 pts/0 S 05:15 0:00 -bash
rich 20605 0.2 0.4 1564 552 pts/0 T 05:22 0:00 sleep 100
rich 20606 0.0 0.5 2584 740 pts/0 R 05:22 0:00 ps au
$
```

The ps command shows the status of the stopped job as T, which indicates the command is either being traced or is stopped.

If you really want to exit the shell with the stopped job still active, just type the exit command again. The shell will exit, terminating the stopped job. Alternately, now that you know the PID of the stopped job, you can use the kill command to send a SIGKILL signal to terminate it:

```
$ kill -9 20605
$
[1]+ Killed sleep 100
$
```

When you kill the job, initially you won't get any response. However, the next time you do something that produces a shell prompt, you'll see a message indicating that the job was killed. Each time the shell produces a prompt, it also displays the status of any jobs that have changed states in the shell.