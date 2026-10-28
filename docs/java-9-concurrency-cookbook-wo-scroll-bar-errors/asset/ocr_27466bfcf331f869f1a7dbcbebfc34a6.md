16. Create and launch 100 Producer objects and store the threads in the array created earlier:

```
for (int i=0; i<THREADS; i++) {
    Producer producer=new Producer("Producer: "+ i , buffer);
    producerThreads[i]=new Thread(producer);
    producerThreads[i].start();
}
```

17. Wait for the finalization of the threads using the join() method:

```
for (int i=0; i<THREADS; i++){
    try {
        producerThreads[i].join();
        consumerThreads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

18. Write a message to the console with the size of the buffer:

```
System.out.printf("Main: Size of the buffer: %d\n", buffer.size());
System.out.printf("Main: End of the example\n");
```

# How it works...

In this recipe, you have used the LinkedTransferQueue class parameterized with the String class to implement the producer/consumer problem. This LinkedTransferQueue class is used as a buffer to share the data between producers and consumers.

You have implemented a Producer class that adds strings to the buffer using the put() method. You have executed 100 producers and every producer inserts in the buffer 10,000 strings, so you insert 1,000,000 strings in the buffer. The put() method adds the element at the end of the buffer.

You also have implemented a Consumer class, which gets a string from the buffer using the take() method. This method returns and deletes the first element of the buffer. If the buffer is empty, the method blocks the thread that makes the call until there are strings in the buffer to consume. You have executed 100 consumers, and every consumer gets 10,000 strings from the buffer.