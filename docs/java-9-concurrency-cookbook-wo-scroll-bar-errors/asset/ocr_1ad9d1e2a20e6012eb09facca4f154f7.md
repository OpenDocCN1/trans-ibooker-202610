23. Call the awaitTermination() method of the executor object, indicating a long waiting period to guarantee that this method won't return until the three tasks have finished:

```
try {
    executor.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

24. For each task, write a message with the size of the result list using the get() method of the Task object:

```
try {
    System.out.printf("Main: System Task: Number of Results: %d\n", systemTask.get().size());
    System.out.printf("Main: App Task: Number of Results: %d\n", appsTask.get().size());
    System.out.printf("Main: Documents Task: Number of Results: %d\n",documentsTask.get().size());
} catch (InterruptedException e) {
    e.printStackTrace();
} catch (ExecutionException e) {
    e.printStackTrace();
}
```

# How it works...

The first point to take into consideration to understand this example is the difference between the submit() method of the ThreadPoolExecutor class when you pass a Callable object as the parameter and the submit() method when you pass a Runnable object as the parameter. In the first case, you can use the Future object that this method returns to control the status of the task and to get its result. But in the second case, when you pass a Runnable object, you can only use the Future object that this method returns to control the status of the task. If you call the get() method of that Future object, you will get a null value.

To override this behavior, you have implemented the Task class. This class extends the FutureTask class that is a class that implements the Future interface and the Runnable interface. When you call a method that returns a Future object (for example, the submit() method), you will normally get a FutureTask object. So you can use this class with two objectives: