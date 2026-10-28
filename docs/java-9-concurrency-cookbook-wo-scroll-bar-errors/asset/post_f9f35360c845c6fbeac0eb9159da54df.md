
When you execute the program, the exception is thrown, but the program doesn't stop. In the Main class you have included a call to the isCompletedAbnormally() method of the ForkJoinTask class using the original task. This method returns true if that task, or one of its subtasks, has thrown an exception. You also used the getException() method of the same object to get the Exception object that it has thrown.

When you throw an unchecked exception in a task, it also affects its parent task (the task that sent it to the ForkJoinPool class) and the parent task of its parent task, and so on. If you revise the entire output of the program, you'll see that there aren't output messages for the finalization of some tasks. The starting messages of those tasks are as follows:

```
Task: Starting form 0 to 100
Task: Starting form 0 to 50
Task: Starting form 0 to 25
Task: Starting form 0 to 12
Task: Starting form 0 to 6
```

These tasks are the ones that threw the exception and its parent tasks. All of them have finished abnormally. Take this into account when you develop a program with the ForkJoinPool and ForkJoinTask objects that can throw exceptions if you don't want this behavior.

The following screenshot shows part of an execution of this example:

![](img/cc06c59cd21f558f43e9475d7f212170_253_0.png)

## There's more...

In this example, you used the `join()` method to wait for the finalization of tasks and get their results. You can also use one of the following two versions of the `get()` method with this purpose:

- `get()`: This version of the `get()` method returns the value returned by the `compute()` method if `ForkJoinTask` has finished its execution, or it waits until its finalization.
- `get(long timeout, TimeUnit unit)`: This version of the `get()` method, if the result of the task isn't available, waits the specified time for it. If the specified period of time passes and the result isn't yet available, the method returns a `null` value. The `TimeUnit` class is an enumeration with the following constants: `DAYS`, `HOURS`, `MICROSECONDS`, `MILLISECONDS`, `MINUTES`, `NANOSECONDS`, and `SECONDS`.

There are two main differences between the `get()` and `join()` methods:

- The `join()` method can't be interrupted. If you interrupt the thread that called the `join()` method, the method throws `InterruptedException`.
- While the `get()` method will return `ExecutionException` if the tasks throw any unchecked exception, the `join()` method will return `RuntimeException`.

You can obtain the same result obtained in the example if, instead of throwing an exception, you use the `completeExceptionally()` method of the `ForkJoinTask` class. The code would be as follows:

```
Exception e=new Exception("This task throws an Exception: "+
"Task from "+start+" to "+end);
completeExceptionally(e);
```

## See also

- The *Creating a fork/join pool* recipe in this chapter

## Canceling a task

When you execute the ForkJoinTask objects in a ForkJoinPool class, you can cancel them before they start their execution. The ForkJoinTask class provides the cancel() method for this purpose. There are some points you have to take into account when you want to cancel a task, which are as follows:

- The ForkJoinPool class doesn't provide any method to cancel all the tasks it has running or waiting in the pool
- When you cancel a task, you don't cancel the tasks this task has executed

In this recipe, you will implement an example of the cancellation of ForkJoinTask objects. You will look for the position of a number in an array. The first task that finds the number will cancel the remaining tasks. As that functionality is not provided by the fork/join framework, you will implement an auxiliary class to do this cancellation.

## Getting ready...

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or other IDE such as NetBeans, open it and create a new Java project.

## How to do it...

Follow these steps to implement the example:

- 1. Create a class named ArrayGenerator. This class will generate an array of random integer numbers with the specified size. Implement a method named generateArray(). It will generate the array of numbers. It receives the size of the array as a parameter:

```
public class ArrayGenerator {
    public int[] generateArray(int size) {
        int array[]=new int[size];
        Random random=new Random();
        for (int i=0; i<size; i++){
            array[i]=random.nextInt(10);
        }
        return array;
    }
}
```

- 8. Declare a private array of int numbers named numbers:

```
private int numbers[];
```

- 9. Declare two private int attributes named start and end. These attributes will determine the elements of the array this task has to process:

```
private int start, end;
```

- 10. Declare a private int attribute named number to store the number you are going to look for:

```
private int number;
```

- 11. Declare a private TaskManager attribute named manager. You will use this object to cancel all the tasks:

```
private TaskManager manager;
```

- 12. Declare a private int constant and initialize it to -1. It will be the returned value by the task when it doesn't find the number:

```
private final static int NOT_FOUND=-1;
```

- 13. Implement the constructor of the class to initialize its attributes:

```
public SearchNumberTask(int numbers[], int start, int end, int number, TaskManager manager){
    this.numbers=numbers;
    this.start=start;
    this.end=end;
    this.number=number;
    this.manager=manager;
}
```

- 14. Implement the compute() method. Start the method by writing a message to the console indicating the values of the start and end attributes:

```
@Override
protected Integer compute() {
    System.out.println("Task: "+start+":"+end);
```

- 15. If the difference between the start and end attributes is greater than 10 (the task has to process more than 10 elements of the array), call the launchTasks() method to divide the work of this task into two subtasks:

```
int ret;
if (end-start>10) {
    ret=launchTasks();
```

- 16. Otherwise, look for the number in the block of the array that the task calling the lookForNumber() method has to process:

```
} else {
    ret=lookForNumber();
}
```

- 17. Return the result of the task:

```
return ret;
```

- 18. Implement the lookForNumber() method:

```
private int lookForNumber() {
```

- 19. For all the elements in the block of elements this task has to process, compare the value stored in that element with the number you are looking for. If they are equal, write a message to the console indicating, in such a circumstance, to use the cancelTasks() method of the TaskManager object to cancel all the tasks, and return the position of the element where you found the number:

```
for (int i=start; i<end; i++){
    if (numbers[i]==number) {
        System.out.printf("Task: Number %d found in position %d\n", number,i);
        manager.cancelTasks(this);
        return i;
    }
}
```

- 20. Inside the loop, put the task to sleep for one second:

```
try {
    TimeUnit.SECONDS.sleep(1);
} catch (InterruptedException e) {
    e.printStackTrace();
}
}
```

- 27. Implement the main class of the example by creating a class named Main with a main() method:

```
public class Main {
    public static void main(String[] args) {
```

- 28. Create an array of 1000 numbers using the ArrayGenerator class:

```
ArrayGenerator generator=new ArrayGenerator();
int array[]=generator.generateArray(1000);
```

- 29. Create a TaskManager object:

```
TaskManager manager=new TaskManager();
```

- 30. Create a ForkJoinPool object using the default constructor:

```
ForkJoinPool pool=new ForkJoinPool();
```

- 31. Create a Task object to process the array generated before:

```
SearchNumberTask task=new SearchNumberTask (array,0,1000,
    5,manager);
```

- 32. Execute the task in the pool asynchronously using the execute() method:

```
pool.execute(task);
```

- 33. Shut down the pool using the shutdown() method:

```
pool.shutdown();
```

- 34. Wait for the finalization of the tasks using the awaitTermination() method of the ForkJoinPool class:

```
try {
    pool.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

- 35. Write a message to the console indicating the end of the program:

```
System.out.printf("Main: The program has finished\n");
```
