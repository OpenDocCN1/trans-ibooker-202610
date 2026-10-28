8. Implement the run() method. In a loop with 1,000,000 steps, assign the number of the steps to the integer attribute. You have to get the lock before the assignment and release it after:

```
@Override
public void run() {
    for (int i=0; i<1000000; i++) {
        lock.lock();
        number=i;
        lock.unlock();
    }
}
```

9. Implement the main class of the example by creating a class named Main and adding the main() method to it:

```
public class Main {
    public static void main(String[] args) {
```

10. Create a TaskAtomic object named atomicTask:

```
TaskAtomic atomicTask=new TaskAtomic();
```

11. Create a TaskLock object named lockTask:

```
TaskLock lockTask=new TaskLock();
```

12. Declare the number of threads and create an array of Thread objects to store the threads:

```
int numberThreads=50;
Thread threads[]=new Thread[numberThreads];
Date begin, end;
```

13. Launch the specified number of threads to execute the TaskLock object. Calculate and write its execution time in the console:

```
begin=new Date();
for (int i=0; i<numberThreads; i++) {
    threads[i]=new Thread(lockTask);
    threads[i].start();
}
```