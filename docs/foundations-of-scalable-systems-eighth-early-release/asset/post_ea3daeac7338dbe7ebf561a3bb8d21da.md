
```
29      } catch (InterruptedException e) {
30          Thread.currentThread().interrupt();
31      }
32  }
33 }
```

To bring the philosophers described in Example 4-7 to life, we must instantiate a thread for each and give each philosopher access to its neighboring chopsticks. This is done through the thread constructor call on line 16 in Example 4-8. In the for loop we create five philosophers and start these as independent threads, where each chopstick is accessible to two threads, one as a left chopstick, and one as a right.

## Example 4-8. Dining Philosophers - deadlocked version

```
private final static int NUMCHOPSTICKS = 5 ;
private final static int NUMPHILOSOPHERS = 5;
public static void main(String[] args) throws Exception {

    final Philosopher[] ph = new Philosopher[NUMPHILOSOPHERS];
    Object[] chopSticks = new Object[NUMCHOPSTICKS];

    for (int i = 0; i < NUMCHOPSTICKS; i++) {
        chopSticks[i] = new Object();
    }

    for (int i = 0; i < NUMPHILOSOPHERS; i++) {
        Object leftChopStick = chopSticks[i];
        Object rightChopStick = chopSticks[(i + 1) % chopSticks.length];

        ph[i] = new Philosopher(leftChopStick, rightChopStick);
    }

    Thread th = new Thread(ph[i], "Philosopher " + (i + 1));
    th.start();
}
}
```

Running this code produces the following output on my first attempt. If you run the code you will almost certainly see different outputs, but the final outcome will be the same.

```
Philosopher 4 : Thinking deeply
Philosopher 5 : Thinking deeply
Philosopher 1 : Thinking deeply
Philosopher 2 : Thinking deeply
Philosopher 3 : Thinking deeply
Philosopher 4 : Picked up left chop stick
Philosopher 1 : Picked up left chop stick
Philosopher 3 : Picked up left chop stick
Philosopher 5 : Picked up left chop stick
Philosopher 2 : Picked up left chop stick
```

10 lines of output, then ... nothing! We have a deadlock. This is a classic circular waiting deadlock. Imagine the following scenario:

- 1. Each philosopher indulges in a long thinking session
- 2. Simultaneously, they all decide they are hungry and reach for their left chop stick.
- 3. No philosopher can eat (proceed) as none can pick up their right chop stick

Real philosophers in this situation would figure out some way to proceed by putting down a chopstick or two until one or more of their colleagues can eat. We can sometimes do this in our software by using timeouts on blocking operations. When the timeout expires a thread releases the critical section and retries, allowing other blocked threads a chance to proceed. This is not optimal though, as blocked threads hurt performance, and setting timeout values in an inexact science.

It is much better therefore to design a solution to be deadlock free. This means that one or more threads will always be able to make progress. With circular wait deadlocks, this can be achieved by imposing a resource allocation protocol on the shared resources, so that threads will not always request resources in the same order.

In the Dining Philosophers problem, we can do this by making sure one of our philosophers picks up their right chopstick first. Let’s assume we instruct Philosopher 4 to do this. This leads to a possible sequence of operations such as below:

Philosopher 0 picks up left chopstick (chopStick[0]) then right (chopStick[1])
Philosopher 1 picks up left chopstick (chopStick[1]) then right (chopStick[2])
Philosopher 2 picks up left chopstick (chopStick[2]) then right (chopStick[3])
Philosopher 3 picks up left chopstick (chopStick[3]) then right (chopStick[4])
Philosopher 4 picks up right chopstick (chopStick[0]) then left (chopStick[4])

In this example, Philosopher 4 must block, as Philosopher 0 already has acquired access to chopstick[0]. With Philosopher 4 blocked, Philosopher 3 is assured access to chopstick[4] and can then proceed to satisfy their appetite.

The fix for the Dining Philosophers solution is shown in Example 4-10.

## Example 4-10. Solving the Dining Philosophers deadlock

```
if (i == NUMPHILOSOPHERS - 1) {
    // The last philosopher picks up the right fork first
    ph[i] = new Philosopher(rightChopStick, leftChopStick);
} else {
    // all others pick up the left chop stick first
    ph[i] = new Philosopher(leftChopStick, rightChopStick);
}
```

