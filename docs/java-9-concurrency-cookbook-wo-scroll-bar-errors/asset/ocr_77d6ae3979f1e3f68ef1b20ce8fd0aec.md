20. Create five Task objects and five threads to execute them. Write log messages to indicate that you're going to launch a new thread and that you have created the thread:

```
for (int i=0; i<threads.length; i++) {
    logger.log(Level.INFO,"Launching thread: "+i);
    Task task=new Task();
    threads[i]=new Thread(task);
    logger.log(Level.INFO,"Thread created: "+
                threads[i].getName());
    threads[i].start();
}
```

21. Write a log message to indicate that you have created the threads:

```
logger.log(Level.INFO,"Ten Threads created."+
    "Waiting for its finalization");
```

22. Wait for the finalization of the five threads using the join() method. After the finalization of each thread, write a log message indicating that the thread has finished:

```
for (int i=0; i<threads.length; i++) {
    try {
        threads[i].join();
        logger.log(Level.INFO,"Thread has finished its execution",
                    threads[i]);
    } catch (InterruptedException e) {
        logger.log(Level.SEVERE, "Exception", e);
    }
}
```

23. Write a log message to indicate the end of the execution of the main program, using the exiting() method:

```
logger.exiting(Main.class.getName(), "main()");
}
```