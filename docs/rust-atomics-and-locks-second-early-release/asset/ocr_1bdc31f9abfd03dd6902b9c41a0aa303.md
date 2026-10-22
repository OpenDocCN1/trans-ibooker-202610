As a move closure, it moves (or copies) its captures rather than borrowing them, giving it a copy of t. Because it also captures num_done, we’ve changed that variable to be a reference, since we do still want to borrow that same AtomicUsize. Note that the atomic types do not implement the Copy trait, so we’d have gotten an error if we tried to move one into more than one thread.

Closure capture subtleties aside, the change to use fetch_add here is very simple. We don’t know in which order the threads will increment num_done, but as the addition is atomic, we don’t have to worry about anything and can be sure it will be exactly 100 when all threads are done.

# Example: Statistics

Continuing with this concept of reporting what other threads are doing through atomics, let’s extend our example to also collect and report some statistics on the time it takes to process an item.

Next to num_done, we’re adding two atomic variables, total_time and max_time, to keep track of the amount of time spent processing items. We’ll use these to report the average and peak processing times.

```
fn main() {
    let num_done = &AtomicU32::new(0);
    let total_time = &AtomicU64::new(0);
    let max_time = &AtomicU64::new(0);

    thread::scope(|s| {
        // Four background threads to process all 100 items, 25 each.
        for t in 0..4 {
            s.spawn(move || {
                for i in 0..25 {
                    let start = Instant::now();
                    process_item(t * 25 + i);
                    let time_taken = start.elapsed().as_micros()
 as u64;

                    num_done.fetch_add(1, Relaxed);
                    total_time.fetch_add(time_taken, Relaxed);
                    max_time.fetch_max(time_taken, Relaxed);
                }
            }
        }
    }
```