# OUTPUT LOCKING

The `println` macro uses `io::Stdout::lock()` to make sure its output does not get interrupted. A `println!()` expression will wait until any concurrently running one is finished before writing any output. If this was not the case, we could’ve gotten more interleaved output such as:

```
Hello fromHello from another thread!
  another This is my threthreadHello fromthread id: ThreadId!
( the main thread.
2)This is my thread
id: ThreadId(3)
```

Rather than passing the name of a function to `thread::spawn`, as in our example above, it’s far more common to pass it a *closure*. This allows us to capture values to move into the new thread:

```
let numbers = vec![1, 2, 3];

thread::spawn(move || {
    for n in numbers {
        println!("{n}");
    }
});
```

Here, ownership of `numbers` is transferred to the newly spawned thread. We have to use a `move` closure to transfer ownership. Otherwise, the closure would capture `numbers` by reference even though the new thread might outlive it, resulting in a compiler error.

Since a thread might run until the very end of the program’s execution, the `spawn` function has a `'static` bound on its argument type. In other words, it only accepts functions that may be kept around forever. A closure capturing a local variable by reference may not be kept around forever, since that reference would become invalid the moment the local variable ceases to exist.