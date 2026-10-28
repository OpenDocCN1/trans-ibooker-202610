The first important point is the creation of ThreadPoolExecutor in the constructor of the Server class. The ThreadPoolExecutor class has four different constructors, but due to their complexity, the Java concurrency API provides the Executors class to construct executors and other related objects. Although you can create ThreadPoolExecutor directly using one of its constructors, it's recommended that you use the Executors class.

In this case, you created a cached thread pool using the newFixedThreadPool() method of the Executors class in order to create the executor. This method creates an executor with the maximum number of threads. If the number of tasks you send is more than the number of threads, the remaining tasks will be blocked until there is a free thread available to process them. This method receives the maximum number of threads you want to have in your executor as parameters. In our case, we used the availableProcessors() method of the Runtime class that returns the number of processors available to JVM. Normally, this number matches the number of cores of the computer.

Reutilization of threads has the advantage that it reduces the time taken for thread creation. The cached thread pool, however, has the disadvantage of having constant lying threads for new tasks. Therefore, if you send too many tasks to this executor, you could overload the system.

Once you have created the executor, you can send tasks of the Runnable or Callable type for execution using the execute() method. In this case, you sent objects of the Task class that implements the Runnable interface.

You also printed some log messages with information about the executor. Specifically, you used the following methods:

+   - getPoolSize(): This method returned the actual number of threads in the pool of the executor.
+   - getActiveCount(): This method returned the number of threads that were executing tasks in the executor.
+   - getTaskCount(): This method returned the number of tasks that were scheduled for execution. The returned value is only an approximation because it changes dynamically.
+   - getCompletedTaskCount(): This method returned the number of tasks completed by the executor.