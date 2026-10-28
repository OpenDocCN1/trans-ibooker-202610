14. Create a Lock object named lock, a Task1 object named task1, a Task2 object named task2, and an array of 10 threads:

```
Lock lock=new ReentrantLock();
Task1 task1=new Task1(lock);
Task2 task2=new Task2(lock);
Thread threads[]=new Thread[10];
```

15. Launch 10 threads to execute the first task by controlling its execution time:

```
Date begin, end;

begin=new Date();
for (int i=0; i<threads.length; i++) {
    threads[i]=new Thread(task1);
    threads[i].start();
}

for (int i=0; i<threads.length; i++) {
    try {
        threads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
end=new Date();
System.out.printf("Main: First Approach: %d\n", (end.getTime()-begin.getTime()));
```

16. Launch 10 threads to execute the second task by controlling its execution time:

```
begin=new Date();
for (int i=0; i<threads.length; i++) {
    threads[i]=new Thread(task2);
    threads[i].start();
}

for (int i=0; i<threads.length; i++) {
    try {
        threads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
end=new Date();
System.out.printf("Main: Second Approach: %d\n", (end.getTime()-begin.getTime()));
```