More formally we are imposing an ordering on the acquisition of shared resources, such that:

```
chopStick[0]< chopStick[1]< chopStick[2]< chopStick[3]<
chopStick[4]
```

This means each thread will always attempt to acquire chopstick[0] before chopstick[1], and chopstick[1] before chopstick[2], and so on. For philosopher 4, this means it will attempt to acquire chopstick[0] before chopstick[4], thus breaking the potential for a circular wait deadlock.

Deadlocks are a complicated topic and this section has just scratched the surface. You’ll see deadlocks in many distributed systems. For example, a

user request acquires a lock on some data in a *Students* database table, and must then update rows in the *Classes* table to reflect student attendance. Simultaneously another user request acquires locks on the *Classes* table, and next must update some information in the *Students* table. If these requests interleave such that each request acquires locks in an overlapping fashion, we have a deadlock.

I’ll revisit deadlocks later when discussing concurrent database access and locking in Part 3 of this book.

## Thread States

Multithreaded systems have a system scheduler that decides which threads to run when. In Java, the scheduler is known as a preemptive, priority-based scheduler. In short this means it chooses to execute the highest priority thread which wishes to run.

Every thread has a priority (by default 5, range 0 to 10). A thread inherits its priority from its parent thread. Higher priority threads get scheduled more frequently than lower priority threads, but in most applications having all threads as the default priority suffices.

The scheduler cycles threads through four distinct states, based on their behavior. These are:

*Created*

A thread object has been created but its `start()` method has not been invoked. Once `start()` is invoked, the thread enters the runnable state.

*Runnable*

A thread is able to run. The scheduler will choose which thread(s) to execute in a first-in first-out (FIFO) manner – one thread can be allocated at any time to each core in the node. Threads then execute until they block (e.g. on a `synchronized` statement), execute a

yield(), suspend() or sleep() statement, the run() method terminates, or are preempted by the scheduler. Preemption occurs when a higher priority thread becomes runnable, or when a system-specific time period, known as a time slice, expires. Preemption based on time slicing allows the scheduler to ensure that all threads eventually get a chance to execute – no execution hungry threads can hog the CPU.

# Blocked

A thread is blocked if it is waiting for a lock, a notification event to occur (e.g. sleep timer to expire, resume() method executed), or is waiting for a network or disk request to complete. When the specific event a blocked thread is waiting for occurs, it moves back to the runnable state.

# Terminated

A thread’s run() method has completed or it has called the stop() method. The thread will no longer be scheduled.

An illustration of this scheme is in Figure 4-5. The scheduler effectively maintains FIFO queue in the Runnable state for each thread priority. High priority threads are used typically to respond to events (eg an emergency timer), and execute for a short period of time. Low priority threads are used for background, ongoing tasks like checking for corruption of files on disk through recalculating checksums. Background threads basically use up idle CPU cycles.

![](img/936a72197720a5158b16541313af61c9_109_0.png)

Figure 4-5. Threads states and transitions

## Thread Coordination

There are many problems that require threads with different roles to coordinate their activities. Imagine a collection of threads that each accept documents from users, do some processing on the documents (e.g. generate a pdf), and then send the processed document to a shared printer pool. Each printer can only print one document at a time, so they read from a shared print queue, grabbing and printing documents in the order they arrive.

This printing problem is an illustration of the classic producer-consumer problem. Producers generate and send messages via a shared FIFO buffer to consumers. Consumers retrieve these messages, process them, and then ask for more work from the buffer. A simple illustration of this problem is in Figure 4-6. It’s a bit like a 24 hour, 365 day buffet restaurant - the kitchen keeps producing, the wait staff collect the food and put it in the buffet, and hungry diners help themselves. Forever.

Like virtually all real resources, the buffer has a limited capacity. Producers generate new items, but if the buffer is full, they must wait until some item(s) have been consumed before they can add the new item to the buffer. Similarly, if the consumers are consuming faster than the producers are producing, then they must wait if there are no items in the buffer, and somehow get alerted when new items arrive.
