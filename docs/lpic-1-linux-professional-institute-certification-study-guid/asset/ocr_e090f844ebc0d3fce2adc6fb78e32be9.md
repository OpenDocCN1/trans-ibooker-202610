```
Loop #4
Loop #5
Loop #6
Loop #7
Loop #8
Loop #9
Loop #10
This is the end of the test program
$
```

The output appears in the nohup.out file just as if the process ran on the command line!

If you run another command using nohup, the output is appended to the existing nohup.out file. Be careful when running multiple commands from the same directory, as all of the output will be sent to the same nohup.out file, which can get confusing.

# Sending Signals

The Bash shell can send control signals to processes running on the system. This allows you to stop or interrupt a runaway application process if necessary. There are two basic Linux signals you can generate using key combinations on the keyboard to interrupt or stop a foreground process.

# Interrupting a Process

The Ctrl+C key combination generates a SIGINT signal and sends it to any processes currently running in the shell. The SIGINT signal interrupts the running process, which for most processes causes them to stop (it is possible to code a script to ignore interrupt signals, but this is not common). You can test this by running a command that normally takes a long time to finish and pressing the Ctrl+C key combination:

```
$ sleep 100
$
```

The Ctrl+C key combination doesn't produce any output on the monitor; it just stops the current process running in the shell.

# Pausing a Process

Instead of terminating a process, you can pause it in the middle of whatever it's doing. Sometimes this can be a dangerous thing (for example, if a script has a file lock open on