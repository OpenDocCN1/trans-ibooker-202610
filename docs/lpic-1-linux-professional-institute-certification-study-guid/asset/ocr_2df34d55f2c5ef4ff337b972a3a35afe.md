# # Running Multiple Background Jobs

You can start any number of background jobs at the same time from the command-line prompt:

```
```
$ ./test18.sh &
[1] 19582
$ This is test program
Loop #1
$ ./test18.sh &
[2] 19597
$ This is test program
Loop #1
$ ./test18.sh &
[3] 19612
$ This is test program
Loop #1
Loop #2
Loop #2
Loop #2
```
```

Each time you start a new job, the shell assigns it a new job number, and the Linux system assigns it a new PID. You can see that all of the scripts are running by using the ps command:

```
```
$ ps au
USER    PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
rich   19498  0.0  1.2 2688 1628 pts/0 S    11:38   0:00 -bash
rich   19582  0.0  0.9 2276 1180 pts/0 S    11:55   0:00 /bin/bash ./test18.sh
rich   9597   0.1  0.9 2276 1180 pts/0 S    11:55   0:00 /bin/bash ./test18.sh
rich   19612  0.1  0.9 2276 1180 pts/0 S    11:55   0:00 /bin/bash ./test18.sh
rich   19639  0.0  0.4 1564  552 pts/0 S    11:56   0:00 sleep 10
rich   19640  0.0  0.4 1564  552 pts/0 S    11:56   0:00 sleep 10
rich   19641  0.0  0.4 1564  552 pts/0 S    11:56   0:00 sleep 10
rich   19642  0.0  0.5 2588  744 pts/0 R    11:56   0:00 ps au
$
```
```

Each of the background processes you start appears in the ps command output listing of running processes. If all of the processes display output in your terminal session, things can get pretty messy pretty quickly. Fortunately, there's a simple way to solve that problem, which we'll discuss in the next section.