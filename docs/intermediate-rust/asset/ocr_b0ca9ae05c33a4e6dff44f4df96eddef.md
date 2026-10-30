By taking the name of the variable to pin to the stack, the macro ensures that the caller has the value it wants to pin somewhere on the stack already. The shadowing of $var ensures that the caller cannot drop the Pin and continue to use the unpinned value (which would breach the Pin contract for any target type that's !Unpin). By moving the value stored in $var, the macro also ensures that the caller cannot drop the $var binding the macro declares without also dropping the original variable. Specifically, without that line, the caller could write (note the extra scope):

```
let foo = /* */; { pin_mut!(foo); foo.poll() }; foo.mut_self_method();
```

Here, we give a pinned instance of foo to poll, but then we later use a &mut to foo without a Pin, which violates the Pin contract. With the extra reassignment, on the other hand, that code would also move foo into the new scope, rendering it unusable after the scope ends.

Pinning on the stack therefore requires unsafe code, unlike Box::pin, but avoids the extra allocation that Box introduces, and also works in no_std environments.

# Back to the Future

We now have our pinned future, and we know what that means. But you may have noticed that none of this important pinning stuff shows up in most asynchronous code you write with async and await. And that's because the compiler hides it from you.

Remember back when we discussed Listing 9-5, when I told you that <expr>.await desugars into something like:

```
loop { if let Poll::Ready(r) = expr.poll() { break r } else { yield } }
```

That was an ever-so-slight simplification, because as we've seen you can only call Future::poll if you have a Pin<&mut Self> for the future. The desugaring is actually a bit more sophisticated, as shown in Listing 9-12.

```
① match expr {
    mut pinned => loop {
        ② match unsafe { Pin::new_unchecked(&mut pinned) }.poll() {
            Poll::Ready(r) => break r,
            Poll::Pending => yield,
        }
    }
}
```

Listing 9-12: A more accurate de-sugaring of <expr>.await

The match ① is a neat shorthand to ensure that the expansion remains a valid expression, while also moving the expression result into a variable that we can then pin on the stack. Beyond that, the main new addition is the call to Pin::new_unchecked ②. That call is safe because for the containing async block to be polled, it must already be pinned due to the signature