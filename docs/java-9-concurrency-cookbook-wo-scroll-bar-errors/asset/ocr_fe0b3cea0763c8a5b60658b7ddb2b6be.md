```
for (int i=0; i<numberThreads; i++) {
    try {
        threads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
end=new Date();

System.out.printf("Main: Lock results: %d\n",
(end.getTime()-begin.getTime()));
```

14. Launch the specified number of threads to execute the TaskAtomic object. Calculate and write its execution time in the console:

```
begin=new Date();
for (int i=0; i<numberThreads; i++) {
    threads[i]=new Thread(atomicTask);
    threads[i].start();
}

for (int i=0; i<numberThreads; i++) {
    try {
        threads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
end=new Date();

System.out.printf("Main: Atomic results: %d\n",
(end.getTime()-begin.getTime()));
```

# How it works...

When you execute the example, you will see how the execution time of the TaskAtomic tasks that use atomic variables are always better than the TaskLock tasks that use locks. You will obtain a similar result if you use the synchronized keyword instead of locks.

The conclusion of this recipe is that utilization of atomic variables will give you better performance than other synchronization methods. If you don't have an atomic type that fits your needs, maybe you can try to implement your own atomic type.