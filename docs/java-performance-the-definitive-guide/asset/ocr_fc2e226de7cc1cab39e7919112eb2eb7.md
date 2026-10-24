The options_list is a series of comma-separated name-value pairs that control how the recording is made. The possible options are exactly the same as those that can be specified on the command line with the -XX:+FlightRecorderOptions=string flag.

If a continuous recording has been enabled, the current data in the circular buffer can be dumped to a file at any time via this command:

```
% jcmd process_id JFR.dump [options_list]
```

The list of options includes the following:

name=name
The name under which the recording was started (see the next example for JFR.check).

filename=path
The location to dump the file to.

It is possible that multiple JFR recordings have been enabled for a given process. To see the available recordings:

```
% jcmd 21532 JFR.check [verbose]
21532:
Recording 1: name=1 maxsize=250.0MB (running)

Recording 2: name=2 maxsize=250.0MB (running)
```

In this example, process ID 21532 has two active JFR recordings that are named 1 and 2. That name can be used to identify them in other jcmd commands.

Finally, to abort a recording in process:

```
% jcmd process_id JFR.stop [options_list]
```

That command takes the following options:

name=name
The recording name to stop.

discard=boolean
If true, discard the data rather than writing it to the previously provided filename (if any).

filename=path
Write the data to the given path.

In an automated performance-testing system, running these command-line tools and producing a recording is useful when it comes time to examine those runs for regressions.