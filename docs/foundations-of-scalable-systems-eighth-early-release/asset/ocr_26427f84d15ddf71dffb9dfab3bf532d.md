+   - The thread priority, which by default is 5 for all threads. We’ll cover thread priorities later.
+   - The identifier of the parent thread – in this example both parent threads are the main thread

Note to instantiate a thread, we call the start() method, not the run() method we define in the Runnable. The start() method contains the internal system magic to create the execution context for a separate thread to execute. If we call run() directly, the code will execute, but no new thread will be created. The run() method will execute as part of the main thread, just like any other Java method invocation that you know and love. You will still have a single threaded code.

In the example, we use sleep() to pause the execution of the main thread and make sure it does not terminate before the NamingThread. This approach, namely coordinating two threads by delaying for an absolute time period (e.g. 1 second in the example) is not a very robust mechanism. What if for some reason - a slower CPU, a long delay reading disk, additional complex logic in the method – our thread doesn’t terminate in the expected time frame? In this case, main will terminate first – this is not what we intend. In general, if you are using absolute times for thread coordination, you are doing it wrong. Almost always. Like 99.99999% of the time.

A simple and robust mechanism for one thread to wait until another has completed its work is to use the join() method. We could replace the try-catch block in the above example with:

```
t0.join();
```

This method causes the calling thread (in this case, main) to block until the thread referenced by t0 terminates. If the referenced thread has terminated before the call to join(), then the method call returns immediately. In this way we can coordinate, or synchronize, the behavior of multiple threads. Synchronization of multiple threads is in fact the major focus of the rest of this chapter.