14. Cancel all the tasks you have sent to the executor:

```
for (int i=0; i<resultTasks.length; i++) {
    resultTasks[i].cancel(true);
}
```

15. Write the result of those tasks that haven't been canceled to the console, using the get() method of the ResultTask objects:

```
for (int i=0; i<resultTasks.length; i++) {
    try {
        if (!resultTasks[i].isCancelled()) {
            System.out.printf("%s\n",resultTasks[i].get());
        }
    } catch (InterruptedException | ExecutionException e) {
        e.printStackTrace();
    }
}
```

16. Finish the executor using the shutdown() method:

```
executor.shutdown();
}
```

# How it works...

The done() method is called by the FutureTask class when the task that is being controlled finishes its execution. In this example, you implemented a Callable object, the ExecutableTask class, and then a subclass of the FutureTask class that controls the execution of the ExecutableTask objects.

The done() method is called internally by the FutureTask class after establishing the return value and changing the status of the task to isDone. You can't change the result value of the task or change its status, but you can close resources used by the task, write log messages, or send notifications. The FutureTask class might be used to ensure that a specific task is run only once, as calling its run() method will execute its wrapped Runnable/Callable interface only once (and the result can be fetched with get when it's available).