For that reason, most profilers will not report methods that are blocked; those threads are shown as being idle. In this particular example, that is a good thing. Threads wait in the `select()` method because no data is flowing into the server; they are not being inefficient. That is their normal state.

In other cases, you do want to see the time spent in those blocking calls. The time that a thread spends inside the `wait()` method—waiting for another thread to notify it—is a significant determinant of the overall execution time of many applications. Most Java-based profilers have filter sets and other options that can be tweaked to show or hide these blocking calls.

Alternately, it is usually more fruitful to examine the execution patterns of threads rather than the amount of time a profiler attributes to the blocking method itself. **Figure 3-7** shows a thread display from the Oracle Developer Studio profiling tool.

![](bbox=[0.143, 0.338, 0.858, 0.559])

*Figure 3-7. A thread timeline profile*

Each horizontal area here is a different thread (so the figure shows nine threads: thread 1.14 to thread 1.22). The colored (or different grayscale) bars represent execution of different methods; blank areas represent places where the thread is not executing. At a high level, observe that thread 1.14 executed code and then waited for something.

Notice too the blank areas where no thread appears to be executing. The image shows only nine of many threads in the application, so it is possible that these threads are waiting for one of those other threads to do something, or the thread could be executing a blocking `read()` (or similar) call.