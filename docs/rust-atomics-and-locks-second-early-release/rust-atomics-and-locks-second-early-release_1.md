## Chapter 1. Basics of Rust Concurrency

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 1st chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at shiraevans@gmail.com.

Long before multi-core processors were commonplace, operating systems allowed for a single computer to run many programs concurrently. This is achieved by rapidly switching between processes, allowing each to repeatedly make a little bit of progress, one by one. Nowadays, virtually all our computers and even our phones and watches have processors with multiple cores, which can truly execute multiple processes in parallel.

Operating systems isolate processes from each other as much as possible, allowing a program to do its thing while completely unaware of what any other processes are doing. For example, a process can not normally access the memory of another process, or communicate with it in any way without asking the operating system’s kernel first.

However, a program can spawn extra *threads of execution*, as part of the same *process*. Threads within the same process are not isolated from each other. Threads share memory, and can interact with each other through that memory.

This chapter will explain how threads are spawned in Rust, and all the basic concepts around them, such as how to safely share data between multiple threads. The concepts explained in this chapter are foundational to the rest of the book.

> NOTE
> If you’re already familiar with these parts of Rust, feel free to skip ahead. However, before you continue to the next chapters, make sure you have a good understanding of threads, interior mutability, Send and Sync, and know what a mutex, a condition variable, and thread parking is.

### Threads in Rust

Every program starts with exactly one thread: the main thread. This thread will execute your main function, and can be used to spawn more threads if necessary.

In Rust, new threads are spawned using the thread::spawn function from the standard library. It takes a single argument: the function the new thread will execute. The thread stops once this function returns.

Let’s take a look at an example:

```
use std::thread;

fn main() {
    thread::spawn(f);
    thread::spawn(f);

    println!("Hello from the main thread.");
}

fn f() {
    println!("Hello from another thread!");
    let id = thread::current().id();
    println!("This is my thread id: {id:?}");
}
```

We spawn two threads that will both execute f as their main function. Both of these threads will print a message and show their thread id, while the main thread will also print its own message.

### THREAD ID

The Rust standard library assigns every thread a unique identifier. This identifier is accessible through Thread::id() and is of the type ThreadId. There’s not much you can do with a ThreadId other than copying it around and checking for equality. There is no guarantee that these IDs will be assigned consecutively; only that they will be different for each thread.

If you run our example program above several times, you might notice the output varies between runs. This is the output I got on my machine during one particular run:

```
Hello from the main thread.
Hello from another thread!
This is my thread id:
```

Surprisingly, part of the output seems to be missing.

What happened here, is that the main thread finished executing the main function before the newly spawned threads finished executing their function.

Returning from main will exit the entire program, even if other threads are still running.

In this particular example, one of the newly spawned threads had just enough time to get to halfway through the second message, before the program was shut down by the main thread.

If we want to make sure the threads are finished before we return from main, we can wait for them by joining them. To do so, we have to use the JoinHandle returned by the spawn function:

```
fn main() {
    let t1 = thread::spawn(f);
    let t2 = thread::spawn(f);

    println!("Hello from the main thread.");

    t1.join().unwrap();
    t2.join().unwrap();
}
```

The .join() method will wait until the thread has finished executing, and returns a thread::Result. If the thread did not successfully finish its function because it panicked, this will contain the panic message.

Running this version of our program will no longer result in truncated output:

```
Hello from the main thread.
Hello from another thread!
This is my thread id: ThreadId(3)
Hello from another thread!
This is my thread id: ThreadId(2)
```

The only thing that still changes between runs is the order in which the messages are printed:

```
Hello from the main thread.
Hello from another thread!
Hello from another thread!
This is my thread id: ThreadId(2)
This is my thread id: ThreadId(3)
```

### OUTPUT LOCKING

The `println` macro uses `io::Stdout::lock()` to make sure its output does not get interrupted. A `println!()` expression will wait until any concurrently running one is finished before writing any output. If this was not the case, we could’ve gotten more interleaved output such as:

```
Hello fromHello from another thread!
  another This is my threthreadHello fromthread id: ThreadId!
( the main thread.
2)This is my thread
id: ThreadId(3)
```

Rather than passing the name of a function to `thread::spawn`, as in our example above, it’s far more common to pass it a *closure*. This allows us to capture values to move into the new thread:

```
let numbers = vec![1, 2, 3];

thread::spawn(move || {
    for n in numbers {
        println!("{n}");
    }
});
```

Here, ownership of `numbers` is transferred to the newly spawned thread. We have to use a `move` closure to transfer ownership. Otherwise, the closure would capture `numbers` by reference even though the new thread might outlive it, resulting in a compiler error.

Since a thread might run until the very end of the program’s execution, the `spawn` function has a `'static` bound on its argument type. In other words, it only accepts functions that may be kept around forever. A closure capturing a local variable by reference may not be kept around forever, since that reference would become invalid the moment the local variable ceases to exist.





从线程中获取返回值只需从闭包中直接返回即可。该返回值可从 `join` 方法返回的 `thread::Result` 中获得：

```
let numbers = Vec::from_iter(0..=1000);

let t = thread::spawn(move || {
    let len = numbers.len();
    let sum = numbers.into_iter().sum::<usize>();
    sum / len ①
});

let average = t.join().unwrap(); ②

println!("average: {average}");
```

这里，线程闭包返回的值 (1) 通过 `join` 方法 (2) 发送回主线程。

如果 `numbers` 为空，线程在尝试除以零时就会 panic (1)，而 `join` 会返回该 panic 信息，导致主线程因 `unwrap` (2) 也 panic。

### THREAD BUILDER

`thread::spawn` 函数实际上只是 `thread::Builder::new().spawn().unwrap()` 的便捷简写。

`thread::Builder` 允许你在 spawn 线程前为新线程设置一些配置。你可以用它来配置新线程的栈大小并为新线程命名。线程的名称可通过 `thread::current().name()` 获取，会在 panic 消息中使用，并在大多数平台的监控和调试工具中可见。

此外，Builder 的 `spawn` 方法返回一个 `io::Result`，允许你在 spawn 新线程失败时进行处理。这可能发生在操作系统内存耗尽时，或对程序施加了资源限制时。`thread::spawn` 函数如果无法 spawn 新线程则会直接 panic。

