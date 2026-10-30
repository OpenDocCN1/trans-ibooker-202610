
fact that when we return Poll::Pending, the next call to poll will start at the top of the function again. We need some way to know what we were in the middle of so that we know what operation to continue on. Furthermore, we need to keep track of different information depending on what we’re doing: if we’re waiting for a receive to finish we need to keep that ReceiveFuture (the definition of which is not shown in this example) around so that we can poll it the next time we are polled ourselves, and similar for SendFuture. The Options here might strike you as weird too; we’ll get back to those shortly.

When we implement Future for Forward, we declare its output type as () ② because this future doesn’t actually return anything. Instead, the future resolves (with no result) when it has finished forwarding everything from the input channel to the output channel. In a more complete example, the Output of our forwarding type might be a Result so that it could communicate errors from receive() and send() back up the stack to the function that’s polling for the completion of the forwarding. But this code is complicated enough already, so we’ll leave that for another day.

When Forward is polled, it needs to resume wherever it last left off, which we find out by matching on the enum variant currently held in self ③. Whichever branch we go into, the first step is to poll the future that blocks progress for the current operation; if we’re trying to receive we poll the ReceiveFuture, and if we’re trying to send we poll the SendFuture. If that call to poll returns Poll::Pending, then we can make no progress, and we return Poll::Pending ourselves. But if the current future resolves, we have work to do!

When one of the inner futures resolves, we need to update what the current operation is by switching which enum variant is stored in self. In order to do so, we have to move out of self to call Receiver::receive or Sender::send—but we can’t do that because all we have is &mut self. So, we store the state we have to move in an Option, which we move out of with Option::take ④. This is silly since we’re about to overwrite self anyway ⑤, and hence the Options will always be Some, but sometimes tricks are needed to make the borrow checker happy.

Finally, if we do make progress, we then poll self again ⑥ so that if we can immediately make progress on the pending send or receive, we do so. This is actually necessary for correctness when implementing the real Future trait, which we’ll get back to later, but for now think of this as an optimization.

We just hand-wrote a state machine: a type that has a number of possible states, and moves between them in response to particular events. This was a fairly simple state machine, at that. Imagine having to write code like this for more complicated use cases where you have additional intermediate steps!

Beyond writing the unwieldy state machine, we have to know the types of the futures that Sender::send and Receiver::receive return so that we can store them in our type. If those methods instead returned impl Future, we’d have no way to write out the types for our variants. The send and receive methods also have to take ownership of the sender and the receiver; if they did not, the lifetimes of the futures they returned would be tied to the

borrow of self, which would end when we return from poll. But that would not work, since we’re trying to store those futures in self.

> NOTE You may have noticed that Receiver looks a lot like an asynchronous version of Iterator. Others have noticed the same thing, and the standard library is on its way to adding a trait specifically for types that can meaningfully implement poll_next. Down the line, these asynchronous iterators (often referred to as streams) may end up with first-class language support, such as the ability to loop over them directly!

Ultimately, this code is hard to write, hard to read, and hard to change. If we wanted to add error handling, for example, the code complexity would increase significantly. Luckily, there’s a better way!

## async/await

Rust 1.39 gave us the async keyword and the closely related await postfix operator, which we used in the original example in Listing 9-3. Together, they provide a much more convenient mechanism for writing asynchronous state machines like the one in Listing 9-5. Specifically, they let you write the code in such a way that it doesn’t even look like a state machine!

```
async fn forward<T>(rx: Receiver<T>, tx: Sender<T>) {
    while let Some(t) = rx.next().await {
        tx.send(t).await;
    }
}
```

Listing 9-5: Implementing a channel-forwarding future using async and await, repeated from Listing 9-3

If you don’t have much experience with async and await, the difference between Listing 9-3 and Listing 9-5 might give you an idea of why the Rust community was so excited to see them land. But since this is an intermediate book, let’s dive a little deeper to understand just how this short segment of code can replace the much longer manual implementation. To do that, we first need to talk about generators—the mechanism by which async and await are implemented.

## Generators

A generator is, briefly described, a chunk of code with some extra compiler-generated bits that enables it to stop, or yield, its execution midway through, and then resume from where it last yielded later on. Take the forward function in Listing 9-3, for example. Imagine that it gets to the call to send, but the channel is currently full. The function can’t make any more progress, but it also cannot block (this is nonblocking code, after all), so it needs to return. Now suppose the channel eventually clears up, and we want to proceed with the send. If we call forward again from the top, it’ll call next again and the item we previously tried to send will be lost, so that’s no good. Instead, we turn forward into a generator.

Whenever the forward generator cannot make progress anymore, it needs to store its current state somewhere so that when its execution eventually resumes, it resumes in the right place with the right state. It saves the state through an associated data structure that’s generated by the compiler, which contains all the state of the generator at a given point in time. A method on that data structure (also generated) then allows the generator to resume from its current state, stored in &mut self, and updates the state again when the generator again cannot make progress.

This “return but allow me to resume later” operation is called yielding, which effectively means it returns while keeping some extra state on the side. When we later want to resume a call to forward, we invoke the known entry point into the generator (the resume method, which is poll for async generators), and the generator inspects the previously stored state in self to decide what to do next. This is exactly the same thing we did manually in Listing 9-4! In other words, the code in Listing 9-3 loosely desugars to the hypothetical code shown in Listing 9-6.

```
generator fn forward<T>(rx: Receiver<T>, tx: Sender<T>) {
    loop {
        let mut f = rx.next();
        let r = if let Poll::Ready(r) = f.poll() { r } else { yield };
        if let Some(t) = r {
            let mut f = tx.send(t);
            let _ = if let Poll::Ready(r) = f.poll() { r } else { yield };
        } else { break Poll::Ready(()); }
    }
}
```

Listing 9-6: Desugaring async/await into a generator

At the time of writing, generators are not actually usable in Rust—they are only used internally by the compiler to implement async/await—but that may change in the future. Generators come in handy in a number of cases, such as to implement iterators without having to carry around a struct, or an impl Iterator that figures out how to yield items one at a time.

If you look closely at Listings 9-3 and 9-5, they may seem a little magical once you know that every await or yield is really a return from the function. After all, there are several local variables in the function, and it’s not clear how they’re restored when we resume later on. This is where the compiler-generated part of generators comes into play. The compiler transparently injects code to persist those variables into and read them from the generator’s associated data structure, rather than the stack, at the time of execution. So if you declare, write to, or read from some local variable a, you are really operating on something akin to self.a. Problem solved! It’s all really quite marvelous.

One subtle but important difference between the manual forward implementation and the async/await version is that the latter can hold references across yield points. This enables functions like Receiver::next and Sender::send in Listing 9-5 to take &mut self rather than the self they took in Listing 9-3. If we tried to use a &mut self receiver for these methods in the manual state
