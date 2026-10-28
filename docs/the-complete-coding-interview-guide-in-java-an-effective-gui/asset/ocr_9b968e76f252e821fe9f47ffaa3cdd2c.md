On the other hand, if the queue is not empty, then the consumer dequeues one product and notifies the producer thread via notify(), as follows:

```
synchronized (queue) {
    String product = queue.remove(0);

    if (product != null) {
        // simulate consuming time
        Thread.sleep(rnd.nextInt(MAX_CONS_TIME_MS));
        logger.info(() -> "Consumed: " + product);

        queue.notify();
    }
}
```

The complete code is available in the bundled code, ProducerConsumerWaitNotify.

# Producer-Consumer via built-in blocking queues

If you can use a built-in blocking queue, then you can choose a BlockingQueue or even a TransferQueue. Both of them are thread-safe. In the following code, we use a TransferQueue or, more precisely, a LinkedTransferQueue.

# The producer

The producer waits for the consumer to be available via hasWaitingConsumer():

```
while (queue.hasWaitingConsumer()) {

    String product = "product-" + rnd.nextInt(1000);

    // simulate the production time
    Thread.sleep(rnd.nextInt(MAX_PROD_TIME_MS));
    queue.add(product);
    logger.info(() -> "Produced: " + product);
}
```

After adding a product to the queue, the consumer should be ready to consume it.