### Scoped Threads

#### WARNING

截至 2022 年 6 月，作用域线程尚未成为 Rust 标准库的一部分。它已被提议并作为不稳定特性实现，可在 Rust 的 nightly 版本中使用 `#![feature(scoped_threads)]`。

如果我们确定 spawn 的线程绝对不会超出某个作用域的生命周期，那么该线程就可以安全地借用不会永久存活的东西，例如局部变量，只要它们在该作用域内存活即可。

Rust 标准库提供了 `thread::scope` 函数来 spawn 这种作用域线程。它允许我们 spawn 无法超出我们传递给该函数的闭包作用域的线程，从而可以安全地借用局部变量。

其工作原理最好通过示例来说明：

```
let numbers = vec![1, 2, 3];

thread::scope(|s| {
    s.spawn(|| {
        println!("length: {}", numbers.len());
    });
    s.spawn(|| {
        for n in &numbers {
            println!("{n}");
        }
    });
});
```

我们使用一个闭包调用 `thread::scope` 函数。我们的闭包会直接执行并获得一个参数 `s`，它代表作用域。我们可以使用 `s` 来 spawn 线程，这些线程可以借用像 `numbers` 这样的局部变量。

当作用域结束时，所有尚未被 join 的线程都会自动被 join。

该模式保证了在作用域中 spawn 的线程都不会超出作用域。因为这样，作用域 spawn 方法在其参数类型上没有 `'static` 限制，允许我们引用任何只要在作用域内存活的东西，例如 `numbers`。

在上面的示例中，两个新线程并发访问 `numbers`。这是可以的，因为它们（或主线程）都不修改它。如果我们将第一个线程改为修改 `numbers`，如下所示，编译器将不允许我们 spawn 另一个也使用 `numbers` 的线程：

```
let mut numbers = vec![1, 2, 3];

thread::scope(|s| {
    s.spawn(|| {
        numbers.push(1);
    });
```

```
s.spawn(|| {
    numbers.push(2); // Error!
});
});

error[E0499]: cannot borrow `numbers` as mutable more than once at a time
  --> example.rs:7:13
   |
4 |     s.spawn(|| {
   |     -- first mutable borrow occurs here
5 |     numbers.push(1);
   |     ------ first borrow occurs due to use of `numbers`
in closure
   |
7 |     s.spawn(|| {
   |     ^^ second mutable borrow occurs here
8 |     numbers.push(2);
   |     ------ second borrow occurs due to use of `numbers`
in closure
```

### THE LEAKPOCALYPSE

在 Rust 1.0 之前，标准库有一个名为 `thread::scoped` 的函数，它会直接 spawn 一个线程，就像 `thread::spawn` 一样。它允许非 `'static` 捕获，因为它返回的不是 `JoinHandle`，而是 `JoinGuard`，该 guard 在被 drop 时会 join 线程。任何被借用的数据只需在该 `JoinGuard` 存活期间存活即可。这似乎是安全的，只要 `JoinGuard` 最终会被 drop。

就在 Rust 1.0 发布前夕，人们逐渐意识到无法保证某物一定会被 drop。有很多方式，例如引用计数节点的循环，会导致我们忘记某物而不 drop 它。

最终，在一些人所称的“泄漏末日”中，`thread::scoped` 被移除，并且 `mem::forget` 被设为 *safe*，以强调忘记（或泄漏）某物始终是可能的，并且不能依赖 `Drop` 来保证正确性。

直到很久以后，在 Rust 1.63 中，才添加了一个新的 `thread::scoped` 函数，采用了新的设计，不再依赖 `Drop` 来保证正确性。

### Shared Ownership and Reference Counting

到目前为止，我们已经研究了使用 `move` 闭包将值的所有权转移给线程（“Rust 中的线程”）以及从更长寿命的父线程借用数据（“作用域线程”）。当在两个线程之间共享数据且无法保证任一线程比另一个更长寿时，它们都不能拥有该数据。它们之间共享的任何数据都需要与寿命最长的线程一样长。

### Statics

创建不由单个线程拥有的东西有几种方法。最简单的一种是 `static` 值，它由整个程序 *拥有*，而不是单个线程。在下面的示例中，两个线程都可以访问 X，但它们都不拥有它：

```
static X: [i32; 3] = [1, 2, 3];

thread::spawn(|| dbg!(&X));
thread::spawn(|| dbg!(&X));
```

`static` 项具有常量初始化器，从不被 drop，并且在程序的主函数甚至开始之前就已存在。每个线程都可以借用它，因为它保证始终存在。

### Leaking

另一种共享所有权的方式是通过 *泄漏* 分配。使用 `Box::leak`，可以释放 Box 的所有权，承诺永远不 drop 它。从那时起，该 Box 将永远存活，没有所有者，允许任何线程在程序运行期间随时借用它。

```
let x: &'static [i32; 3] = Box::leak(Box::new([1, 2, 3]));

thread::spawn(move || dbg!(x));
thread::spawn(move || dbg!(x));
```

`move` 闭包可能看起来像我们将所有权移入线程，但仔细查看 `x` 的类型会发现我们只是在给线程 *一个引用* 到数据。

> TIP
> 
> 引用是 `Copy` 的，这意味着当你 `move` 它们时，原来的引用仍然存在，就像整数或布尔值一样。

注意 `'static` 生命周期并不意味着该值自程序开始就已存在，而只是意味着它会存活到程序结束。过去 simply 与之无关。

泄漏 Box 的缺点是我们泄漏了内存。我们分配了某些东西，但从未 drop 并释放它。如果这种情况只发生有限次数，这是可以的。但如果我们一直这样做，程序会慢慢耗尽内存。

### Reference Counting

为了能够 drop 并释放某些东西，我们不能完全放弃其所有权。相反，我们可以共享所有权。通过跟踪所有者的数量，我们可以确保只有在没有所有者 left 时才 drop 该值。

Rust 标准库通过 `Rc` 类型提供了此功能，`Rc` 是 “Reference Counted” 的缩写。它与 `Box` 非常相似，只是克隆它不会分配新东西，而是增加存储在包含值旁边的计数器。原始的和克隆的 `Rc` 都引用相同的分配；它们共享所有权。

