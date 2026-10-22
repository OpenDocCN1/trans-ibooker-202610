## 第 2 章. 原子操作的介绍

### 早期发布读者的说明

使用 Early Release 电子书，您可以在官方发布这些标题之前很久就利用这些技术——作者在写作时的原始未编辑内容。

这将是最终书籍的第 2 章。请注意 GitHub 仓库将在稍后激活。

如果您对如何改进本书的内容和/或示例有任何评论，或者如果您注意到本章中缺少材料，请联系编辑 shiraevans@gmail.com。

单词 *atomic* 来自希腊语 ἄτομος，意思是 *不可分割*，不能被切成更小块的东西。在计算机科学中，它用于描述不可分割的操作：它要么完全完成，要么尚未发生。

如“借用和数据竞争”中所述，多个线程并发读取和修改同一个变量通常会导致未定义行为。然而，原子操作确实允许不同线程安全地读取和修改同一个变量。由于此类操作是不可分割的，它要么完全发生在另一个操作之前，要么完全发生在之后，从而避免未定义行为。稍后，在 X REF HERE FOR CH04 中，我们将看到这在硬件层面上是如何工作的。

原子操作是涉及多个线程的任何事物的主要构建块。所有其他并发原语，例如互斥锁和条件变量，都是使用原子操作实现的。

在 Rust 中，原子操作可通过 `std::sync::atomic` 中标准原子类型的方法获得。它们都以 `Atomic` 开头，例如 `AtomicI32` 或 `AtomicUsize`。可用的类型取决于硬件架构，有时还取决于操作系统，但几乎所有平台至少提供指针大小的所有原子类型。

与大多数类型不同，它们允许通过共享引用进行修改（例如 `&AtomicU8`）。这得益于内部可变性，如“内部可变性”中所述。

每种可用的原子类型都有相同的接口，具有用于存储和加载的方法、用于原子 `fetch and modify` 操作的方法，以及一些更高级的 `compare and exchange` 方法。我们将在本章的其余部分详细讨论它们。

但是，在深入研究不同的原子操作之前，我们需要简要介绍一个称为 *内存排序* 的概念。

### 内存排序

每个原子操作都接受一个类型为 `std::sync::atomic::Ordering` 的参数，它决定了我们对操作相对顺序的保证。最简单、保证最少的变体是 `Relaxed`。`Relaxed` 仍然保证单个原子变量的一致性，但不承诺不同变量之间操作的相对顺序。

这意味着两个线程可能会以不同的顺序看到对不同变量的操作。例如，如果一个线程先写入一个变量，然后很快写入第二个变量，另一个线程可能会看到相反的顺序。

在本章中，我们只会查看这不是问题的情况，并且在不深入细节的情况下到处使用 `Relaxed`。我们将在 X REF HERE FOR CH03 中讨论内存排序的所有细节以及其他可用的内存排序。




### 原子加载与存储

我们接下来要查看的第一个两个原子操作是最基本的操作：加载和存储。它们的函数签名如下：

```
impl AtomicI32 {
    pub fn load(&self, ordering: Ordering) -> i32;
    pub fn store(&self, value: i32, ordering: Ordering);
}
```

`load` 方法会原子地加载原子变量中存储的值，而 `store` 方法会原子地在其中存储一个新值。注意 `store` 方法接受一个共享引用（&T）而不是独占引用（&mut T），即使它修改了值。

让我们来看一下这两种方法的一些实际用例。

### 示例：停止标志

第一个示例使用 `AtomicBool` 作为停止标志。这种标志用于通知其他线程停止运行。

```
fn main() {
    static STOP: AtomicBool = AtomicBool::new(false);

    // Spawn a thread to do the work.
    let background_thread = thread::spawn(|| {
        while !STOP.load(Relaxed) {
            some_work();
        }
    });

    // Use the main thread to listen for user input.
    for line in std::io::stdin().lines() {
        match line.unwrap().as_str() {
            "help" => println!("commands: help, stop"),
            "stop" => break,
            cmd => println!("unknown command: {cmd:?}"),
        }
    }

    // Inform the background thread it needs to stop.
    STOP.store(true, Relaxed);

    // Wait until the background thread finishes.
    background_thread.join().unwrap();
}
```

在这个示例中，后台线程会反复运行 `some_work()`，而主线程允许用户输入一些命令来与程序交互。在这个简单示例中，唯一有用的命令是 `stop` 用于使程序停止。

