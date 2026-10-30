
machine implementation, the borrow checker would have no way to enforce that the `Receiver` stored inside `Forward` cannot be referenced between when `Receiver::next` is called and when the future it returns resolves, and so it would reject the code. Only by moving the `Receiver` into the future can we convince the compiler that the `Receiver` is not otherwise accessible. Meanwhile, with async/await, the borrow checker can inspect the code before the compiler turns it into a state machine and verify that `rx` is indeed not accessed again until after the future is dropped, when the await on it returns.

## THE SIZE OF GENERATORS

The data structure used to back a generator’s state must be able to hold the combined state at any one yield point. If your async fn contains, say, a `[u8; 8192]`, those 8 KiB must be stored in the generator itself. Even if your async fn only contains smaller local variables, it must also contain any future that it awaits, since it needs to be able to poll such a future later, when `poll` is invoked.

This nesting means that generators, and thus futures based on async functions and blocks, can get quite large without any visible indicator of that increased size in your code. This can in turn impact your program’s runtime performance, since those giant generators may have to be copied across function calls and in and out of data structures, which amounts to a fair amount of memory copying. In fact, you can usually identify when the size of your generator-based futures is affecting performance by looking for excessive amounts of time spent in the `memcpy` function in your application’s performance profiles!

Finding these large futures isn’t always easy, however, and often requires manually identifying long or complex chains of async functions. Clippy may be able to help with this in the future, but at the time of writing, you’re on your own. When you do find a particularly large future, you have two options: you can try to reduce the amount of local state the async functions need, or you can move the future to the heap (with `Box::pin`), so that moving the future just requires moving the pointer to it. The latter is by far the easiest way to go, but also introduces an extra allocation and a pointer indirection. Your best bet is usually to put the problematic future on the heap, measure your performance, and then use your performance benchmarks to guide you from there.

## Pin and Unpin

We’re not quite done. While generators are neat, a challenge arises from the technique as I’ve described it so far. In particular, it’s not clear what happens if the code in the generator (or, equivalently, the async block) takes a reference to a local variable. In the code from Listing 9-5, the future that `rx.next()` returns must necessarily hold a reference to `rx` if a next message is not immediately available so that it knows where to try again when the generator next resumes. When the generator yields, the future and the reference the future contains get stashed away inside the generator. But what now happens if the generator is moved? Specifically, look at the code in Listing 9-7 which calls `forward`.

```rust
async fn try_forward<T>(rx: Receiver<T>, tx: Sender<T>) -> Option<impl Future>
{
    let mut f = forward(rx, tx);
    if f.poll().is_pending() { Some(f) } else { None }
}
```

*Listing 9-8: Moving a future after polling it*

The `try_forward` function only polls `forward` once, to forward as many messages as possible without blocking. If the receiver may still produce more messages (that is, if it returned `Poll::Pending` instead of `Poll::Ready(None)`), those messages are deferred to be forwarded at some later time by returning the forwarding future to the caller, which may choose to poll again at a time when it sees fit.

Let’s work through what happens here with what we know about async and await so far. When we poll the `forward` generator, it goes through the `while` loop some unknown number of times and eventually returns either `Poll::Ready(())` if the receiver ended, or `Poll::Pending` otherwise. If it returns `Poll::Pending`, the generator contains a future returned from either `rx.next()` or `tx.send(t)`. Those futures both contain a reference to one of the arguments initially provided to `forward` (`rx` and `tx`, respectively), which must also be stored in the generator. But when `try_forward` returns the entire generator, the fields of the generator also move. Thus, `rx` and `tx` no longer reside at the same locations in memory, and the references stored in the stashed-away future are no longer pointing to the right data!

What we’ve run into here is a case of a self-referential data structure: one that holds both data and references into that data. With generators, these self-referential structures are very easy to construct, and being unable to support them would be a significant blow to ergonomics because it would mean you wouldn’t be able to hold references across any yield point. The (ingenious) solution for supporting self-referential data structures in Rust comes in the form of the `Pin` type and the `Unpin` trait. Very briefly, `Pin` is a wrapper type that prevents the wrapped type from being (safely) moved, and `Unpin` is a marker trait that says that the implementing type can be removed safely from a `Pin`.

## Pin

There’s a lot of nuance to cover here, so let’s start with a concrete example of the wrapper `Pin`. In Listing 9-2 I gave you a simplified version of the `Future` trait, but we’re now ready to peel back one part of the simplification. Listing 9-8 shows the `Future` trait somewhat closer to its final form.

```rust
trait Future {
    type Output;
    fn poll(self: Pin<&mut Self>) -> Poll<Self::Output>;
}
```

*Listing 9-8: A less simplified view of the `Future` trait with `Pin`*

In particular, this definition requires that you call `poll` on `Pin<&mut Self>`. Once you have a value behind a `Pin`, that constitutes a contract that that value will never move again. This means that you can construct self-references internally to your heart’s delight, exactly as you want for generators.

> **NOTE** While `Future` makes use of `Pin`, `Pin` is not tied to the `Future` trait—you can use `Pin` for any self-referential data structure.

But how do you get a `Pin` to call `poll`? And how can `Pin` ensure that the contained value won’t move? To see how this magic works, let’s look at the definition of `std::pin::Pin` and some of its key methods, shown in Listing 9-9.

```rust
struct Pin<P> { pointer: P }
impl<P> Pin<P> where P: Deref {
    pub unsafe fn new_unchecked(pointer: P) -> Self;
}
impl<'a, T> Pin<&'a mut T> {
    pub unsafe fn get_unchecked_mut(self) -> &'a mut T;
}
impl<P> Deref for Pin<P> where P: Deref {
    type Target = P::Target;
    fn deref(&self) -> &Self::Target;
}
```

*Listing 9-9: `std::pin::Pin` and its key methods*

There’s a lot to unpack here, and we’re going to have to go over the definition in Listing 9-9 a few times before all the bits make sense, so please bear with me.

First, you’ll notice that `Pin` holds a *pointer type*. That is, rather than hold some `T` directly, it holds a type `P` that dereferences through `Deref` into `T`. This means that rather than have a `Pin<MyType>`, you’ll have a `Pin<Box<MyType>>`, `Pin<Rc<MyType>>`, or `Pin<&mut T>`. The reason for this design is simple—`Pin`’s primary goal is to make sure that once you place a `T` behind a `Pin`, that `T` won’t move, as doing so might invalidate self-references stored in the `T`. If the `Pin` just held a `T` directly, then simply moving the `Pin` would be enough to invalidate that invariant! In the remainder of this section, I’ll refer to `P` as the *pointer* type and `T` as the *target* type.

Next, notice that `Pin`’s constructor, `new_unchecked`, is unsafe. This is because the compiler has no way to actually check that the pointer type indeed promises that the pointed-to (target) type won’t move again. Consider, for example, a variable `foo` on the stack. If `Pin`’s constructor were safe, we could do `Pin::new(&mut foo)`, call a method that requires `Pin<&mut Self>` (and thus assumes that `Self` won’t move again), and then drop the `Pin`. At this point, we could modify `foo` as much as we liked, since it is no longer borrowed—including moving it! We could then pin it again and call the same method, which would be none the wiser that any self-referential pointers it may have constructed the first time around would now be invalid.
