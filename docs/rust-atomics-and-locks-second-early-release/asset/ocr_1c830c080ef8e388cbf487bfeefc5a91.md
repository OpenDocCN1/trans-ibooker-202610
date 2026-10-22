```
thread::scope(|s| {
    s.spawn(|| {
        loop {
            let mut q = queue.lock().unwrap();
            let item = loop {
                if let Some(item) = q.pop_front() {
                    break item;
                } else {
                    q = not_empty.wait(q).unwrap();
                }
            };
            drop(q);
            dbg!(item);
        }
    });

    for i in 0.. {
        queue.lock().unwrap().push_back(i);
        not_empty.notify_one();
        thread::sleep(Duration::from_secs(1));
    }
});
```

We had to change a few things:

+   - We now not only have a Mutex containing the queue, but also a Condvar to communicate the not empty condition through.
+   - We no longer need to know which thread to wake up, so we don’t store the return value from spawn anymore. Instead, we notify the consumer through the condition variable with the notify_one method.
+   - Unlocking, waiting, and re-locking is all done by the wait method. We had to restructure the control flow a bit to be able to pass the guard to the wait method, while still dropping it before processing an item.

Now we can spawn as many consuming threads as we like, and even spawn more later, without having to change anything. The condition variable takes care of delivering the notifications to whichever thread is interested.