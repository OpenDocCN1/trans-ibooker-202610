The following screenshot shows the output of an execution of the example:

![](bbox=[0.28, 0.16, 0.72, 0.34])

You can see how participants arrive, and once the internal counter arrives at 0, the CountDownLatch object wakes up the Videoconference object that writes the messages indicating that the video conference should start.

# There's more...

The CountDownLatch class has another version of the await() method, which is as follows:

+   - await(long time, TimeUnit unit): In this method, the thread will continue to sleep until it's interrupted, that is, either the internal counter of CountDownLatch arrives at 0 or the specified time passes. The TimeUnit class is an enumeration with the following constants: DAYS, HOURS, MICROSECONDS, MILLISECONDS, MINUTES, NANOSECONDS, and SECONDS.

# Synchronizing tasks in a common point

The Java concurrency API provides a synchronizing utility that allows the synchronization of two or more threads at a determined point. It's the CyclicBarrier class. This class is similar to the CountDownLatch class explained in the Waiting for multiple concurrent events recipe in this chapter, but it presents some differences that make it a more powerful class.