![](bbox=[0.22, 0.09, 0.73, 0.24])

16.4 – Producer-Consumer design pattern

Most commonly, in this pattern, the producer thread and the consumer thread communicate via a queue (the producer enqueues data and the consumer dequeues data) and a set of rules specific to the modeled business. This queue is known as the data buffer. Of course, depending on the process design, other data structures can play the role of data buffer as well.

Now, let's assume the following scenario (set of rules):

+   - If the data buffer is empty, then the producer produces one product (by adding it to the data buffer).
+   - If the data buffer is not empty, then the consumer consumes one product (by removing it from the data buffer).
+   - As long as the data buffer is not empty, the producer waits.
+   - As long as the data buffer is empty, the consumer waits.

Next, let's solve this scenario via two common approaches. We will start with a solution that is based on the wait() and notify() methods.

# Producer-Consumer via wait() and notify()

Some interviewers may ask you to implement a Producer-Consumer application using the wait() and notify() methods. In other words, they don't allow you to use a built-in thread-safe queue such as BlockingQueue.

For example, let's consider that the data buffer (queue) is represented by a LinkedList, that is, a non-thread-safe data structure. To ensure that this shared LinkedList is accessible in a thread-safe manner by the producer and the consumer, we rely on the synchronized keyword.