```
});
}

// The main thread shows status updates, every second.
loop {
    let total_time =
Duration::from_micros(total_time.load(Relaxed));
    let max_time =
Duration::from_micros(max_time.load(Relaxed));
    let n = num_done.load(Relaxed);
    if n == 100 { break; }
    if n == 0 {
        println!("Working.. nothing done yet.");
    } else {
        println!(
            "Working.. {n}/1000 done, {:?} average, {:?} peak",
            total_time / n,
            max_time,
        );
    }
    thread::sleep(Duration::from_secs(1));
}
});

println!("Done!");
}
```

The background threads now use Instant::now() and Instant::elapsed() to measure the time they spend in process_item(), An atomic add operation is used to add the amount of microseconds to total_time, and an atomic max operation is used to keep track of the highest measurement in max_time.

The main thread divides the total time by the number of processed items to obtain the average processing time, and reports that together with the peak time from max_time.

Since the three atomic variables are updated separately, it is possible for the main thread to load the values after a thread has incremented num_done, but before it has updated total_time, resulting in an underestimate of the average. More subtly, because the Relaxed memory ordering gives no