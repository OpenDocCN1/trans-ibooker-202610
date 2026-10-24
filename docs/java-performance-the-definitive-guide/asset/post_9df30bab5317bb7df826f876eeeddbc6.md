
![](img/1711f142ae3cfd740ef106afbbba2046_98_0.png)

Figure 3-15. JFR Start Flight Recording window

Flight recordings are made in one of two modes: either for a fixed duration (1 minute in this case) or continuously. For continuous recordings, a circular buffer is utilized; the buffer will contain the most recent events that are within the desired duration and size.

To perform proactive analysis—meaning that you will start a recording and then generate some work or start a recording during a load-testing experiment after the JVM has warmed up—a fixed-duration recording should be used. That recording will give a good indication of how the JVM responded during the test.

The continuous recording is best for reactive analysis. This lets the JVM keep the most recent events and then dump out a recording in response to an event. For example, the WebLogic application server can trigger that a recording be dumped out in response to an abnormal event in the application server (such as a request that takes more than 5 minutes to process). You can set up your own monitoring tools to dump out the recording in response to any sort of event.

## Enabling JFR via the command line

After enabling JFR (with the `-XX:+FlightRecorder` option), there are different ways to control how and when the actual recording should happen.

In JDK 8, the default recording parameters can be controlled when the JVM starts by using the `-XX:+FlightRecorderOptions=string` parameter; this is most useful for reactive recordings. The `string` in that parameter is a list of comma-separated name-value pairs taken from these options:

- `name=name` — The name used to identify the recording.
- `defaultrecording=<true/false>` — Whether to start the recording initially. The default value is false; for reactive analysis, this should be set to true.
- `settings=path` — Name of the file containing the JFR settings (see the next section).
- `delay=time` — The amount of time (e.g., 30s, 1h) before the recording should start.
- `duration=time` — The amount of time to make the recording.
- `filename=path` — Name of the file to write the recording to.
- `compress=<true/false>` — Whether to compress (with gzip) the recording; the default is false.
- `maxage=time` — Maximum time to keep recorded data in the circular buffer.
- `maxsize=size` — Maximum size (e.g., 1024K, 1M) of the recording’s circular buffer.

`-XX:+FlightRecorderOptions` only sets the defaults for any options; individual recordings can override those settings.

In both JDK 8 and JDK 11, you can start a JFR when the program initially begins by using the `-XX:+StartFlightRecording=string` flag with a similar comma-separated list of options.

Setting up a default recording like that can be useful in some circumstances, but for more flexibility, all options can be controlled with `jcmd` during a run.

To start a flight recording:

```bash
% jcmd process_id JFR.start [options_list]
```

The options_list is a series of comma-separated name-value pairs that control how the recording is made. The possible options are exactly the same as those that can be specified on the command line with the `-XX:+FlightRecorderOptions=string` flag.

If a continuous recording has been enabled, the current data in the circular buffer can be dumped to a file at any time via this command:

```
% jcmd process_id JFR.dump [options_list]
```

The list of options includes the following:

- `name=name` — The name under which the recording was started (see the next example for JFR.check).
- `filename=path` — The location to dump the file to.

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

- `name=name` — The recording name to stop.
- `discard=boolean` — If true, discard the data rather than writing it to the previously provided filename (if any).
- `filename=path` — Write the data to the given path.

In an automated performance-testing system, running these command-line tools and producing a recording is useful when it comes time to examine those runs for regressions.

Templates are managed by the jmc template manager; you may have noticed a button to start the template manager in Figure 3-15. Templates are stored in two locations: under the `$HOME/.jmc/<release>` directory (local to a user) and in the `$JAVA_HOME/jre/lib/jfr` directory (global for a JVM). The template manager allows you to select a global template (the template will say that it is “on server”), select a local template, or define a new template. To define a template, cycle through the available events, and enable or disable them as desired, optionally setting the threshold at which the event kicks in.

Figure 3-16 shows that the File Read event is enabled with a threshold of 15 ms; file reads that take longer than that will cause an event to be triggered. This event has also been configured to generate a stack trace for the File Read events. That increases the overhead—which in turn is why taking a stack trace for events is a configurable option.

![](img/1711f142ae3cfd740ef106afbbba2046_102_0.png)

*Figure 3-16. A sample JFR event template*

The event templates are simple XML files, so the best way to determine which events are enabled in a template (and their thresholds and stack-trace configurations) is to read the XML file. Using an XML file also allows the local template file to be defined on one machine and then copied to the global template directory for use by others on the team.

![](img/1711f142ae3cfd740ef106afbbba2046_103_0.png)

## Quick Summary

- Java Flight Recorder provides the best possible visibility into the JVM, since it is built into the JVM itself.
- Like all tools, JFR introduces some level of overhead into an application. For routine use, JFR can be enabled to gather a substantial amount of information with low overhead.
- JFR is useful in performance analysis, but it is also useful when enabled on a production system so that you can examine the events that led up to a failure.

## Summary

Good tools are key to good performance analysis; in this chapter, we’ve just scratched the surface of what tools can tell us. Here are the key things to keep in mind:

- No tool is perfect, and competing tools have relative strengths. Profiler X may be a good fit for many applications, but in some cases it will miss something that Profiler Y points out quite clearly. Always be flexible in your approach.
- Command-line monitoring tools can gather important data automatically; be sure to include gathering this monitoring data in your automated performance testing.
- Tools rapidly evolve: some of the tools mentioned in this chapter are probably already obsolete (or at least have been superseded by new, superior tools). Keeping up-to-date in this area is important.

## Working with the JIT Compiler

The *just-in-time (JIT)* compiler is the heart of the Java Virtual Machine; nothing controls the performance of your application more than the JIT compiler.

This chapter covers the compiler in depth. It starts with information on how the compiler works and discusses the advantages and disadvantages of using a JIT compiler. Until JDK 8 came along, you had to choose between two Java compilers. Today, those two compilers still exist but work in concert with each other, though in rare cases choosing one is necessary. Finally, we’ll look at some intermediate and advanced tunings of the compiler. If an application is running slowly without any obvious reason, those sections can help you determine whether the compiler is at fault.

## Just-in-Time Compilers: An Overview

We’ll start with some introductory material; feel free to skip ahead if you understand the basics of just-in-time compilation.

Computers—and more specifically CPUs—can execute only a relatively few, specific instructions, which are called *machine code*. All programs that the CPU executes must therefore be translated into these instructions.

Languages like C++ and Fortran are called *compiled languages* because their programs are delivered as binary (compiled) code: the program is written, and then a static compiler produces a binary. The assembly code in that binary is targeted to a particular CPU. Compatible CPUs can execute the same binary: for example, AMD and Intel CPUs share a basic, common set of assembly language instructions, and later versions of CPUs almost always can execute the same set of instructions as previous versions of that CPU. The reverse is not always true; new versions of CPUs often introduce instructions that will not run on older versions of CPUs.
