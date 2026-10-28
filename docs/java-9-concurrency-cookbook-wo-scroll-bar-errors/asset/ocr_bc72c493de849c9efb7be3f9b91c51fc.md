```
} else if (result>0) {
    return 1;
}
return 0;
}
```

5. Implement the getDelay() method. Return the difference between the start date of the object and the actual date in TimeUnit, which is received as a parameter:

```
public long getDelay(TimeUnit unit) {
    Date now=new Date();
    long diff=startDate.getTime()-now.getTime();
    return unit.convert(diff,TimeUnit.MILLISECONDS);
}
```

6. Create a class named Task and specify that it implements the Runnable interface:

```
public class Task implements Runnable {
```

7. Declare a private int attribute named id to store a number that identifies this task:

```
private final int id;
```

8. Declare a private DelayQueue attribute parameterized by the Event class named queue:

```
private final DelayQueue<Event> queue;
```

9. Implement the constructor of the class to initialize its attributes:

```
public Task(int id, DelayQueue<Event> queue) {
    this.id=id;
    this.queue=queue;
}
```