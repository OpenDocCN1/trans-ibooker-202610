Let’s update the example from “Example: Progress Reporting” to split the work over four threads:

```
fn main() {
    let num_done = &AtomicUsize::new(0);

    thread::scope(|s| {
        // Four background threads to process all 100 items, 25 each.
        for t in 0..4 {
            s.spawn(move || {
                for i in 0..25 {
                    process_item(t * 25 + i);
                    num_done.fetch_add(1, Relaxed);
                }
            });
        }

        // The main thread shows status updates, every second.
        loop {
            let n = num_done.load(Relaxed);
            if n == 100 { break; }
            println!("Working.. {n}/100 done");
            thread::sleep(Duration::from_secs(1));
        }
    });

    println!("Done!");
}
```

A few things have changed. Most importantly, we now spawn four background threads rather than one, and use fetch_add instead of store to modify the num_done atomic variable.

More subtly, we now use a move closure for the background threads, and num_done is now a reference. This is not related to our use of fetch_add, but rather to how we spawn four threads in a loop. This closure captures t to know which of the four threads it is, and thus whether to start at item 0, 25, 50, or 75. Without the move keyword, the closure would try to capture t by reference. That isn’t allowed, as it only exists briefly during the loop.