```
let a = Rc::new([1, 2, 3]);
let b = a.clone();

assert_eq!(a.as_ptr(), b.as_ptr()); // Same allocation!
```

Drop 一个 `Rc` 会递减计数器。只有最后一个 `Rc` 会看到计数器降到零，并且会是 drop 并释放包含数据的那个。

如果我们尝试将一个 `Rc` 发送到另一个线程，我们会遇到以下编译器错误：

```
error[E0277]: `Rc` cannot be sent between threads safely
  |
8 |     thread::spawn(move || dbg!(b));
  |                    ^^^^^^^^^^^^^^^
```




事实证明，Rc 不是线程安全的（“Thread Safety: Send and Sync” 中会详细介绍）。如果多个线程拥有指向同一分配的 Rc，它们可能会同时尝试修改引用计数，这会导致不可预测的结果。

相反，我们可以使用 Arc，它代表 "Atomically Reference Counted"。它与 Rc 完全相同，只是它保证对引用计数的修改是不可分割的——原子的——操作，使其可以安全地与多个线程一起使用。（第 2 章会详细介绍。）

```
let a = Arc::new([1, 2, 3]); ①
let b = a.clone(); ②

thread::spawn(move || dbg!(a)); ③
thread::spawn(move || dbg!(b)); ③
```

① 我们将一个数组放入新的分配中，并附带一个引用计数器，其初始值为一。

② 克隆 Arc 会将引用计数增加到二，并为我们提供指向同一分配的第二个 Arc。

③ 两个线程都通过自己的 Arc 访问共享数组。两个线程在丢弃自己的 Arc 时都会递减引用计数。最后一个丢弃其 Arc 的线程会看到计数降为零，并将是丢弃并释放数组的线程。

### NAMING CLONES

必须为 Arc 的每个克隆赋予不同的名称，很快就会使代码变得混乱且难以跟踪。虽然 Arc 的每个克隆都是一个单独的对象，但每个克隆都代表相同的共享值，这并没有通过为每个克隆赋予不同的名称来体现。

Rust 允许（并鼓励）你通过定义同名的新变量来*遮蔽*变量。如果在同一作用域中这样做，则原始变量将无法再命名。但通过打开新的作用域，像 `let a = a.clone();` 这样的语句就可以在该作用域内重用相同的名称，同时在作用域之外保留原始变量。

通过用 { } 将闭包包装在新的作用域中，我们可以在将变量移入闭包之前克隆它们，而无需重命名它们。

```
let a = Arc::new([1, 2, 3]);

let b = a.clone();

thread::spawn(move || {
    dbg!(b);
});

dbg!(a);
```

```
let a = Arc::new([1, 2,
3]);

thread::spawn({
    let a = a.clone();
    move || {
        dbg!(a);
    }
});

dbg!(a);
```

Arc 的克隆位于同一作用域。
每个线程都获得一个具有不同名称的克隆。

Arc 的克隆位于不同的作用域。
我们可以在每个线程中使用相同的名称。

由于所有权是共享的，引用计数指针（`Rc<T>` 和 `Arc<T>`）具有与共享引用（`&T`）相同的限制。它们不会提供对其包含值的可变访问，因为该值可能同时被其他代码借用。

```
error[E0596]: cannot borrow data in an `Arc` as mutable
  |
6 |     a.sort();
  |     ^^^^^^^^
```

### Borrowing and Data Races

在 Rust 中，值可以通过两种方式借用：

*Immutable Borrowing*

使用 `&` 借用某物会得到一个*不可变引用*。此类引用可以被复制。对其所引用数据的访问在该引用的所有副本之间共享。正如名称所示，编译器通常不允许你通过此类引用*变异*某物，因为这可能会影响当前借用相同数据的其他代码。

*Mutable Borrowing*

使用 `&mut` 借用某物会得到一个*可变引用*。可变借用保证它是该数据唯一活动的借用，从而确保变异数据不会改变其他代码当前正在查看的任何内容。

这两种概念共同完全防止了*数据竞争*：一种线程在变异数据而另一线程并发访问该数据的情况。数据竞争通常是*未定义行为*，这意味着编译器无需考虑这些情况。它只需假设它们不会发生。

为了说明这意味着什么，让我们看一个例子：

```
fn f(a: &i32, b: &mut i32) {
    let before = *a;
    *b += 1;
    let after = *a;
    if before != after {
        x(); // never happens
    }
}
```

在这里，我们获得对整数的不可变引用，并存储该整数在递增 b 所指的整数之前和之后的值。编译器可以自由假设关于借用和数据竞争的基本规则得到维护，这意味着 b 不可能指向与 a 相同的整数。事实上，只要 a 正在借用它，整个程序中就没有任何东西可以可变地借用 a 所指的整数。因此，它可以很容易地得出结论：*a 将不会改变，if 语句的条件永远不会为真，并且可以作为优化完全删除对 x 的调用。

除了使用 unsafe 块来禁用编译器的某些安全检查之外，不可能编写破坏编译器假设的 Rust 程序。

### UNDEFINED BEHAVIOR

像 C、C++ 和 Rust 这样的语言有一组需要遵循的规则，以避免称为*未定义行为*的东西。例如，Rust 的一条规则是任何对象上永远不能有超过一个可变引用。

在 Rust 中，只有在使用 `unsafe` 代码时才有可能违反这些规则中的任何一条。那个词并不意味着代码不正确或永远不安全，而是指编译器不会为你验证代码是否安全。如果代码确实违反了这些规则，则称为* unsound*。

编译器可以假设这些规则永远不会被破坏，而无需检查。当被破坏时，这会导致所谓的*未定义行为*，我们必须不惜一切代价避免。如果我们允许编译器做出实际上不成立的假设，它很容易对代码的不同部分得出更多错误的结论，影响整个程序。

作为一个具体的例子，让我们看一下使用切片上的 `get_unchecked` 方法的小片段。

```
let a = [123, 456, 789];
let b = unsafe { a.get_unchecked(index) };
```

`get_unchecked` 方法通过其索引为我们提供切片的一个元素，就像 a[`index`] 一样，但允许编译器假设索引始终在范围内，而无需任何检查。

