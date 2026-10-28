
+   2. Declare a private AtomicInteger attribute named counter to store the number of consumers that are waiting to consume elements:

```
private final AtomicInteger counter;
```

+   3. Declare a private LinkedBlockingQueue attribute named transferred:

```
private final LinkedBlockingQueue<E> transferred;
```

+   4. Declare a private ReentrantLock attribute named lock:

```
private final ReentrantLock lock;
```

+   5. Implement the constructor of the class to initialize its attributes:

```
public MyPriorityTransferQueue() {
    counter=new AtomicInteger(0);
    lock=new ReentrantLock();
    transferred=new LinkedBlockingQueue<E>();
}
```

+   6. Implement the tryTransfer() method. This method tries to send the element to a waiting consumer immediately, if possible. If there isn't any consumer waiting, the method returns false:

```
@Override
public boolean tryTransfer(E e) {
    boolean value=false;
    try {
        lock.lock();
        if (counter.get() == 0) {
            value = false;
        } else {
            put(e);
            value = true;
        }
    } finally {
        lock.unlock();
    }
    return value;
}
```

7. Implement the transfer() method. This method tries to send the element to a waiting consumer immediately, if possible. If there is no consumer waiting, the method stores the element in a special queue to be sent to the first consumer that tries to get an element and blocks the thread until the element is consumed:

```
@Override
public void transfer(E e) throws InterruptedException {
    lock.lock();
    if (counter.get() != 0) {
        try {
            put(e);
        } finally {
            lock.unlock();
        }
    } else {
        try {
            transferred.add(e);
        } finally {
            lock.unlock();
        }
        synchronized (e) {
            e.wait();
        }
    }
}
```

8. Implement the tryTransfer() method that receives three parameters: the element, the time to wait for a consumer if there is none, and the unit of time used to specify the wait. If there is a consumer waiting, it sends the element immediately. Otherwise, it converts the time specified into milliseconds and uses the wait() method to put the thread to sleep. When the consumer takes the element, if the thread is sleeping in the wait() method, you need to wake it up using the notify() method, as you'll see in a moment:

```
@Override
public boolean tryTransfer(E e, long timeout, TimeUnit unit)
        throws InterruptedException {
    lock.lock();
    if (counter.get() != 0) {
        try {
            put(e);
        } finally {
            lock.unlock();
        }
        return true;
    } else {
```

```
long newTimeout=0;
try {
    transferred.add(e);
    newTimeout = TimeUnit.MILLISECONDS.convert(timeout, unit);
} finally {
    lock.unlock();
}
e.wait(newTimeout);
lock.lock();
boolean value;
try {
    if (transferred.contains(e)) {
        transferred.remove(e);
        value = false;
    } else {
        value = true;
    }
} finally {
    lock.unlock();
}
return value;
}
```

9. Implement the hasWaitingConsumer() method. Use the value of the counter attribute to calculate the return value of this method. If the counter has a value that is bigger than zero, it returns true; else, it returns false:

```
@Override
public boolean hasWaitingConsumer() {
    return (counter.get()!=0);
}
```

10. Implement the getWaitingConsumerCount() method. Return the value of the counter attribute:

```
@Override
public int getWaitingConsumerCount() {
    return counter.get();
}
```

11. Implement the take() method. This method is called by the consumers when they want to consume an element. First, get the lock defined earlier and increment the number of waiting consumers:

```
@Override
public E take() throws InterruptedException {
```

```
lock.lock();
try {
    counter.incrementAndGet();
```

12. If there aren't any elements in the transferred queue, free the lock and try to get an element from the queue using the take() element and get the lock again. If there aren't any elements in the queue, this method will put the thread to sleep until there are elements to consume:

```
E value=transferred.poll();
if (value==null) {
    lock.unlock();
    value=super.take();
    lock.lock();
```

13. Otherwise, take the element from the transferred queue and wake up the thread that is waiting to consume that element, if there is one. Take into account that you are synchronizing an object coming to this class from the outside. You have to guarantee that the object wouldn't be used for locking in other parts of the application:

```
} else {
    synchronized (value) {
        value.notify();
    }
}
```

14. Finally, decrement the counter of waiting consumers and free the lock:

```
counter.decrementAndGet();
} finally {
    lock.unlock();
}
return value;
}
```

15. Next, implement a class named Event that extends the Comparable interface parameterized by the Event class:

```
public class Event implements Comparable<Event> {
```

16. Declare a private String attribute named thread to store the name of the thread that creates the event:

```
private final String thread;
```

17. Declare a private int attribute named priority to store the priority of the event:

```
private final int priority;
```

18. Implement the constructor of the class to initialize its attributes:

```
public Event(String thread, int priority){
    this.thread=thread;
    this.priority=priority;
}
```

19. Implement a method to return the value of the thread attribute:

```
public String getThread() {
    return thread;
}
```

20. Implement a method to return the value of the priority attribute:

```
public int getPriority() {
    return priority;
}
```

21. Implement the compareTo() method. This method compares the actual event with the event received as a parameter. Return -1 if the actual event has a higher priority than the parameter, 1 if the actual event has a lower priority than the parameter, and 0 if both the events have the same priority. You will get the list ordered by priority in descending order. Events with a higher priority will be stored first in the queue:

```
public int compareTo(Event e) {
    return Integer.compare(e.priority, this.getPriority());
}
```

22. Implement a class named Producer that implements the Runnable interface:

```
public class Producer implements Runnable {
```

23. Declare a private MyPriorityTransferQueue attribute parameterized by the Event class named buffer to store the events generated by this producer:

```
private final MyPriorityTransferQueue<Event> buffer;
```

24. Implement the constructor of the class to initialize its attributes:

```
public Producer(MyPriorityTransferQueue<Event> buffer) {
    this.buffer=buffer;
}
```

25. Implement the run() method of the class. Create 100 Event objects using its order of creation as priority (the latest event will have the highest priority) and insert them into the queue using the put() method:

```
@Override
public void run() {
    for (int i=0; i<100; i++) {
        Event event=new Event(Thread.currentThread().getName(), i);
        buffer.put(event);
    }
}
```

26. Implement a class named Consumer that implements the Runnable interface:

```
public class Consumer implements Runnable {
```

27. Declare a private MyPriorityTransferQueue attribute parameterized by the Event class named buffer to get the events consumed by this class:

```
private final MyPriorityTransferQueue<Event> buffer;
```

28. Implement the constructor of the class to initialize its attribute:

```
public Consumer(MyPriorityTransferQueue<Event> buffer) {
    this.buffer=buffer;
}
```

29. Implement the run() method. It consumes 1,002 events (all the events generated in the example) using the take() method and writes the number of threads that generated the event and their priority in the console:

```
@Override
public void run() {
    for (int i=0; i<1002; i++) {
        try {
            Event value=buffer.take();
            System.out.printf("Consumer: %s: %d\n",value.getThread(),
                value.getPriority());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
