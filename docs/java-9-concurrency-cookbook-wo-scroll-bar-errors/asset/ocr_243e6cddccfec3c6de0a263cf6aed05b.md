# How to do it...

Follow these steps to implement the example:

+   1. Create a class named `ProducerConsumerTest` that extends the `MultithreadedTestCase` class:
+   
+   ```
+   public class ProducerConsumerTest extends MultithreadedTestCase {
+   ```
+   
+   2. Declare a private `LinkedTransferQueue` attribute parameterized by the `String` class named `queue`:
+   
+   ```
+   private LinkedTransferQueue<String> queue;
+   ```
+   
+   3. Implement the `initialize()` method. This method won't receive any parameters and will return no value. It will call the `initialize()` method of its parent class and then initialize the queue attribute:
+   
+   ```
+   @Override
+   public void initialize() {
+       super.initialize();
+       queue=new LinkedTransferQueue<String>();
+       System.out.printf("Test: The test has been initialized\n");
+   }
+   ```
+   
+   4. Implement the `thread1()` method. It will implement the logic of the first consumer. Call the `take()` method of the queue and then write the returned value in the console:
+   
+   ```
+   public void thread1() throws InterruptedException {
+       String ret=queue.take();
+       System.out.printf("Thread 1: %s\n",ret);
+   }
+   ```
+   
+   5. Implement the `thread2()` method. It will implement the logic of the second consumer. First wait until the first thread has slept in the `take()` method. To put the thread to sleep, use the `waitForTick()` method. Then, call the `take()` method of the queue and write the returned value in the console:
+   
+   ```
+   public void thread2() throws InterruptedException {
+       waitForTick(1);
+       String ret=queue.take();
+       System.out.printf("Thread 2: %s\n",ret);
+   }
+   ```