这意味着在此代码片段中，因为 a 的长度为 3，编译器可能假设 `index` 小于三。确保其假设成立是我们的工作。

如果我们破坏了这一假设，例如如果我们使用等于 3 的 `index` 运行此代码，可能会发生任何事情。它可能导致从 a 之后内存中存储的字节中读取。它可能导致

程序崩溃。它可能最终执行程序中完全不相关的部分。它可能导致各种混乱。

也许令人惊讶的是，未定义行为甚至可以*倒流回过去*，导致其之前的代码出现问题。要了解这如何发生，想象我们在之前的片段之前有一个 `match` 语句，如下所示：

```
match index {
    0 => x(),
    1 => y(),
    _ => z(index),
}

let a = [123, 456, 789];
let b = unsafe { a.get_unchecked(index) };
```

由于不安全代码，编译器可以假设 `index` 永远只有 0、1 或 2。它可能会逻辑上得出结论，我们的 `match` 语句的最后一个分支只会匹配 2，因此 `z` 只会被调用为 `z(2)`。这一结论可能不仅用于优化 `match`，还用于优化 `z` 本身。这可能包括丢弃未使用的代码部分。

如果我们使用 3 的 `index` 执行此操作，我们的程序可能尝试执行已被优化掉的部分，导致在到达最后一行的 `unsafe` 块之前就出现完全不可预测的情况。就像这样，未定义行为可以通过整个程序向后和向前传播，通常以非常出乎意料的方式。

在调用任何 `unsafe` 函数时，请仔细阅读其文档，并确保你完全理解其*安全要求*：作为调用者，你需要维护的假设，以避免未定义行为。

### Interior Mutability

上一节中介绍的借用规则很简单，但可能相当有限。尤其是在涉及多个线程时。遵循这些规则使得线程之间的通信极其有限，几乎不可能，因为任何可被多个线程访问的数据都不能被变异。

幸运的是，有一个逃生舱口：称为*内部可变性*。具有内部可变性的数据类型稍微弯曲了借用规则。在某些条件下，这些类型可以通过“不可变”引用允许变异。

在“Shared Ownership and Reference Counting”中，我们已经看到了涉及内部可变性的一个微妙示例。Rc 和 Arc 都变异引用计数，即使可能有多个克隆都在使用相同的引用计数。

一旦涉及内部可变类型，称一个引用为“不可变”或“可变”就会变得令人困惑且不准确，因为有些东西可以通过两者进行变异。更准确的术语是“共享”和“独占”：*共享引用*（&T）可以被复制并与其他人共享，而*独占引用*（&mut T）保证它是该 T 的唯一*独占借用*。对于大多数类型，共享引用不允许变异，但有例外。由于在本书中我们将主要使用这些例外，我们将在本书的其余部分使用更准确的术语。

> CAUTION
> 
> 请记住，内部可变性只弯曲了共享借用的规则，以允许在共享时进行变异。它不会改变独占借用方面的任何内容。独占借用仍然保证没有其他活动借用。通过不安全代码创建对某物的多个独占引用始终是未定义行为，无论内部可变性如何。

让我们看一下具有内部可变性的一些类型，以及它们如何允许通过共享引用进行变异而不导致未定义



behavior.

### Cell

A Cell<T> simply wraps a T, but allows mutations through a shared reference. To avoid undefined behavior, it only allows you to copy the value out (if T is Copy), or replace it with another value as a whole. In addition, it can only be used within a single thread.

Let’s take a look at similar example as in the previous section, but this time using Cell<i32> instead of i32:

```
fn f(a: &Cell<i32>, b: &Cell<i32>) {
    let before = a.get();
    b.set(b.get() + 1);
    let after = a.get();
    if before != after {
        x(); // might happen
    }
}
```

Unlike last time, it is now possible for the if condition to be true. Because a Cell<i32> has interior mutability, the compiler can no longer assume its value won’t change as long as we have a shared reference to it. Both a and b might refer to the same value, such that mutating through b might affect a as well. It may still assume, however, that no other threads are accessing the cells concurrently.

The restrictions on a Cell are not always easy to work with. Since it can’t directly let us borrow the value it holds, we need to move a value out (leaving something in its place), modify it, then put it back, to mutate its contents.

```
fn f(v: &Cell<Vec<i32>>) {
    let mut v2 = v.take(); // Replaces the contents of the Cell with an empty Vec
    v2.push(1);
    v.set(v2); // Put the modified Vec back
}
```

### RefCell

Unlike a regular Cell, a RefCell does allow you to borrow its contents, at a small run-time cost. A RefCell<T> does not only hold a T, but also holds a counter that keeps track of any outstanding borrows. If you try to borrow it while it is already mutably borrowed, it will panic, which avoids undefined behavior. Just like a Cell, a RefCell can only be used within a single thread.

```
fn f(v: &RefCell<Vec<i32>>) {
    v.borrow_mut().push(1); // We can modify the `Vec` directly.
}
```

While Cell and RefCell can be very useful, they become rather useless when we need to do something with multiple threads. So let’s move on to the types that are relevant for concurrency.

### Mutex and RwLock

An RwLock or reader-writer lock is the concurrent version of a RefCell. An RwLock<T> holds a T, and tracks any outstanding borrows. However, unlike a RefCell, it does not panic on conflicting borrows. Instead, it blocks the current thread—putting it to sleep—while waiting for conflicting borrows to disappear. We’ll just have to patiently wait for our turn with the data, after the other threads are done with it.

Borrowing the contents of an RwLock is called locking. By locking it we temporarily block concurrent conflicting borrows, allowing us to borrow it without causing data races.

A Mutex (short for mutual exclusion) is very similar, but conceptually slightly simpler. Instead of keeping track of the number of shared and exclusive borrows like an RwLock, it only allows exclusive borrows.

We’ll go more into detail on these types in “Locking: Mutexes and RwLocks”.

### Atomics

The atomic types represent the concurrent version of a Cell, and are the main topic of Chapter 2 and X REF HERE FOR CH03. Like a Cell, they avoid undefined behavior by making us copy values in and out as a whole, without letting us borrow the contents directly.

Unlike a Cell though, they cannot be of arbitrary size. Because of this, there is no generic Atomic<T> type for any T, but there are only specific atomic types such as AtomicU32 and AtomicPtr. Which ones are available depends on the platform, since they require support from the processor to avoid data races. (We’ll dive into that in X REF HERE FOR CH04.)

