8. Finally, implement the Main class with the main() method. Create a Position and StampedLock object, create three threads--one for each task--start the threads, and wait for their finalization:

```
public class Main {

    public static void main(String[] args) {

        Position position=new Position();
        StampedLock lock=new StampedLock();

        Thread threadWriter=new Thread(new Writer(position,lock));
        Thread threadReader=new Thread(new Reader(position, lock));
        Thread threadOptReader=new Thread(new OptimisticReader
            (position, lock));

        threadWriter.start();
        threadReader.start();
        threadOptReader.start();

        try {
            threadWriter.join();
            threadReader.join();
            threadOptReader.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

# How it works...

In this example, we tested the three modes you can use with a stamped lock. In the Writer task, we get the lock with the writeLock() method (that acquires the lock in write mode). In the Reader task, we get the lock with the readLock() method (that acquires the lock in read mode). Finally, in the OptimisticRead task, first we use tryOptimisticRead() and then we use the validate() method to check whether we can access data or not.

The first two methods, if they can get control of the lock, wait until they get the lock. The tryOptimisticRead() method always returns a value. It will be 0 if we are unable to use the lock and a value different from 0 if we can use it. Remember that in this case, we always need to use the validate() method to check whether we can really access the data.