为了让后台线程停止，使用原子 `STOP` 布尔值来向后台线程传达此条件。当前台线程读取到停止命令时，它将标志设置为 true，后台线程在每次新迭代之前都会检查该标志。主线程使用 `join` 方法等待后台线程完成当前迭代。

只要后台线程定期检查标志，这个简单的解决方案就运作良好。如果 `some_work()` 可能长时间被阻塞，这可能会导致从发出停止命令到程序退出之间出现不可接受的延迟。

### 示例：进度报告

在下一个示例中，我们在后台线程上逐个处理 100 个项目，同时主线程定期向用户提供进度更新。

```
fn main() {
    let num_done = AtomicUsize::new(0);

    thread::scope(|s| {
        // A background thread to process all 100 items.
        s.spawn(|| {
            for i in 0..100 {
                process_item(i);
                num_done.store(i + 1, Relaxed);
            }
        });

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

这次，我们使用了一个作用域线程（“Scoped Threads”），它会自动为我们处理线程的 join，并允许我们借用局部变量。

每当后台线程完成处理一个项目时，它会将已处理的项目数量存储在 `AtomicUsize` 中。与此同时，主线程会将该数字显示给用户以告知进度，大约每秒一次。一旦主线程看到所有 100 个项目都已处理完毕，它就会退出作用域，这会隐式地 join 后台线程，并通知用户一切已完成。

### 同步

一旦最后一个项目被处理，主线程可能需要长达一整秒的时间才知道这一点，从而在末尾引入不必要的延迟。为了解决这个问题，我们可以使用线程挂起（“Thread parking”）在有新信息时唤醒主线程使其从睡眠中恢复。

下面是同一个示例，但现在使用 `thread::park_timeout` 而不是 `thread::sleep`：

```
fn main() {
    let num_done = AtomicUsize::new(0);

    let main_thread = thread::current();

    thread::scope(|s| {
        // A background thread to process all 100 items.
        s.spawn(|| {
            for i in 0..100 {
                process_item(i);
                num_done.store(i + 1, Relaxed);
                main_thread.unpark(); // Wake up the main thread.
            }
        });

        // The main thread shows status updates.
        loop {
            let n = num_done.load(Relaxed);
            if n == 100 { break; }
            println!("Working.. {n}/100 done");
            thread::park_timeout(Duration::from_secs(1));
        }
    });

    println!("Done!");
}
```

变化不大。我们通过 `thread::current()` 获得了主线程的句柄，后台线程现在在每次状态更新后使用它来取消挂起主线程。主线程现在使用 `park_timeout` 而不是 `sleep`，这样它就可以被中断。

现在，任何状态更新都会立即报告给用户，同时仍会每秒重复上一次更新以显示程序仍在运行。

### 示例：延迟初始化

在我们进入更高级的原子操作之前，最后一个示例是关于延迟初始化的。

假设有一个值 x，我们是从文件中读取、从操作系统获取或以其他方式计算的，我们期望它是恒定的。也许 x 是操作系统的版本，或者总内存量，或者 tau 的第 400 位数字。对于这个示例来说并不重要。

由于我们不期望它发生变化，我们可以在第一次需要时才请求或计算它，并记住结果。第一个需要它的线程必须计算该值，但它可以将其存储在原子静态变量中以供所有线程使用，包括如果它以后再次需要时它自己使用。

让我们看一个示例。为了简单起见，我们假设 x 从不为零，这样我们就可以在计算之前使用零作为占位符。

```
fn get_x() -> u64 {
    static X: AtomicU64 = AtomicU64::new(0);
    let mut x = X.load(Relaxed);
    if x == 0 {
        x = calculate_x();
        X.store(x, Relaxed);
    }
    x
}
```

第一个调用 `get_x()` 的线程会检查静态变量 X，看到它仍然为零，计算其值，并将结果写回静态变量以供将来使用。之后，任何对 `get_x()` 的调用都会看到静态变量中的值不为零，并立即返回它而不再次计算。

然而，如果第二个线程在第一个线程仍在计算 x 时调用 `get_x()`，第二个线程也会看到零并并行计算 x。哪个线程先完成，结果就会被另一个线程覆盖。这就是所谓的竞争。不是数据竞争，这在 Rust 中是不确定的行为且在不使用 unsafe 的情况下不可能发生，但仍然是一种具有不可预测获胜者的竞争。

由于我们期望 x 是恒定的，谁赢得竞争并不重要，因为结果无论如何都相同。根据我们期望 `calculate_x()` 花费的时间，这可能是一种非常好或非常糟糕的策略。

如果预计 `calculate_x()` 需要很长时间，最好让线程在第一个线程仍在初始化 x 时等待。你可以使用条件变量或线程挂起（“Waiting: Parking and Condition Variables”）来实现这一点，但对于一个小示例来说这很快就会变得过于复杂。Rust 标准库通过 `std::sync::Once` 和 `std::lazy::SyncOnceCell` 提供了完全相同的功能，因此通常无需自己实现。

### 获取并修改操作

现在我们已经看到了基本加载和存储操作的几个用例，让我们继续介绍更有趣的操作：获取并修改操作。这些操作会修改原子变量，但也会作为一次原子操作加载（获取）原始值。

最常用的有 `fetch_add` 和 `fetch_sub`，分别执行加法和减法。其他可用的操作包括用于位操作的 `fetch_or` 和 `fetch_and`，以及可用于保持运行最大值或最小值的 `fetch_max` 和 `fetch_min`。

它们的函数签名如下：

```
impl AtomicI32 {
    pub fn fetch_add(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_sub(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_or(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_and(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_nand(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_xor(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_max(&self, v: i32, ordering: Ordering) -> i32;
    pub fn fetch_min(&self, v: i32, ordering: Ordering) -> i32;
    pub fn swap(&self, v: i32, ordering: Ordering) -> i32; // "fetch_store"
}
```

唯一的例外是简单地存储新值而不考虑旧值的操作。它没有被命名为 `fetch_store`，而是被命名为 `swap`。

下面是一个快速演示，展示 `fetch_add` 如何返回操作之前的值：

```
let a = AtomicU32::new(100);
let b = a.fetch_add(23, Relaxed);
```




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

### Example: Progress Reporting from Multiple Threads

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

### Example: Statistics

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

### Example: ID Allocation

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





A second way to deal with the overflow is to use `fetch_sub` to decrement the counter again before panicking, like this:

```
fn allocate_new_id() -> u32 {
    static NEXT_ID: AtomicU32 = AtomicU32::new(0);
    let id = NEXT_ID.fetch_add(1, Relaxed);
    if id >= 1000 {
        NEXT_ID.fetch_sub(1, Relaxed);
        panic!("too many IDs!");
    }
    id
}
```

It’s still possible for the counter to very briefly be incremented beyond 1000 when multiple threads execute this function at the same time, it is limited by the amount of active threads. It’s reasonable to assume there will never be billions of active threads at once, especially not all simultaneously executing the same function in the brief moment between `fetch_add` and `fetch_sub`.

This is how overflows are handled for the number of running threads in the standard library’s `thread::scope` implementation.

The third way of handling overflows is arguably the only truly correct one, as it prevents the addition from happening at all if it would overflow. However, we cannot implement that with the atomic operations we’ve seen so far. For this, we’ll need compare-and-exchange operations.

### Compare-and-Exchange Operations

The most advanced and flexible atomic operation is the *compare and exchange* operation. This operation checks if the atomic value is equal to a given value, and only if that is the case will it replace it with a new value, all atomically as a single operation. It will return the previous value and tell us whether it replaced it or not.

Its signature is a bit more complicated than the ones we’ve seen so far:

```
impl AtomicI32 {
    pub fn compare_exchange(
        &self,
        expected: i32,
        new: i32,
        store_order: Ordering,
        load_order: Ordering
    ) -> Result<i32, i32>;
}
```

Ignoring memory ordering for a moment, it is basically identical to the following implementation, except it all happens as a single undividable atomic operation:

```
pub fn compare_exchange(&self, expected: i32, new: i32) -> Result<i32, i32> {
    // In reality, the load, comparison and store,
    // all happen as a single atomic operation.
    let v = self.load();
    if v == expected {
        // Value is as expected. Replace it and report success.
        self.store(new);
        Ok(v)
    } else {
        // The value was not as expected. Leave it untouched and report failure.
        Err(v)
    }
}
```

What this allows us to do, is to load value, perform any calculation we like, and then update the atomic variable to the updated value only if it hasn’t changed in the meantime. If we put this in a loop to retry if it did change, we could use this to implement all the other atomic operations, making this the most generic one.

To demonstrate, let’s increment an AtomicU32 by one without using fetch_add, just to see how compare_exchange is used in practice:

```
fn increment(a: &AtomicU32) {
    let mut current = a.load(Relaxed); ①
    loop {
```

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

Next to compare_exchange, there is also a very similar compare_exchange_weak method. The difference is that the weak version might sometimes leave the value untouched and still return an Err, even though the atomic value matched the expected value. On some platforms, this method can be implemented more efficiently and should be preferred in cases where the consequence of a spurious compare and exchange failure are insignificant, such as in our increment function above. In X REF HERE FOR CH04, we’ll dive into the low level details to find out why the weak version can be more efficient.

### Example: ID Allocation Without Overflow

Now, back to our overflow problem in allocate_new_id() from “Example: ID Allocation”.

To stop incrementing NEXT_ID beyond a certain limit to prevent overflows, we can use compare_exchange to implement atomic addition with an upper bound. Using that idea, let’s make a version of allocate_new_id that always handles overflow correctly, even in the practically impossible situations:

```
fn allocate_new_id() -> u32 {
    static NEXT_ID: AtomicU32 = AtomicU32::new(0);
    let mut id = NEXT_ID.load(Relaxed);
    loop {
        assert!(id < 1000, "too many IDs!");
        match NEXT_ID.compare_exchange_weak(id, id + 1, Relaxed, Relaxed) {
            Ok(_) => return id,
            Err(v) => id = v,
        }
    }
}
```

Now we check and panic before modifying NEXT_ID, guaranteeing it will never be incremented beyond 1000, making overflow impossible. We can now raise the upper limit from 1000 to u32::MAX if we want, without having to worry about edge cases in which it might get incremented beyond the limit.

### Example: Racy Lazy Initialization

In “Example: Lazy Initialization”, we looked at an example of lazy initialization of a constant value. We made a function that lazily initializes a value on the first call, but re-uses it on later calls. When multiple threads run the function concurrently during the first call, more than one thread might execute the initialization, and they will overwrite eachothers result in an unpredictable order.

This is fine for values that we expect to be constant, or when we don’t care about changing values. However, there are also use cases where such a value gets initialized to a different value each time, even though we need

every invocation of the function within a single run of the program to return the same value.

For example, imagine a function get_key() that returns a randomly generated key that’s only generated once per run of the program. It might be an encryption key used for communication with the program, that needs to be unique every time the program is run, but stay constant within a process.

This means we cannot simply use a store operation after generating a key, since that might overwrite a key generated by another thread just moments ago, resulting in two threads using different keys. Instead, we can use compare_exchange to make sure we only store the key if we’re still the first, and otherwise throw our key away and use the stored key instead.

Here’s an implementation of this idea:

```
fn get_key() -> u64 {
    static KEY: AtomicU64 = AtomicU64::new(0);
    let key = KEY.load(Relaxed);
    if key == 0 {
        let new_key = generate_random_key();
        match KEY.compare_exchange(0, new_key, Relaxed, Relaxed)
        {
            Ok(_) => new_key,
            Err(k) => k,
        }
    } else {
        key
    }
}
```

We only generate a new key if KEY was not yet initialized.
We replace KEY by our newly generated key, but only if it is still zero.
If we swapped the zero for our new key, we return our newly generated key. New invocations of get_key() will now return the same new key that’s now stored in KEY.
If we lost the race to another thread initializing KEY before we could, we forget our newly generated key and use the key from KEY instead.

This is a good example of a situation where compare_exchange_weak is not appropriate, and we want to use the strong version instead. We don’t run our compare and exchange operation in a loop, and we don’t want to

return zero if the operation spuriously failed. Even if we did put this in a loop, generating a new key probably takes more than a few simple operations, so risking a spurious failure from `compare_exchange_weak` is simply not worth the small performance benefit it might bring us.

As mentioned in “Example: Lazy Initialization”, if `generate_random_key()` takes a lot of time, it might make more sense to block threads during initialization, to avoid potentially spending time generating keys that will not be used. The Rust standard library provides such functionality through `std::sync::Once` and `std::lazy::SyncOnceCell`.

### Summary

- Atomic operations are *undividable*; they either fully completed, or they haven’t happened yet.
- Atomic operations in Rust are done through the atomic types in `std::sync::atomic`, such as `AtomicI32`.
- Not all atomic types are available on all platforms.
- The relative ordering of atomic operations is tricky when multiple variables are involved. More in X REF HERE FOR CH03.
- Simple loads and stores are nice for very basic inter-thread communication, like stop flags and status reporting.
- Lazy initialization can be done as a *race*, without causing a *data race*.
- Fetch-and-modify operations allow for a small set of basic atomic modifications that are especially useful when multiple threads are modifying the same atomic variable.
- Atomic addition and subtraction silently wrap around on overflow.




- 比较并交换操作是最灵活、最通用的，并且是构建任何其他原子操作的基础构建块。
- 一个 *弱* 的比较并交换操作可能略微更高效。
