```
System.out.printf("MyExecutor: Running tasks: %d\n", getActiveCount());
System.out.printf("MyExecutor: Pending tasks: %d\n", getQueue().size());
super.shutdown();
}
```

5. Override the shutdownNow() method. Write in the console information about the executed, running, and pending tasks. Then, call the shutdownNow() method of the parent class using the super keyword:

```
@Override
public List<Runnable> shutdownNow() {
    System.out.printf("MyExecutor: Going to immediately shutdown.\n");
    System.out.printf("MyExecutor: Executed tasks: %d\n", getCompletedTaskCount());
    System.out.printf("MyExecutor: Running tasks: %d\n", getActiveCount());
    System.out.printf("MyExecutor: Pending tasks: %d\n", getQueue().size());
    return super.shutdownNow();
}
```

6. Override the beforeExecute() method. Write a message in the console with the name of the thread that is going to execute the task and the hash code of the task. Store the start date in HashMap using the hash code of the task as the key:

```
@Override
protected void beforeExecute(Thread t, Runnable r) {
    System.out.printf("MyExecutor: A task is beginning: %s : %s\n", t.getName(), r.hashCode());
    startTimes.put(r, new Date());
}
```

7. Override the afterExecute() method. Write a message in the console with the result of the task and calculate the running time of the task after subtracting the start date of the task stored in HashMap of the current date:

```
@Override
protected void afterExecute(Runnable r, Throwable t) {
    Future<?> result=(Future<?>)r;
    try {
        System.out.printf("***********************************\n");
        System.out.printf("MyExecutor: A task is finishing.\n");
```