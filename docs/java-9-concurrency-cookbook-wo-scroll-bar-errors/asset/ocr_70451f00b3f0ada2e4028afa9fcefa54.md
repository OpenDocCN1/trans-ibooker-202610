+   2. Declare a private AtomicInteger attribute named counter to store the number of consumers that are waiting to consume elements:

```
private final AtomicInteger counter;
```

+   3. Declare a private LinkedBlockingQueue attribute named transferred:

```
private final LinkedBlockingQueue<E> transfered;
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
    transfered=new LinkedBlockingQueue<E>();
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