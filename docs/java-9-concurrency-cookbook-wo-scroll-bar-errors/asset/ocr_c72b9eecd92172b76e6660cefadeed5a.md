```
System.out.printf("******************************************
  ******\n");
try {
    TimeUnit.SECONDS.sleep(1);
} catch (InterruptedException e) {
    e.printStackTrace();
}
} while (!task.isDone());
```

34. Shut down the pool using the shutdown() method:

```
pool.shutdown();
```

35. Wait for the finalization of the tasks using the awaitTermination() method:

```
try {
    pool.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

36. Write the number of the appearances of the word in the document. Check that this number is the same as the number written by the DocumentMock class:

```
try {
    System.out.printf("Main: The word appears %d in the
                    document",task.get());
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}
```

# How it works...

In this example, you implemented two different tasks:

+   - DocumentTask: A task of this class has to process a set of lines of the document determined by the start and end attributes. If this set of lines has a size smaller than 10, it creates LineTask per line, and when they finish their execution, it sums the results of those tasks and returns the result of the sum. If the set of lines the task has to process has a size of 10 or bigger, it divides the set in two and creates two DocumentTask objects to process those new sets. When those tasks finish their execution, the tasks sum their results and return that sum as a result.