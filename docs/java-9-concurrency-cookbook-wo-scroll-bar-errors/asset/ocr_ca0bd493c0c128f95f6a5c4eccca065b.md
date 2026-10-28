4. Implement the run() method. First, get control of the lock1 object using the lock() method and write aa message in the console indicating that you have got it:

```
@Override
public void run() {
    lock1.lock();
    System.out.printf("Task 1: Lock 1 locked\n");
```

5. Then, get control of lock2 using the lock() method and write a message in the console indicating that you have got it:

```
lock2.lock();
System.out.printf("Task 1: Lock 2 locked\n");
```

6. Finally, release the two lock objects-first the lock2 object and then the lock1 object:

```
lock2.unlock();
lock1.unlock();
}
```

7. Create a class named Task2 and specify that it implements the Runnable interface:

```
public class Task2 implements Runnable{
```

8. Declare two private Lock attributes, named lock1 and lock2:

```
private Lock lock1, lock2;
```

9. Implement the constructor of the class to initialize its attributes:

```
public Task2(Lock lock1, Lock lock2) {
    this.lock1=lock1;
    this.lock2=lock2;
}
```

10. Implement the run() method. First, get control of the lock2 object using the lock() method and write a message in the console indicating that you have got it:

```
@Override
public void run() {
    lock2.lock();
    System.out.printf("Task 2: Lock 2 locked\n");
```