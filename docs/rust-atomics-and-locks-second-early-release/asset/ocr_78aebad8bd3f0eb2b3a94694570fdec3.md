```
let new = current + 1;
match a.compare_exchange(current, new, Relaxed, Relaxed)
{
    Ok(_) => return,
    Err(v) => current = v,
}
```

First, we load the current value of a.
We calculate the new value we want to store in a, not taking into account potential concurrent modifications of a by other threads.
We use compare_exchange to update the value of a, but only if its value is still the same value we loaded before.
If a was indeed still the same as before, it is now replaced by our new value and we are done.
If a was not the same as before, another thread must’ve changed it in the brief moment since we loaded it. The compare_exchange operation gives us the changed value that a had, and we’ll try again using that value instead. The brief moment between loading and updating is so short, that it’s unlikely for this to loop more than a few iterations.

Next to compare_exchange, there is also a very similar compare_exchange_weak method. The difference is that the weak version might sometimes leave the value untouched and still return an Err, even though the atomic value matched the expected value. On some platforms, this method can be implemented more efficiently and sould be preferred in cases where the consequence of a spurious compare and exchange failure are insignificant, such as in our increment function above. In X REF HERE FOR CH04, we’ll dive into the low level details to find out why the weak version can be more efficient.

# Example: ID Allocation Without Overflow

Now, back to our overflow problem in in allocate_new_id() from “Example: ID Allocation”.