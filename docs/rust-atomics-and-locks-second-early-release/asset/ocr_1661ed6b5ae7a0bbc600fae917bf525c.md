If we try to compile this, we’re faced with the following error:

```
error[E0277]: `Rc<i32>` cannot be sent between threads safely
    --> src/main.rs:3:5
     |
3   |         thread::spawn(move || {
     |         ^^^^^^^^^^^^^ `Rc<i32>` cannot be sent between
threads safely
     |
     = help: within `[closure@src/main.rs:3:19: 5:6]`, the trait
`Send` is not implemented for `Rc<i32>`
     = note: required because it appears within the type
`[closure@src/main.rs:5:19: 5:6]`
note: required by a bound in `spawn`
```

The thread::spawn function requires its argument to be Send, and a closure is only Send if all of its captures are. If we try to capture something that’s not Send our mistake is caught, protecting us from undefined behavior.

# Locking: Mutexes and RwLocks

The most commonly used tool for sharing (mutable) data between threads is called a mutex, which is short for "mutual exclusion". The job of a mutex is to ensure threads have exclusive access to some data by temporarily blocking other threads that try to access it at the same time.

Conceptually, a mutex has only two operations: lock and unlock, and two states: locked and unlocked. When a thread locks an unlocked mutex, the mutex is marked as locked and the thread can immediately continue. When a thread then attempts to lock an already locked mutex, that operation will block. The thread is put to sleep while it waits for the mutex to be unlocked. Unlocking is only possible on a locked mutex, and should be done by the same thread that locked it. If other threads are waiting to lock the mutex, unlocking will cause one of those threads to be woken up, so it can try to lock the mutex again and continue its course.