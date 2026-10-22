behavior.

# Cell

A Cell<T> simply wraps a T, but allows mutations through a shared reference. To avoid undefined behavior, it only allows you to copy the value out (if T is Copy), or replace it with another value as a whole. In addition, it can only be used within a single thread.

Let’s take a look at similar example as in the previous section, but this time using Cell<i32> instead of i32:

```
fn f(a: &Cell<i32>, b: &Cell<i32>) {
    let before = a.get();
    b.set(b.get() + 1);
    let after = a.get();
    if before != after {
        x(); // might happen
    }
}
```

Unlike last time, it is now possible for the if condition to be true. Because a Cell<i32> has interior mutability, the compiler can no longer assume its value won’t change as long as we have a shared reference to it. Both a and b might refer to the same value, such that mutating through b might affect a as well. It may still assume, however, that no other threads are accessing the cells concurrently.

The restrictions on a Cell are not always easy to work with. Since it can’t directly let us borrow the value it holds, we need to move a value out (leaving something in its place), modify it, then put it back, to mutate its contents.

```
fn f(v: &Cell<Vec<i32>>) {
    let mut v2 = v.take(); // Replaces the contents of the Cell with an empty Vec
    v2.push(1);
    v.set(v2); // Put the modified Vec back
}
```