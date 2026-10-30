of Future::poll. And the async block was polled for us to reach the call to Pin::new_unchecked, so the generator state is pinned. Since pinned is stored in the generator that corresponds to the async block (it must be so that yield will resume correctly), we know that pinned will not move again.
Furthermore, pinned is not accessible except through a Pin once we’re in the loop, so no code is able to move out of the value in pinned. Thus, we meet all the safety requirements of Pin::new_unchecked, and the code is safe.

# Going to Sleep

We went pretty deep into the weeds with Pin, but now that we’re out the other side, there is another issue around futures that may have been making your brain itch. If a call to Future::poll returns Poll::Pending, you need something to call poll again at a later time to check whether you can make progress yet. That something is usually called the executor. Your executor could be a simple loop that polls all the futures you are waiting on until they’ve all returned Poll::Ready, but that would burn a lot of CPU cycles you could probably have used for other, more useful things, like running your web browser. Instead, we want the executor to do whatever useful work it can do, and then go to sleep. It should stay asleep until one of the futures can make progress, and only then wake up to do another pass, before going to sleep again.

# Waking Up

The condition that determines when to check back with a given future varies widely. It might be “when a network packet arrives on this port,” “when the mouse cursor moves,” “when someone sends on this channel,” “when the CPU receives a particular interrupt,” or even “after this much time has passed.” On top of that, developers can write their own futures that wrap multiple other futures, and thus may have several wake-up conditions. Some futures may even introduce their own entirely custom wake events.
To accommodate these many use cases, Rust introduces the notion of a Waker: a way to wake the executor to signal that progress can be made. The Waker is what makes the whole machinery around futures work. The executor constructs a Waker that integrates with the mechanism the executor uses to go to sleep, and passes the Waker in to any Future it polls. How? With the additional parameter to Future::poll that I’ve hidden from you so far. Sorry about that. Listing 9-13 gives the final and true definition for Future—no more lies!

```
trait Future {
    type Output;
    fn poll(self: Pin<&mut Self>, cx: &mut Context<' _>) -> Poll<Self::Output>;
}
```

Listing 9-13: The actual Future trait with Context