thread to enter will check if heavy is an instance of the HeavyFactory. Since it's not the default Supplier, this thread will replace heavy with an instance of HeavyFactory. Finally, it returns the Heavy instance that this HeavyFactory holds.

The second concurrent thread to enter will again check if heavy is an instance of HeavyFactory, and will bypass the creation. It would simply return the same instance the first thread returned. Here we assume Heavy itself is thread-safe, and we're only focusing on the thread safety of Holder.

We've taken care of the race condition, but since the instance has been created lazily, we no longer need to be so protective. Now that heavy has been replaced with HeavyFactory, subsequent calls to the getHeavy method will go directly to the HeavyFactory's get method and will not incur any synchronization overhead.

We designed lazy initialization and, at the same time, avoided null checks. We also ensured the thread safety of the lazy instance creation. This is a simple, lightweight implementation of the virtual proxy pattern. Next, we'll use lambda expressions to postpone function evaluations.