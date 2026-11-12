# Sending Multiple Jobs to the Background

You can start any number of background jobs from the command-line prompt. Each time you start a new job, the shell assigns it a new job number, and the Linux system assigns it a new PID, as shown in Listing 2.48.

# Listing 2.48: Showing multiple background jobs with the jobs command

```
$ bash CriticalBackups.sh &
[2] 1540
$
$ jobs -l

[1]- 1539 Running      sleep 3000 &
[2]+ 1540 Running      bash CriticalBackups.sh &
$
```

The second program sent to the background is a shell script (shell scripts are covered in Chapter 9) that performs important backups. This may take a while to run, so it is sent to the background and assigned the 2 job number.

In Listing 2.48, notice the plus sign (+) next to the new background job’s number. It denotes the last job added to the background job stack. The minus sign (-) indicates that this particular job is the second-to-last process, which was added to the job stack.

# Bringing Jobs to the Foreground

You don’t have to leave your running programs in the background. If desired, you can return them to foreground mode. To accomplish this, use the fg command and the background job’s number, preceded by a percent sign (%). An example is shown in Listing 2.49.

# Listing 2.49: Bringing a background job to the foreground with the fg command

```
$ jobs -l
[1]- 1539 Running      sleep 3000 &
[2]+ 1540 Running      bash CriticalBackups.sh &
$
$ fg %2
bash CriticalBackups.sh
```

The downside to moving a job back into foreground mode is you now have your terminal session tied up until the program completes.

> **NOTE** You don’t have to run a program in foreground mode to see its output. By default, STDOUT (covered in Chapter 1) is sent to the terminal where a job was put into the background.