```
} }\n}) ;
```

The consuming thread runs an infinite loop in which it pops items out of the queue to display them using the `dbg` macro. When the queue is empty, it stops and sleep using the `park` function. If it gets unparked, it starts over and starts popping items from the queue again, until it is empty. And so on.

The producing thread produces a new number every second by pushing it into the queue. Every time it adds an item, it uses the `unpark` method on the `Thread` object that refers to the consuming thread to unpark it. That way, the consuming thread gets woken up to process the new element.

An important observation to make here is that this program would still have been theoretically correct, although inefficient, if we remove parking. This is important, because `park()` does not guarantee that it will only return because of a matching `unpark()`. While somewhat rare, it might have *spurious wake-ups*. Our example deals with that just fine, because the consuming thread will lock the queue, see that it is empty, and directly unlock it and park itself again.

An important property of thread parking is that a call to `unpark()` *before* the thread parks itself does not get lost. The request to unpark is still recorded, and the next time the thread tries to park itself, it clears that request and directly continues without actually going to sleep. To see why that is critical for correct operation, let’s go through a possible ordering of the steps executed by both threads:

+   1. The consuming thread, let’s call that C, locks the queue.
+   2. C tries to pop an item from the queue, but it is empty, resulting in `None`.
+   3. C unlocks the queue.
+   4. The producing thread, which we’ll call P, locks the queue.
+   5. P pushes a new item on to the queue.