Since they are so limited in size, atomics often don’t directly contain the information that needs to be shared between threads. Instead, they are often used as a tool to make it possible to share other—often bigger—things between threads. When atomics are used to say something about other data, things can get surprisingly complicated.

### UnsafeCell

An UnsafeCell is the primitive building block for interior mutability.

An UnsafeCell<T> wraps a T, but does not come with any conditions or restrictions to avoid undefined behavior. Instead, its get method just gives a raw pointer to the value it wraps, which can only be meaningfully used in unsafe blocks. It leaves it up to the user to use it in a way that does cause any undefined behavior.

Most commonly, an UnsafeCell is not used directly, but wrapped in another type that provides safety through a limited interface, such as Cell or Mutex. All types with interior mutability—including all types discussed above—are built on top of UnsafeCell.

### Thread Safety: Send and Sync

In this chapter, we’ve seen several types that are not *thread safe*, types that can only be used on a single thread, such as `Rc`, `Cell`, and others. Since that restriction is needed to avoid undefined behavior, it’s something the compiler needs to understand and check for you, so you can use these types without having to use `unsafe` blocks.

The language uses two special traits to keep track of which types can be safely used across threads:

#### Send

A type is `Send` if it can be sent to another thread. That is, if ownership of a value of that type can be transferred to another thread. For example, `Arc<i32>` is `Send`, but `Rc<i32>` is not.

#### Sync

A type is `Sync` if it can be shared with another thread. In other words, a type T is `Sync` if and only if a shared reference to that type, `&T`, is `Send`. For example, a `i32` is `Sync`, but a `Cell<i32>` is not.

All primitive types such as `i32`, `bool`, and `str` are all both `Send` and `Sync`.

Both of these traits are *auto traits*, which means that they are automatically implemented for your types based on their fields. A `struct` with fields that are all `Send` and `Sync`, is itself also `Send` and `Sync`.

The way to opt-out of either of these is to add a field to your type that does not implement the trait. For that purpose, the special `PhantomData<T>` type often comes in handy. That type is treated by the compiler as a T, except it doesn’t actually exist at runtime. It’s a zero sized type, taking no space.

Let’s take a look at this struct:

```
struct X {
    handle: i32,
```

```
_not_sync: PhantomData<Cell<()>>,
}
```

In this example, the struct X would be both Send and Sync if handle was its only field. However, we added a zero-sized PhantomData<Cell<()>> field, which is treated as if it were a Cell<()>. Since a Cell<()> is not Sync, neither is X. It is still Send, however, since all its fields implement Send.

Raw pointers (*const T and *mut T) are neither Send nor Sync, since the compiler doesn’t know much about what they represent.

The way to opt-in to either of the traits is the same as with any other trait: an impl block to implement the trait for you type:

```
struct X {
    p: *mut i32,
}

unsafe impl Send for X {}
unsafe impl Sync for X {}
```

Note how implementing these traits requires the unsafe keyword, since the compiler cannot check for you if it’s correct. It’s a promise you make to the compiler, which it will just have to trust.

If you try to move something into another thread which is not Send, the compiler will politely stop you from doing that. Here is a small example to demonstrate that:

```
fn main() {
    let a = Rc::new(123);
    thread::spawn(move || {
        dbg!(a);
    });
}
```

Here, we try to send an Rc<i32> to a new thread, but Rc<i32>, unlike Arc<i32>, does not implement Send.

If we try to compile this, we’re faced with the following error:

```
error[E0277]: `Rc<i32>` cannot be sent between threads safely
    --> src/main.rs:3:5
     |
3   |         thread::spawn(move || {
     |         ^^^^^^^^^^^^^ `Rc<i32>` cannot be sent between
threads safely
     |
     = help: within `[closure@src/main.rs:3:19: 5:6]`, the trait
`Send` is not implemented for `Rc<i32>`
     = note: required because it appears within the type
`[closure@src/main.rs:5:19: 5:6]`
note: required by a bound in `spawn`
```

The thread::spawn function requires its argument to be Send, and a closure is only Send if all of its captures are. If we try to capture something that’s not Send our mistake is caught, protecting us from undefined behavior.

### Locking: Mutexes and RwLocks

The most commonly used tool for sharing (mutable) data between threads is called a mutex, which is short for "mutual exclusion". The job of a mutex is to ensure threads have exclusive access to some data by temporarily blocking other threads that try to access it at the same time.

Conceptually, a mutex has only two operations: lock and unlock, and two states: locked and unlocked. When a thread locks an unlocked mutex, the mutex is marked as locked and the thread can immediately continue. When a thread then attempts to lock an already locked mutex, that operation will block. The thread is put to sleep while it waits for the mutex to be unlocked. Unlocking is only possible on a locked mutex, and should be done by the same thread that locked it. If other threads are waiting to lock the mutex, unlocking will cause one of those threads to be woken up, so it can try to lock the mutex again and continue its course.





使用互斥锁保护数据本质上是所有线程之间的约定：它们只有在持有互斥锁被锁定的情况下才会访问数据。这样一来，就不会有两个线程同时访问该数据，从而避免数据竞争。

### Rust 的 Mutex

Rust 标准库通过 `std::sync::Mutex<T>` 提供了此功能。它是关于类型 T 的泛型类型，其中 T 是互斥锁所保护的数据的类型。通过将此 T 作为互斥锁的一部分，数据只能通过互斥锁访问，从而提供了一个安全的接口，可以保证所有线程都会遵守约定。

为了确保只有锁定互斥锁的线程才能解锁它，它没有 `unlock()` 方法。相反，其 `lock()` 方法返回一种特殊类型，称为 `MutexGuard`。该守卫代表我们已锁定互斥锁的保证。它通过 `DerefMut` trait 表现得像一个独占引用，从而为我们提供对互斥锁保护的数据的独占访问。解锁互斥锁是通过丢弃守卫来完成的。当我们丢弃守卫时，我们放弃了访问数据的能力，并且守卫的 `Drop` 实现将解锁互斥锁。

让我们看一个示例来了解互斥锁的实际使用情况：

