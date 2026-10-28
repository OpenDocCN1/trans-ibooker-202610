30. Implement the main class of the example by creating a class named Main with a main() method:

```
public class Main {

    public static void main(String[] args) throws Exception {
```

31. Create a MyPriorityTransferQueue object named buffer:

```
MyPriorityTransferQueue<Event> buffer=new
    MyPriorityTransferQueue<Event>();
```

32. Create a Producer task and launch 10 threads to execute this task:

```
Producer producer=new Producer(buffer);
Thread producerThreads[]=new Thread[10];
for (int i=0; i<producerThreads.length; i++) {
    producerThreads[i]=new Thread(producer);
    producerThreads[i].start();
}
```

33. Create and launch a Consumer task:

```
Consumer consumer=new Consumer(buffer);
Thread consumerThread=new Thread(consumer);
consumerThread.start();
```

34. Write the actual consumer count in the console:

```
System.out.printf("Main: Buffer: Consumer count: %d\n",
    buffer.getWaitingConsumerCount());
```

35. Transfer an event to the consumer using the transfer() method:

```
Event myEvent=new Event("Core Event",0);
buffer.transfer(myEvent);
System.out.printf("Main: My Event has ben transfered.\n");
```

36. Wait for the finalization of the producers using the join() method:

```
for (int i=0; i<producerThreads.length; i++) {
    try {
        producerThreads[i].join();
```