in the default configuration) is quite low, and so the profiles are not as accurate as what we'd see from a more intrusive sampling.

There are other displays like this—for threads, I/O, and system events—but for the most part, these displays simply provide nice views into the actual events in the JFR recording.

# Overview of JFR events

JFR produces a stream of events that are saved as a recording. The displays seen so far provide views of those events, but the most powerful way to look at the events is on the Event panel itself, as shown in Figure 3-13.

![](bbox=[0.14, 0.29, 0.85, 0.65])

Figure 3-13. Java Flight Recorder Event panel

The events to display can be filtered in the left panel of this window; here, application-level and JVM-level events are selected. Be aware that when the recording is made, only certain kinds of events are included in the first place: at this point, we are doing postprocessing filtering (the next section shows how to filter the events included in the recording).

Within the 34-second interval in this example, the application produced 878 events from the JVM and 32 events from the JDK libraries, and the event types generated in that period are shown near the bottom of the window. When we looked at this example with profilers, we saw why the thread-park and monitor-wait events for this