```
fn main() {
    let n = Mutex::new(0);
    thread::scope(|s| {
        for _ in 0..10 {
            s.spawn(|| {
                let mut guard = n.lock().unwrap();
                for _ in 0..100 {
                    *guard += 1;
                }
            });
        }
    });
    assert_eq!(n.into_inner().unwrap(), 1000);
}
```

这里，我们有一个 Mutex<i32>，即保护一个整数的互斥锁，并生成十个线程，每个线程将整数递增一百次。每个线程首先通过 lock() 锁定互斥锁以获得互斥锁守卫，然后使用该守卫访问整数并修改它。守卫在该变量超出作用域后会被隐式丢弃。

线程完成后，我们可以通过 into_inner() 安全地移除对整数的保护。into_inner 方法需要对互斥锁的独占引用，这保证了不会再有其他引用指向该互斥锁，从而使锁定变得不必要。

即使递增以 1 为步长进行，观察整数的线程也只能看到 100 的倍数，因为它只能在互斥锁未锁定时查看整数。有效地，由于互斥锁的存在，这一百次递增现在成为一个单一的不可分割的、原子的操作。

为了清楚地看到互斥锁的效果，我们可以让每个线程在解锁互斥锁前等待一秒：

```
..    s.spawn(|| {
        let mut guard = n.lock().unwrap();
        for _ in 0..100 {
            *guard += 1;
        }
        thread::sleep(Duration::from_secs(1)); // Sleep
before unlocking the mutex.
    });
..
```

现在运行程序时，你会看到它大约需要十秒钟才能完成。每个线程只等待一秒，但互斥锁会确保一次只有一个线程可以这样做。

如果我们在休眠一秒之前丢弃守卫，从而解锁互斥锁，我们会看到它改为并行发生。

```
..    s.spawn(|| {
```

```
let mut guard = n.lock().unwrap();
for _ in 0..100 {
    *guard += 1;
}
drop(guard); // Drop the guard before sleeping
thread::sleep(Duration::from_secs(1));
});
```

one second.
..

有了这个更改，此程序将只花费大约一秒钟，因为现在十个线程可以同时执行其一秒的休眠。这显示了保持互斥锁锁定时间尽可能短的重要性。保持互斥锁锁定时间超过必要的时间可能会完全抵消并行性的任何好处，有效地强制所有操作按串行发生。

### 锁中毒

上面示例中的 unwrap() 调用与锁中毒有关。

当线程在持有锁时发生 panic 时，Rust 中的 Mutex 会被标记为中毒。lock 方法在锁被中毒时会返回 Err。

这是一种防止受互斥锁保护的数据处于不一致状态的机制。如果在上面的示例中，线程在递增整数少于一百次后发生 panic，则互斥锁会解锁，整数会被留在不再是 100 的倍数的意外状态中，可能破坏其他线程所做的假设。在这种情况下自动将互斥锁标记为中毒会强制用户处理这种可能性。

在中毒的互斥锁上调用 lock() 仍然会锁定互斥锁。lock() 返回的 Err 包含互斥锁守卫，允许我们纠正不一致的状态或简单地忽略中毒。

虽然锁中毒看起来像是个好主意，但在实践中并不常用。大多数代码要么忽略中毒，要么使用 unwrap() 在

锁被中毒时发生 panic，有效地将 panic 传播给互斥锁的所有使用者。

### MUTEX GUARD 的生命周期

虽然隐式丢弃守卫来解锁互斥锁对于正确性来说很方便也很重要，但有时会导致微妙的意外。如果我们使用 let 语句为守卫分配一个名称（如我们在上面的示例中所做的那样），那么它何时会被丢弃就相对容易看出，因为局部变量会在其定义的作用域结束时被丢弃。尽管如此，不显式丢弃守卫可能会导致互斥锁被锁定比必要的时间更长，正如上面的示例所示。

使用未分配名称的守卫也是可能的，有时非常方便。由于 MutexGuard 表现得像对受保护数据的独占引用，我们可以直接使用它，而无需先为守卫分配名称。例如，如果你有一个 Mutex<Vec<i32>>，你可以锁定互斥锁，将项目推入 Vec，然后在单个语句中再次解锁互斥锁：

```
list.lock().unwrap().push(1);
```

在较大表达式中产生的任何临时值，例如 lock() 返回的守卫，都将在语句结束时被丢弃。虽然这看起来很明显且合理，但它会导致一个常见的陷阱，通常涉及 match、if let 或 while let 语句。以下是遇到此陷阱的示例：

```
if let Some(item) = list.lock().unwrap().pop() {
    process_item(item);
}
```

如果我们的意图是锁定列表，弹出一个项目，解锁列表，然后在列表解锁后处理该项目，我们在这里犯了一个微妙但重要的错误。临时守卫不会在整个 if let 语句结束之前被丢弃，这意味着我们在处理项目时不必要地持有锁。

也许令人惊讶的是，这对于类似的 if 语句，例如以下示例，并不会发生：

```
if list.lock().unwrap().pop() == Some(1) {
    do_something();
}
```

这里，临时守卫会在 if 语句的主体执行之前被丢弃。原因在于常规 if 语句的条件始终是一个纯布尔值，不能借用任何东西。没有理由将临时值的生命周期从条件扩展到语句的末尾。对于 if let 语句来说，情况可能并非如此。例如，如果我们使用 front() 而不是 pop()，item 将从列表中借用，使得有必要保留守卫。由于借用检查器只是一个检查，并不影响何时或按什么顺序丢弃事物，因此在使用 pop() 时也会发生同样的情况，即使这本是不必要的。

我们可以通过将 pop 操作移动到单独的 let 语句来避免这种情况。然后守卫会在该语句结束时被丢弃，在 if let 之前：

```
let item = list.lock().unwrap().pop();
if let Some(item) = item {
    process_item(item);
}
```

### 读写锁

互斥锁只关心独占访问。MutexGuard 将为我们提供对受保护数据的独占引用（&mut T），即使我们只想查看数据并且共享引用（&T）就已足够。



读写锁是互斥锁的稍微复杂一些的版本，它能够理解独占访问和共享访问的区别，并能提供两者。它有三种状态：未锁定、由单个 *writer*（用于独占访问）锁定，或由任意数量的 *readers*（用于共享访问）锁定。它常用于多线程频繁读取数据，但只偶尔更新的数据。

