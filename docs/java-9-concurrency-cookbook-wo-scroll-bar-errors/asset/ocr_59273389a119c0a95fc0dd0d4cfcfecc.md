18. Create the threads to execute the producer and consumer and start the threads:

```
Thread threadProducer=new Thread(producer);
Thread threadConsumer=new Thread(consumer);

threadProducer.start();
threadConsumer.start();
```

# How it works...

The consumer begins with an empty buffer and calls Exchanger to synchronize with the producer. It needs data to consume. The producer begins its execution with an empty buffer. It creates 10 strings, stores them in the buffer, and uses the Exchanger to synchronize with the consumer.

At this point, both the threads (producer and consumer) are in Exchanger, which changes the data structures. So when the consumer returns from the exchange() method, it will have a buffer with 10 strings. When the producer returns from the exchange() method, it will have an empty buffer to fill again. This operation will be repeated 10 times.

If you execute the example, you will see how producer and consumer do their jobs concurrently and how the two objects interchange their buffers in every step. As it occurs with other synchronization utilities, the first thread that calls the exchange() method is put to sleep until the other threads arrive.

# There's more...

The Exchanger class has another version of the exchange method: exchange(V data, long time, TimeUnit unit). where, V is the type used as a parameter in the declaration of Phaser (List<String> in our case). The thread will sleep until it's interrupted, another thread arrives, or the specified time passes. In this case, a TimeoutException is thrown. The TimeUnit class is an enumeration with the following constants: DAYS, HOURS, MICROSECONDS, MILLISECONDS, MINUTES, NANOSECONDS, and SECONDS.