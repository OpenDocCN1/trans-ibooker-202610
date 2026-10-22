Perhaps surprisingly, this does not happen for a similar if statement, such as in this example:

```
if list.lock().unwrap().pop() == Some(1) {
    do_something();
}
```

Here, the temporary guard does get dropped before the body of the if statement is executed. The reason is that the condition of a regular if statement is always a plain boolean, which cannot borrow anything. There is no reason to extend the lifetime of temporaries from the condition to the end of the statement. For an if let statement, however, that might not be the case. If we had used front() rather than pop() for example, item would be borrowing from the list, making it necessary to keep the guard around. Since the borrow checker is only really a check and does not influence when or in what order things are dropped, the same happens when we use pop(), even though that wouldn’t have been necessary.

We can avoid this by moving the pop operation to a separate let statement. Then the guard is dropped at the end of that statement, before the if let:

```
let item = list.lock().unwrap().pop();
if let Some(item) = item {
    process_item(item);
}
```

# Reader-Writer Lock

A mutex is only concerned with exclusive access. The MutexGuard will provide us an exclusive reference (&mut T) to the protected data, even if we only wanted to look at the data and a shared reference (&T) would have sufficed.