Rust 标准库通过 `std::sync::RwLock<T>` 类型提供这种锁。它的工作方式与标准的 `Mutex` 非常相似，只是其接口主要分为两部分。不是只有一个 `lock()` 方法，它有 `read()` 和 `write()` 方法用于分别以读者的身份或写者的身份锁定。它带有两种保护类型，一种用于读者，一种用于写者：`RwLockReadGuard` 和 `RwLockWriteGuard`。前者仅实现 `Deref` 以表现得像对受保护数据的共享引用，而后者还实现 `DerefMut` 以表现得像独占引用。

它在本质上是 `RefCell` 的多线程版本：动态跟踪对引用的数量以确保借用规则得到遵守。

`Mutex<T>` 和 `RwLock<T>` 都要求 `T` 实现 `Send`，因为它们可用于将 `T` 发送到另一个线程。`RwLock<T>` 额外要求 `T` 也实现 `Sync`，因为它允许多个线程持有对受保护数据的共享引用（`&T`）。严格来说，你可以为不满足这些要求的 `T` 创建锁，但你将无法在线程间共享它，因为锁本身不会实现 `Sync`。

读写锁实现之间存在许多微妙的差异。最重要的是，有些优先读者，有些优先写者。在某些实现中，当有写者在等待时，新读者会被阻塞并必须等待，即使锁已经被读锁定。这是为了防止 *writer starvation*，即许多读者共同导致锁永远无法解锁，从而永远不允许任何写者更新数据的情况。

### MUTEXES IN OTHER LANGUAGES

Rust 标准库的 Mutex 和 RwLock 看起来与其他语言如 C 或 C++ 中的有所不同。

最大的区别在于 Rust 的 `Mutex<T>` 包含它所保护的数据。例如在 C++ 中，`std::mutex` 不包含它保护的数据，甚至根本不知道它在保护什么。这意味着用户有责任记住哪些数据受保护以及由哪个互斥锁保护，并确保每次访问受保护数据时都锁定正确的互斥锁。在阅读其他语言中涉及互斥锁的代码时，或与不熟悉 Rust 的程序员交流时，记住这一点很有用。Rust 程序员可能会谈论“互斥锁内部的数据”，或说诸如“把它包在互斥锁里”之类的话，这对只熟悉其他语言中互斥锁的人来说会令人困惑。

如果你真的需要一个不包含任何内容的独立互斥锁，例如用于保护某些外部硬件，你可以使用 `Mutex<()>`. 但即使在这些情况下，你最好定义一个（可能是零大小的）类型来与该硬件交互并将其包装在 Mutex 中。这样，你仍然被迫在与硬件交互之前锁定互斥锁。

### Waiting: Parking and Condition Variables

当数据被多个线程修改时，存在许多情况需要它们等待某个事件；等待数据上的某个条件变为真。例如，如果我们有一个互斥锁保护一个 Vec，我们可能想等待它包含任何元素。

虽然互斥锁确实允许线程等待其解锁，但它不提供等待其他条件的功功能。如果互斥锁是我们唯一拥有的东西，我们就必须不断锁定互斥锁以反复检查 Vec 中是否已有内容。

#### Thread parking

等待另一个线程通知的一种方法称为 *thread parking*。一个线程可以 *park* 自己，这会使其进入睡眠状态，停止消耗任何 CPU 周期。另一个线程随后可以 *unpark* 已停靠的线程，使其从小睡中唤醒。

线程停靠可通过 `std::thread::park()` 函数获得。对于取消停靠，你在表示要取消停靠的线程的 Thread 对象上调用 `unpark()` 方法。这样的对象可以从 `spawn` 返回的 join handle 获得，或由线程本身通过 `std::thread::current()` 获得。

让我们深入研究一个使用互斥锁在两个线程之间共享队列的示例。在下面的示例中，一个新生成的线程将从队列中消费项目，而主线程将每秒向队列中插入一个新项目。线程停靠用于使消费线程在队列为空时等待。

```
let queue = Mutex::new(VecDeque::new());

thread::scope(|s| {
    // Consuming thread
    let t = s.spawn(|| loop {
        let item = queue.lock().unwrap().pop_front();
        if let Some(item) = item {
            dbg!(item);
        } else {
            thread::park();
        }
    });

    // Producing thread
    for i in 0.. {
        queue.lock().unwrap().push_back(i);
        t.thread().unpark();
        thread::sleep(Duration::from_secs(1));
    }
});
```

消费线程在一个无限循环中运行，从队列中弹出项目并使用 `dbg` 宏显示它们。当队列为空时，它会停止并使用 `park` 函数睡眠。如果它被取消停靠，它会重新开始并再次从队列中弹出项目，直到它为空。如此循环。

生产线程每秒生产一个新数字，将其推入队列。每次添加项目时，它都会在引用消费线程的 `Thread` 对象上使用 `unpark` 方法来取消停靠它。这样，消费线程就会被唤醒以处理新元素。

这里一个重要的观察是，如果我们移除停靠，这个程序在理论上仍然是正确的，只是效率低下。这很重要，因为 `park()` 不保证它只会因为匹配的 `unpark()` 而返回。虽然有点罕见，但它可能会有 *spurious wake-ups*。我们的示例对此处理得很好，因为消费线程会锁定队列，发现它是空的，然后直接解锁并再次停靠自己。

线程停靠的一个重要属性是，在线程停靠自己之前对 `unpark()` 的调用不会丢失。取消停靠请求仍会被记录，下一次线程尝试停靠自己时，它会清除该请求并直接继续，而不会真正进入睡眠。要了解这对正确操作为何至关重要，让我们回顾一下两个线程执行的步骤的可能顺序：

- 1. 消费线程，我们称之为 C，锁定队列。
- 2. C 尝试从队列中弹出项目，但它是空的，结果为 `None`。
- 3. C 解锁队列。
- 4. 生产线程，我们称之为 P，锁定队列。
- 5. P 将新项目推到队列上。
- 6. P 再次解锁队列。
- 7. P 调用 `unpark()` 来通知 C 有新项目。
- 8. C 调用 `park()` 去睡眠，等待更多项目。

