27. Implement the main class of the example by creating a class named Main with a main() method:

```
public class Main {
    public static void main(String[] args) {
```

28. Create an array of 1000 numbers using the ArrayGenerator class:

```
ArrayGenerator generator=new ArrayGenerator();
int array[]=generator.generateArray(1000);
```

29. Create a TaskManager object:

```
TaskManager manager=new TaskManager();
```

30. Create a ForkJoinPool object using the default constructor:

```
ForkJoinPool pool=new ForkJoinPool();
```

31. Create a Task object to process the array generated before:

```
SearchNumberTask task=new SearchNumberTask (array,0,1000,
    5,manager);
```

32. Execute the task in the pool asynchronously using the execute() method:

```
pool.execute(task);
```

33. Shut down the pool using the shutdown() method:

```
pool.shutdown();
```

34. Wait for the finalization of the tasks using the awaitTermination() method of the ForkJoinPool class:

```
try {
    pool.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

35. Write a message to the console indicating the end of the program:

```
System.out.printf("Main: The program has finished\n");
```