```
long newTimeout=0;
try {
    transfered.add(e);
    newTimeout = TimeUnit.MILLISECONDS.convert(timeout, unit);
} finally {
    lock.unlock();
}
e.wait(newTimeout);
lock.lock();
boolean value;
try {
    if (transfered.contains(e)) {
        transfered.remove(e);
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