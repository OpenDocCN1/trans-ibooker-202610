

```
let c = a.load(Relaxed);

assert_eq!(b, 100);
assert_eq!(c, 123);
```

`fetch_add` 操作将 a 从 100 增加到 123，但返回给我们的是旧值 100。之后的任何操作都会看到 123 的值。

这些操作的返回值并不总是相关。如果只需要对原子值应用操作，而对值本身不感兴趣，那么完全忽略返回值是完全可以的。

需要注意的重要一点是，`fetch_add` 和 `fetch_sub` 对溢出实现了 *wrapping* 行为。递增到超出最大可表示值的值会回绕并导致最小可表示值。这与常规整数的加法和减法运算符的行为不同，后者在调试模式下溢出时会 panic。

在“Compare-and-Exchange Operations”中，我们将看到如何使用溢出检查进行原子加法。

但首先，让我们看看这些方法的一些实际用例。

## Example: Progress Reporting from Multiple Threads

在“Example: Progress Reporting”中，我们使用了一个 `AtomicUsize` 来报告后台线程的进度。如果我们将工作拆分到例如四个线程，每个线程处理 25 个项目，我们就需要知道来自四个线程的进度。

我们可以使用一个单独的 `AtomicUsize` 为每个线程，并在主线程中全部加载并求和，但更简单的解决方案是使用单个 `AtomicUsize` 来跟踪所有线程处理的项目总数。

要使其工作，我们不能再使用 `store` 方法，因为那样会覆盖其他线程的进度。相反，我们可以在每次处理完一个项目后使用原子加操作来递增计数器。

让我们更新“Example: Progress Reporting”中的示例，将工作拆分到四个线程：

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

有一些变化。最重要的是，我们现在生成四个后台线程而不是一个，并且使用 fetch_add 而不是 store 来修改 num_done 原子变量。

更微妙的是，我们现在为后台线程使用 move 闭包，并且 num_done 现在是一个引用。这与我们使用 fetch_add 无关，而是与我们在循环中生成四个线程的方式有关。此闭包捕获 t 以知道它是四个线程中的哪一个，从而确定从项目 0、25、50 还是 75 开始。如果没有 move 关键字，闭包将尝试通过引用捕获 t。这是不允许的，因为它在循环期间只存在很短的时间。

作为 move 闭包，它移动（或复制）其捕获的内容而不是借用它们，从而为其提供 t 的副本。因为它还捕获了 num_done，所以我们将该变量更改为引用，因为我们仍然希望借用同一个 AtomicUsize。请注意，原子类型未实现 Copy trait，因此如果我们尝试将一个移动到多个线程中，我们会收到错误。

撇开闭包捕获的细微差别，使用 fetch_add 的更改在这里非常简单。我们不知道线程会以什么顺序递增 num_done，但由于加法是原子的，我们不必担心任何事情，并且可以确定当所有线程完成时它将恰好为 100。

## Example: Statistics

继续使用通过原子来报告其他线程正在做什么的概念，让我们扩展示例以收集并报告处理一个项目所需时间的统计信息。

除了 num_done，我们还添加了两个原子变量 total_time 和 max_time，以跟踪处理项目所花费的时间。我们将使用这些来报告平均和峰值处理时间。

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
                    let time_taken = start.elapsed().as_micros() as u64;

                    num_done.fetch_add(1, Relaxed);
                    total_time.fetch_add(time_taken, Relaxed);
                    max_time.fetch_max(time_taken, Relaxed);
                }
            });
        }
    }
```

```
});

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

后台线程现在使用 Instant::now() 和 Instant::elapsed() 来测量它们在 process_item() 中花费的时间。原子加操作用于将微秒数添加到 total_time，原子 max 操作用于跟踪 max_time 中的最高测量值。

主线程将总时间除以已处理的项目数以获得平均处理时间，并将其与来自 max_time 的峰值时间一起报告。

由于三个原子变量是分开更新的，主线程可能在线程递增 num_done 之后，但在它更新 total_time 之前加载值，从而导致平均值的低估。更微妙的是，由于 Relaxed 内存排序对从另一个线程看到的操作的相对顺序没有任何保证，它甚至可能在仍看到旧的 num_done 值时短暂地看到新的更新的 total_time 值，从而导致平均值的过高估计。

在我们的示例中，这都不是大问题。最坏的情况是短暂地向用户报告不准确的平均值。

如果我们想避免这种情况，可以将三个统计信息放在 Mutex 中。然后我们在更新三个数字时短暂锁定互斥锁，这些数字本身不再需要是原子的。这有效地将三次更新变成一次原子操作，以换取锁定和解锁互斥锁的成本，以及可能暂时阻塞线程。

## Example: ID Allocation

让我们继续讨论一个用例，在该用例中我们确实需要 fetch_add 的返回值。

假设我们需要一个 allocate_new_id() 函数，每次调用时都提供一个新的唯一数字。我们可能会使用这些数字来标识程序中的任务或其他事物；这些事物需要通过可以轻松存储并在线程之间传递的小东西唯一标识，例如整数。

使用 fetch_add 实现此函数 turns out 非常简单：

```
fn allocate_new_id() -> u32 {
    static NEXT_ID: AtomicU32 = AtomicU32::new(0);
    NEXT_ID.fetch_add(1, Relaxed)
}
```

我们只需跟踪要发放的下一个数字，并在每次加载时递增它。第一个调用者将得到 0，第二个得到 1，以此类推。

这里唯一的问题是溢出时的回绕行为。第 4294967296 次调用将溢出 32 位整数，使得下一次调用将再次返回 0。

这是否是个问题取决于用例：调用这么多次的可能性有多大，如果数字不唯一会发生什么最坏的情况？虽然这似乎是一个巨大的数字，但现代计算机可以在几秒钟内轻松执行我们的函数这么多次。如果内存安全依赖于这些数字的唯一性，我们上面的实现是不可接受的。

为了解决这个问题，我们可以尝试在调用次数过多时让函数 panic，如下所示：

```
// This version is problematic.
fn allocate_new_id() -> u32 {
    static NEXT_ID: AtomicU32 = AtomicU32::new(0);
    let id = NEXT_ID.fetch_add(1, Relaxed);
    assert!(id < 1000, "too many IDs!");
    id
}
```

现在，assert 语句将在一千次调用后 panic。然而，这发生在原子加操作已经发生之后，这意味着 NEXT_ID 在我们 panic 时已经递增到 1001。如果另一个线程随后调用该函数，它将在 panic 之前将其递增到 1002，依此类推。虽然可能需要更长的时间，但我们在 NEXT_ID 再次溢出到零时会在 4294966296 次 panic 后遇到相同的问题。

解决此问题的三种常见方法。第一种是不 panic 而是完全在溢出时中止进程。std::process::abort 函数将中止整个进程，排除了任何继续调用我们函数的可能性。虽然中止进程可能需要短暂的时间，在此期间函数仍可能被其他线程调用，但在程序真正中止之前发生数十亿次的可能性可以忽略不计。

事实上，这就是标准库中 Arc::clone() 的溢出检查的实现方式，以防你以某种方式克隆它 isize::MAX 次。在 64 位计算机上这需要数百年，但在 isize 只有 32 位时可以在几秒钟内实现。

