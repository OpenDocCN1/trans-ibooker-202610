```
if (Thread.currentThread().isInterrupted()) {
    System.out.printf("%d : Interrupted\n", Thread.currentThread().getId());
    return;
}
}
```

5. Now, implement the main class of the example by creating a class called Main and implement the main() method:

```
public class Main {
    public static void main(String[] args) {
```

6. First, calculate the number of threads you are going to launch. We use the availableProcessors() method of the Runtime class (we obtain the runtime object associated with the current Java application with the static method, called getRuntime(), of that class). This method returns the number of processors available to the JVM, which is normally equal to the number of cores of the computer that run the application:

```
int numberOfThreads = 2 * Runtime.getRuntime()
    .availableProcessors();
```

7. Create an object of the MyThreadGroup class:

```
MyThreadGroup threadGroup=new MyThreadGroup("MyThreadGroup");
```

8. Create an object of the Task class:

```
Task task=new Task();
```

9. Create the calculated number of Thread objects with this Task class and start them:

```
for (int i = 0; i < numberOfThreads; i++) {
    Thread t = new Thread(threadGroup, task);
    t.start();
}
```

10. Then, write information about ThreadGroup in the console:

```
System.out.printf("Number of Threads: %d\n", threadGroup.activeCount());
System.out.printf("Information about the Thread Group\n");
threadGroup.list();
```