7. Declare an object of the ThreadLocal<Date> class. This object will have an implicit implementation that would include the initialValue() method. This method will return the actual date:

```
private static ThreadLocal<Date> startDate=new
    ThreadLocal<Date>() {
protected Date initialValue(){
    return new Date();
}
};
```

8. Implement the run() method. It has the same functionality as the run() method of UnsafeTask class, but it changes the way it accesses the startDate attribute. Now we will use the get() method of the startDate object:

```
@Override
public void run() {
    System.out.printf("Starting Thread: %s : %s\n",
        Thread.currentThread().getId(),startDate.get());
    try {
        TimeUnit.SECONDS.sleep((int)Math.rint(Math.random()*10));
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    System.out.printf("Thread Finished: %s : %s\n",
        Thread.currentThread().getId(),startDate.get());
}
```

9. The Main class of this example is the same as the unsafe example. The only difference is that it changes the name of the Runnable class.

10. Run the example and analyze the difference.