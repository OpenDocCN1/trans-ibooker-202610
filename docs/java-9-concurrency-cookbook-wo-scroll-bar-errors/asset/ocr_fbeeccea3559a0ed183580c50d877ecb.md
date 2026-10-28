One critical aspect of the ThreadPoolExecutor class, and of executors in general, is that you have to end them explicitly. If you don't do this, the executor will continue its execution and the program won't end. If the executor doesn't have tasks to execute, it continues waiting for new tasks and doesn't end its execution. A Java application won't end until all its non-daemon threads finish their execution. So, if you don't terminate the executor, your application will never end.

To indicate to the executor that you want to finish it, use the shutdown() method of the ThreadPoolExecutor class. When the executor finishes the execution of all the pending tasks, it finishes its execution as well. After you call the shutdown() method, if you try to send another task to the executor, it will be rejected and the executor will throw a RejectedExecutionException exception, unless you have implemented a manager for rejected tasks, as in our case. To manage the rejected tasks of an executor, you need to create a class that implements the RejectedExecutionHandler interface. This interface has a method called rejectedExecution() with two parameters:

+   - A Runnable object that stores the task that has been rejected
+   - An Executor object that stores the executor that rejected the task

This method is called for every task that is rejected by the executor. You need to establish the handler of the rejected tasks using the setRejectedExecutionHandler() method of the ThreadPoolExecutor class.

The following screenshot shows part of an execution of this example:

```
Server: Task Count: 99
Server: Completed Tasks: 0
Server: A new task has arrived
Server: Pool Size: 4
Server: Active Count: 4
Server: Task Count: 100
Server: Completed Tasks: 0
Main: Shutting down the Executor.
Main: Sending another Task.
Server: A new task has arrived
RejectedTaskController: The task com.packtpub.java9.concurrency.cookbook.chapter04.recipe01.task.Task@42dafa95 has been rejected
RejectedTaskController: java.util.concurrent.ThreadPoolExecutor@6500df86[Shutting down, pool size = 4, active threads = 4, queued tasks = 96, completed tasks = 0]
RejectedTaskController: terminating: true
RejectedTaskController: terminated: false
```

See that when the last task arrives at the executor, both the number of threads in the pool and the number of threads that are being executed are represented by 4. This refers to the number of cores of the PC on which the example was executed, and it is the number returned by the availableProcessors() method. Once this is done, we shut down the executor and the next task is rejected. RejectedTaskController writes information about the task and the executor in the console.