虽然在第 3 步释放队列和第 8 步停靠之间只有很短的一瞬间，但在那短暂的瞬间，步骤 4 到 7 可能会发生。如果 `unpark()` 在线程未停靠时被忽略，通知就会丢失。消费线程仍在等待，即使队列中有项目。由于取消停靠请求会为未来的 `park()` 调用保存，我们不必担心这个问题。

然而，取消停靠请求不会堆积。调用 `unpark()` 两次然后之后调用 `park()` 两次仍然会导致线程进入睡眠。第一次 `park()` 清除请求并直接返回，但第二次则正常进入睡眠。

这意味着在我们上面的示例中很重要，我们只有在看到队列为空时才停靠线程，而不是在处理完每个项目后停靠。虽然在这个示例中由于巨大的（一秒）睡眠而极不可能发生，但多个 `unpark()` 调用可能最终只唤醒一次 `park()` 调用。

不幸的是，这也意味着如果在 `park()` 返回后但在队列被锁定并清空之前调用了 `unpark()`，该 `unpark()` 调用是不必要的，但仍会导致下一次 `park()` 调用立即返回。这会导致（空的）队列被额外锁定和解锁一次。虽然这不影响程序的正确性，但确实会影响其效率和性能。

这种机制对于像我们示例中这样的简单情况效果很好，但在事情变得更复杂时很快就会失效。例如，如果我们有多个消费者线程从同一个队列中取项目，生产者线程将无法知道



实际上正在等待并且应该被唤醒。生产者必须确切知道消费者何时在等待，以及它在等待什么条件。

### 条件变量

条件变量是等待受互斥锁保护的数据发生某些变化时更常用的选项。它们有两个基本操作：wait 和 notify。线程可以在条件变量上等待，并在有人通知同一个条件变量时被唤醒。多个线程可以等待同一个条件变量，通知可以发送给其中一个等待线程，也可以发送给所有线程。

这意味着我们可以为我们感兴趣的特定事件或条件创建一个条件变量，例如队列非空，并在该条件上等待。任何导致该事件或条件发生的线程都会通知条件变量，而无需知道有多少线程对该通知感兴趣。

为了避免在解锁互斥锁和等待条件变量之间的短暂瞬间丢失通知的问题，条件变量提供了一种原子地解锁互斥锁并开始等待的方式。这意味着通知根本没有可能丢失的时刻。

Rust 标准库提供了一个条件变量 `std::sync::Condvar`。它的 wait 方法接受一个 MutexGuard 来证明我们已锁定互斥锁。它会解锁互斥锁并进入睡眠，并在被唤醒时重新锁定互斥锁并返回一个新的 MutexGuard，证明互斥锁再次被锁定。

它有两个 notify 函数：`notify_one` 用于唤醒一个等待线程（如果有），以及 `notify_all` 用于唤醒所有线程。

让我们修改我们用于线程挂起的示例以使用 Condvar 代替：

```
let queue = Mutex::new(VecDeque::new());
let not_empty = Condvar::new();
```

```
thread::scope(|s| {
    s.spawn(|| {
        loop {
            let mut q = queue.lock().unwrap();
            let item = loop {
                if let Some(item) = q.pop_front() {
                    break item;
                } else {
                    q = not_empty.wait(q).unwrap();
                }
            };
            drop(q);
            dbg!(item);
        }
    });

    for i in 0.. {
        queue.lock().unwrap().push_back(i);
        not_empty.notify_one();
        thread::sleep(Duration::from_secs(1));
    }
});
```

我们不得不改变一些东西：

- 我们现在不仅有一个包含队列的 Mutex，还有一个 Condvar 来通过它传递非空条件。
- 我们不再需要知道唤醒哪个线程，因此不再存储 spawn 的返回值。相反，我们通过条件变量的 notify_one 方法通知消费者。
- 解锁、等待和重新锁定都由 wait 方法完成。我们不得不稍微重构控制流，以便能够将 guard 传递给 wait 方法，同时在处理项目之前仍然丢弃它。

现在我们可以按需生成任意数量的消费者线程，甚至稍后生成更多，而无需更改任何内容。条件变量会负责将通知传递给感兴趣的线程。

如果我们有一个更复杂的系统，其中线程对不同的条件感兴趣，我们可以为每个条件定义一个 `Condvar`。例如，一个表示队列非空，另一个表示队列为空。然后每个线程都可以等待与其正在执行的操作相关的条件。

通常，`Condvar` 只与单个 `Mutex` 一起使用。如果两个线程尝试使用两个不同的互斥锁并发地 `wait` 在同一个条件变量上，可能会导致 panic。

`Condvar` 的一个缺点是它只能与 `Mutex` 一起使用，但对于几乎大多数用例来说这完全没问题，因为这本来就是已经使用的东西。

### 总结

- 多个线程可以在同一个程序中并发运行，并且可以在任何时候生成。
- 当主线程结束时，整个程序结束。
- 数据竞争是未定义行为，并且在安全代码中由 Rust 的类型系统完全防止。
- 可以发送到其他线程的数据是 `Send`，可以在线程之间共享的数据是 `Sync`。
- 普通线程可能与程序一样长时间运行，因此只能借用 `'static` 数据，例如静态数据和泄漏的分配。
- 引用计数（`Arc`）可用于共享所有权，以确保数据在任何线程使用它时都存活。
- 作用域线程对于限制线程的生命周期很有用，以便它可以借用非 `'static` 数据，例如局部变量。
- `&T` 是一个 *共享* 引用。`&mut T` 是一个 *独占* 引用。常规类型不允许通过共享引用进行突变。
- 一些类型具有内部可变性，这允许通过共享引用进行突变，并且始终基于 `UnsafeCell`。
- `Cell` 和 `RefCell` 是单线程内部可变性的标准类型。`Atomics`、`Mutex` 和 `RwLock` 是它们的多线程等价物。
- `Cell` 和原子操作只允许整体替换值，而 `RefCell`、`Mutex` 和 `RwLock` 允许你通过动态强制访问规则直接突变值。
- 线程挂起可以是一种方便的方式来等待某个条件。
- 当条件涉及受 `Mutex` 保护的数据时，使用 `Condvar` 比线程挂起更方便。
