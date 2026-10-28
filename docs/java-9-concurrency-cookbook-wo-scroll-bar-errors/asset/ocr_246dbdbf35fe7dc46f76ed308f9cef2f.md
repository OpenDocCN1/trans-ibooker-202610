+   20. Create a new task object to sum all the elements of the array:
+   ```
+   MyRecursiveTask task=new MyRecursiveTask(array,0,array.length);
+   ```
+   21. Send the task to the pool using the execute() method:
+   ```
+   pool.execute(task);
+   ```
+   22. Wait for the end of the task using the join() method:
+   ```
+   task.join();
+   ```
+   23. Shut down the pool using the shutdown() method:
+   ```
+   pool.shutdown();
+   ```
+   24. Wait for the finalization of the executor using the awaitTermination() method:
+   ```
+   pool.awaitTermination(1, TimeUnit.DAYS);
+   ```
+   25. Write the result of the task in the console, using the get() method:
+   ```
+   System.out.printf("Main: Result: %d\n",task.get());
+   ```
+   26. Write a message in the console indicating the end of the example:
+   ```
+   System.out.printf("Main: End of the program\n");
+   ```

# How it works...

Threads used by the fork/join framework are called worker threads. Java includes the ForkJoinWorkerThread class that extends the Thread class and implements the worker threads used by the fork/join framework.

In this recipe, you implemented the MyWorkerThread class that extends the ForkJoinWorkerThread class and overrides two methods of the ForkJoinWorkerThread class. Your objective is to implement a counter of tasks in each worker thread so that you can know how many tasks a worker thread has executed. You implemented the counter with a ThreadLocal attribute. This way, each thread will have its own counter in a transparent way for you, the programmer.