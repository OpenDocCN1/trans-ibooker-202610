# 9 ASYNCHRONOUS PROGRAMMING

![](img/bbeab1f2edc3d45a3e6f7f6efbbb1747_120_0.png)

异步编程顾名思义，就是非同步的编程。在高层面上，异步操作在后台执行——程序不会等待异步操作完成，而是会立即继续执行下一行代码。如果你对异步编程还不熟悉，这个定义可能感觉不够，因为它并没有真正解释异步编程是什么。要真正理解 Rust 中的异步编程模型及其工作原理，我们必须先深入了解替代方案。也就是说，我们需要在理解异步模型之前先理解同步编程模型。这在澄清概念和展示使用异步编程的权衡方面都很重要：异步解决方案并不总是正确的！我们将从先快速了解异步编程作为概念的动机开始；然后深入探讨 Rust 中异步在底层实际如何工作。

### What’s the Deal with Asynchrony?

在我们进入同步和异步编程模型的细节之前，我们首先需要快速了解一下你的计算机在运行你的程序时实际在做什么。

计算机很快。非常快。事实上快到它们大部分时间都在等待事情发生。除非你在解压缩文件、编码音频或 crunch 数字，否则很可能你的 CPU 大部分时间都在空闲，等待操作完成。它在等待网络数据包到达，等待鼠标移动，等待磁盘完成写入字节，甚至可能只是等待主内存读取完成。从 CPU 的角度来看，在大多数此类事件之间，漫长的岁月过去了。当其中一个事件发生时，CPU 运行几条指令，然后又回到等待状态。看看你的 CPU 利用率——它可能在个位数徘徊，而且这很可能就是它大部分时间的状态。

### Synchronous Interfaces

同步接口只允许你的程序（或更确切地说，你程序中的单个线程）一次执行一个操作；每个操作必须等待前一个同步操作完成后才能运行。你在野外看到的大多数接口都是同步的：你调用它们，它们去做一些事情，最终在操作完成后返回，你的程序就可以继续了。其原因正如本章后面会看到的，是使操作异步需要相当多的额外机制。除非你需要异步的好处，否则坚持同步模型需要的繁文缛节要少得多。

同步接口隐藏了所有这些等待；应用程序调用一个函数说“把这些字节写入这个文件”，一段时间后，该函数完成，下一行代码执行。在幕后，真正发生的是操作系统将写操作排队到磁盘，然后让应用程序休眠，直到磁盘报告写操作已完成。应用程序将其体验为函数执行时间很长，但实际上它根本没有执行，只是在等待。

以这种方式顺序执行操作的接口也常被称为阻塞，因为接口中必须等待某些外部事件发生才能取得进展的操作会阻塞进一步的执行，直到该事件发生。无论你将接口称为同步还是阻塞，其基本思想都相同：应用程序不会继续，直到当前操作完成。在操作等待期间，应用程序也在等待。

同步接口通常被认为易于使用且易于推理，因为你的代码一次只执行一行。




但它们也只允许应用程序一次做一件事。这意味着如果你的程序想要等待用户输入或网络数据包，除非你的操作系统提供了专门用于此的操作，否则你就无计可施。同样，即使你的应用程序在磁盘写入文件时可以做一些其他有用的工作，它也没有这个选项，因为文件写入操作会阻塞执行！

## 使用多线程实现并发

迄今为止，允许并发执行最常见的方法是使用多线程。在多线程程序中，每个线程负责执行一个特定的独立的阻塞操作序列，操作系统在线程之间进行复用，这样如果有任何线程可以取得进展，就能取得进展。如果一个线程被阻塞，其他某个线程可能仍然可以运行，因此应用程序可以继续做有用的工作。

通常，这些线程使用锁或通道之类的同步原语相互通信，以便应用程序仍能协调它们的工作。例如，你可能有一个等待用户输入的线程，一个等待网络数据包的线程，以及另一个等待其中任一线程在所有三个线程之间共享的通道上发送消息的线程。

多线程为你提供了并发——能够在同一时间执行多个独立操作的能力。运行应用程序的系统（在本例中为操作系统）负责在未被阻塞的线程中选择并决定下一个执行哪个线程。如果一个线程被阻塞，它可以选择运行另一个可以取得进展的线程。

多线程结合阻塞接口可以走得很远，而且大量可投入生产的软件都是以这种方式构建的。但这种方法并非没有缺点。首先，跟踪所有这些线程很快就会变得繁琐；如果你必须为每个并发任务启动一个线程，包括像等待键盘输入这样简单的任务，线程会迅速累积，跟踪所有这些线程如何交互、通信和协调所需的额外复杂性也会随之增加。

其次，在线程之间切换的成本会随着线程数量的增加而变得昂贵。每当一个线程停止运行，另一个线程在其位置重新启动时，你需要进行一次往返操作系统调度程序的调用，而这并非免费。在某些平台上，创建新线程也是一项相当重量级的过程。性能要求高的应用程序通常通过重用线程并使用允许你在许多相关操作上阻塞的操作系统调用来减轻这种成本，但最终你仍会遇到同样的问题：阻塞接口要求你拥有与你想要进行的阻塞调用数量一样多的线程。

最后，线程为你的程序引入了并行性。并发与并行之间的区别很微妙，但很重要：并发意味着你的任务执行是交错的，而并行意味着多个任务同时执行。如果有两个任务，用 ASCII 表示可能看起来像 _-_-_（并发）与 =====（并行）。多线程并不一定意味着并行——即使你有许多线程，你可能只有一个核心，因此在给定时间只有一个线程在执行——但两者通常是相辅相成的。你可以通过使用 Mutex 或其他同步原语使两个线程在执行上互斥，但这会引入额外的复杂性——线程希望并行运行。而虽然并行通常是一件好事——谁不希望自己的程序在更多核心上运行得更快——但这也意味着你的程序必须处理对共享数据结构的真正同时访问。这意味着从 Rc、Cell 和 RefCell 迁移到更强大但也更慢的 Arc 和 Mutex。虽然你可能希望在并发程序中使用后者类型以实现并行，但线程会迫使你使用它们。

## 异步接口

现在我们已经探讨了同步接口，我们可以来看一下替代方案：异步或非阻塞接口。异步接口是指可能不会立即产生结果，而可能表明结果将在稍后某个时间可用。这给了调用者机会在等待期间做其他事情，而不是必须休眠直到特定操作完成。在 Rust 用语中，异步接口是一种返回 Poll 的方法，如清单 9-1 所定义。

```rust
enum Poll<T> {
    Ready(T),
    Pending
}
```

清单 9-1：异步的核心：“给你或稍后再来”的类型

Poll 通常出现在以 poll 开头的函数的返回类型中——这些方法是信号表明它们可以尝试在不阻塞的情况下执行操作。我们将在本章后面了解它们具体如何做到这一点，但总的来说，它们在通常会阻塞之前尝试尽可能多地执行操作，然后返回。至关重要的是，它们记住自己停在了哪里，以便在以后可以再次取得进展时恢复执行。

这些非阻塞函数使我们能够轻松地并发执行多个任务。例如，如果你想从网络或用户键盘读取，以先有事件可用者为准，你所要做的就是在一个循环中轮询两者，直到其中一个返回 Poll::Ready。无需任何额外的线程或同步！

这里的“循环”一词应该让你有点紧张。你不希望你的程序每秒烧掉三十亿次循环，而下一个输入可能要等几分钟才会出现。在阻塞接口的世界里，这不是问题，因为操作系统只是让线程休眠，然后在相关事件发生时负责唤醒它，但在这种大胆的新非阻塞世界中，我们如何避免在等待时浪费周期？这正是本章剩余大部分内容要讨论的问题。

这段代码看起来与等价的同步代码非常相似，而且易于阅读。我们只是在一个循环中发送收到的每条消息，直到没有更多消息为止，而每个 await 点都对应于同步变体可能阻塞的位置。现在想想如果你必须通过手动实现 Future trait 来表达这段代码会怎样。由于每次调用 poll 都是从函数顶部开始的，你需要打包必要的状态以从代码上次让出时的位置继续。结果相当丑陋，如清单 9-4 所示。

```rust
enum Forward<T> { ①
    WaitingForReceive(ReceiveFuture<T>, Option<Sender<T>>),
    WaitingForSend(SendFuture<T>, Option<Receiver<T>>),
}

impl<T> Future for Forward<T> {
    type Output = (); ②
    fn poll(&mut self) -> Poll<Self::Output> {
        match self { ③
            Forward::WaitingForReceive(recv, tx) => {
                if let Poll::Ready((rx, v)) = recv.poll() {
                    if let Some(v) = v {
                        let tx = tx.take().unwrap(); ④
                        *self = Forward::WaitingForSend(tx.send(v), Some(rx)); ⑤
                        // Try to make progress on sending.
                        return self.poll(); ⑥
                    } else {
                        // No more items.
                        Poll::Ready(())
                    }
                } else {
                    Poll::Pending
                }
            }
            Forward::WaitingForSend(send, rx) => {
                if let Poll::Ready(tx) = send.poll() {
                    let rx = rx.take().unwrap();
                    *self = Forward::WaitingForReceive(rx.receive(), Some(tx));
                    // Try to make progress on receiving.
                    return self.poll();
                } else {
                    Poll::Pending
                }
            }
        }
    }
}
```

清单 9-4：手动实现一个通道转发 future

你很少需要在 Rust 中再写这样的代码了，但它提供了对底层工作原理的重要见解，所以让我们逐步分析一下。首先，我们将我们的 future 类型定义为一个枚举 ①，我们将使用它来跟踪我们当前正在等待的内容。这是由于



当我们返回 `Poll::Pending` 时，下一次调用 `poll` 会再次从函数顶部开始执行。我们需要某种方式来知道之前执行到了哪一步，这样才能知道应该继续执行哪个操作。此外，我们还需要根据当前正在执行的操作来跟踪不同的信息：如果我们正在等待接收操作完成，就需要保留那个 `ReceiveFuture`（本示例中未展示其定义），以便下一次我们自己被轮询时可以对它进行轮询；`SendFuture` 也是如此。这里的 `Option` 可能也会让你觉得有些奇怪；我们很快就会回到这个问题。

当我们为 `Forward` 实现 `Future` 时，会将其输出类型声明为 `()` ②，因为这个 future 实际上不会返回任何内容。相反，当它完成了将输入通道中的所有内容转发到输出通道后，这个 future 就会完成（不产生任何结果）。在更完整的示例中，我们的转发类型的 `Output` 可能会是一个 `Result`，这样它就能将 `receive()` 和 `send()` 中的错误向上传递给负责轮询转发操作是否完成的函数。不过，这段代码已经足够复杂了，所以我们把这个问题留到以后再讨论。

当 `Forward` 被轮询时，它需要从上次停止的地方继续执行；我们可以通过匹配 `self` 当前持有的枚举变体来确定这一点 ③。无论进入哪个分支，第一步都是轮询会阻塞当前操作继续进行的 future；如果我们正在尝试接收，就轮询 `ReceiveFuture`；如果正在尝试发送，就轮询 `SendFuture`。如果这次调用 `poll` 返回 `Poll::Pending`，那么我们无法取得任何进展，于是自己也返回 `Poll::Pending`。但如果当前的 future 完成了，我们就有工作要做了！

当其中一个内部 future 完成时，我们需要通过切换 `self` 中存储的枚举变体来更新当前操作。为此，我们必须从 `self` 中移出内容，以调用 `Receiver::receive` 或 `Sender::send`——但我们做不到，因为手头只有 `&mut self`。因此，我们将必须移出的状态存储在一个 `Option` 中，然后使用 `Option::take` 将其移出 ④。由于我们接下来马上就要覆盖 `self` ⑤，这样做其实有些多余，因此这些 `Option` 始终都会是 `Some`；但有时为了让借用检查器满意，就需要使用这样的技巧。

最后，如果我们确实取得了进展，就会再次轮询 `self` ⑥，这样一来，如果我们能够立即继续处理尚未完成的发送或接收操作，就可以直接完成。这在实现真正的 `Future` trait 时对于正确性而言确实是必要的，我们稍后会再讨论；不过目前可以把它看作一种优化。

我们刚刚手写了一个状态机：一种拥有多个可能状态，并会响应特定事件在这些状态之间切换的类型。而且这还是一个相当简单的状态机。想象一下，在更复杂的使用场景中，如果还有额外的中间步骤，你得为此编写怎样一大段代码！

除了要编写这种笨重的状态机之外，我们还必须知道 `Sender::send` 和 `Receiver::receive` 返回的 future 的类型，这样才能将它们存储在我们的类型中。如果这些方法返回的是 `impl Future`，我们就没有办法写出各个变体所需的具体类型。这些 `send` 和 `receive` 方法还必须取得发送端和接收端的所有权；如果不是这样，它们返回的 future 的生命周期就会与对 `self` 的借用绑定，而这个借用会在我们从 `poll` 返回时结束。但这行不通，因为我们正试图将这些 future 存储在 `self` 中。

> 注意：你可能已经注意到，`Receiver` 看起来很像异步版本的 `Iterator`。其他人也注意到了这一点，标准库正在逐步添加一个专门用于那些能够合理实现 `poll_next` 的类型的 trait。从长远来看，这些异步迭代器（通常称为 stream）可能会获得一流的语言支持，例如能够直接对它们进行循环！

归根结底，这段代码难以编写、难以阅读，也难以修改。例如，如果我们想添加错误处理，代码的复杂度就会显著增加。幸运的是，还有更好的方法！

## async/await

Rust 1.39 为我们带来了 `async` 关键字以及与之密切相关的后缀运算符 `await`，我们在示例 9-3 的原始代码中使用了它们。二者结合起来，为编写类似示例 9-5 中的异步状态机提供了一种更加便利的机制。具体来说，它们让你能够以一种完全不像状态机的方式编写代码！

```
async fn forward<T>(rx: Receiver<T>, tx: Sender<T>) {
    while let Some(t) = rx.next().await {
        tx.send(t).await;
    }
}
```

示例 9-5：使用 async 和 await 实现通道转发 future，重复自示例 9-3

如果你对 `async` 和 `await` 不太熟悉，那么示例 9-3 与示例 9-5 之间的差异，或许能让你理解为什么 Rust 社区在它们加入 Rust 时会如此兴奋。不过，既然这是一本中级读物，那我们就深入一点，了解这段简短的代码究竟如何取代前面那段长得多的手动实现。为此，我们首先需要讨论生成器——`async` 和 `await` 的实现机制。

## 生成器

简单来说，生成器是一段代码，其中包含一些由编译器额外生成的部分，使它能够在执行到一半时停止或让出执行权，之后再从上次让出的位置继续执行。以示例 9-3 中的 `forward` 函数为例。假设它执行到了 `send` 调用，但通道当前已满。函数无法再取得任何进展，但也不能阻塞（毕竟这是非阻塞代码），所以它需要返回。现在假设通道最终有了空间，我们希望继续执行发送操作。如果我们从顶部再次调用 `forward`，它会再次调用 `next`，而我们之前尝试发送的项目就会丢失，这显然不行。因此，我们将 `forward` 转换为一个生成器。

每当 `forward` 生成器无法继续取得进展时，它都需要将当前状态存储在某个地方，这样当它最终恢复执行时，就能在正确的位置、以正确的状态继续运行。它通过一个由编译器生成的关联数据结构来保存状态，该数据结构包含生成器在某个时间点的全部状态。这个数据结构上的一个方法（同样由编译器生成）允许生成器从存储在 `&mut self` 中的当前状态恢复执行，并在生成器再次无法取得进展时更新状态。

这种“返回，但允许我稍后恢复执行”的操作称为让出（yielding），它实际上意味着函数在返回的同时，将额外的状态保留在一旁。当我们稍后想要恢复对 `forward` 的调用时，就会调用生成器中已知的入口点（也就是恢复方法；对于异步生成器来说，这个方法就是 `poll`），生成器会检查之前存储在 `self` 中的状态，以决定下一步该做什么。这正是我们在示例 9-4 中手动完成的事情！换句话说，示例 9-3 中的代码大致可以反糖化为示例 9-6 中所示的假想代码。

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

示例 9-6：将 async/await 反糖化为生成器

在撰写本书时，生成器实际上还无法在 Rust 中使用——它们目前仅由编译器内部用于实现 `async/await`——但这种情况未来可能会改变。生成器在许多场景中都很有用，例如实现迭代器时不必携带一个结构体，或者使用能够确定如何一次让出一个项目的 `impl Iterator`。

如果你仔细观察示例 9-3 和示例 9-5，那么在知道每个 `await` 或 `yield` 实际上都是函数的一次返回之后，它们可能会显得有些神奇。毕竟，函数中有多个局部变量，而当我们稍后恢复执行时，并不清楚这些变量是如何被恢复的。这正是生成器中由编译器生成的部分发挥作用的地方。编译器会透明地注入代码，在执行时将这些变量持久化到生成器的关联数据结构中，并从中读取它们，而不是将它们存储在栈上。因此，如果你声明、写入或读取某个局部变量 `a`，实际上操作的是类似于 `self.a` 的东西。问题解决！这一切确实非常奇妙。

手动实现的 `forward` 与 `async/await` 版本之间，有一个微妙但重要的区别：后者可以跨越让出点持有引用。这使得示例 9-5 中的 `Receiver::next` 和 `Sender::send` 等函数可以接收 `&mut self`，而不是示例 9-3 中它们所接收的 `self`。如果我们尝试在手动状态



machine implementation, the borrow checker would have no way to enforce that the `Receiver` stored inside `Forward` cannot be referenced between when `Receiver::next` is called and when the future it returns resolves, and so it would reject the code. Only by moving the `Receiver` into the future can we convince the compiler that the `Receiver` is not otherwise accessible. Meanwhile, with async/await, the borrow checker can inspect the code before the compiler turns it into a state machine and verify that `rx` is indeed not accessed again until after the future is dropped, when the await on it returns.

## THE SIZE OF GENERATORS

The data structure used to back a generator’s state must be able to hold the combined state at any one yield point. If your async fn contains, say, a `[u8; 8192]`, those 8 KiB must be stored in the generator itself. Even if your async fn only contains smaller local variables, it must also contain any future that it awaits, since it needs to be able to poll such a future later, when `poll` is invoked.

This nesting means that generators, and thus futures based on async functions and blocks, can get quite large without any visible indicator of that increased size in your code. This can in turn impact your program’s runtime performance, since those giant generators may have to be copied across function calls and in and out of data structures, which amounts to a fair amount of memory copying. In fact, you can usually identify when the size of your generator-based futures is affecting performance by looking for excessive amounts of time spent in the `memcpy` function in your application’s performance profiles!

Finding these large futures isn’t always easy, however, and often requires manually identifying long or complex chains of async functions. Clippy may be able to help with this in the future, but at the time of writing, you’re on your own. When you do find a particularly large future, you have two options: you can try to reduce the amount of local state the async functions need, or you can move the future to the heap (with `Box::pin`), so that moving the future just requires moving the pointer to it. The latter is by far the easiest way to go, but also introduces an extra allocation and a pointer indirection. Your best bet is usually to put the problematic future on the heap, measure your performance, and then use your performance benchmarks to guide you from there.

## Pin and Unpin

We’re not quite done. While generators are neat, a challenge arises from the technique as I’ve described it so far. In particular, it’s not clear what happens if the code in the generator (or, equivalently, the async block) takes a reference to a local variable. In the code from Listing 9-5, the future that `rx.next()` returns must necessarily hold a reference to `rx` if a next message is not immediately available so that it knows where to try again when the generator next resumes. When the generator yields, the future and the reference the future contains get stashed away inside the generator. But what now happens if the generator is moved? Specifically, look at the code in Listing 9-7 which calls `forward`.

```rust
async fn try_forward<T>(rx: Receiver<T>, tx: Sender<T>) -> Option<impl Future>
{
    let mut f = forward(rx, tx);
    if f.poll().is_pending() { Some(f) } else { None }
}
```

*Listing 9-8: Moving a future after polling it*

The `try_forward` function only polls `forward` once, to forward as many messages as possible without blocking. If the receiver may still produce more messages (that is, if it returned `Poll::Pending` instead of `Poll::Ready(None)`), those messages are deferred to be forwarded at some later time by returning the forwarding future to the caller, which may choose to poll again at a time when it sees fit.

Let’s work through what happens here with what we know about async and await so far. When we poll the `forward` generator, it goes through the `while` loop some unknown number of times and eventually returns either `Poll::Ready(())` if the receiver ended, or `Poll::Pending` otherwise. If it returns `Poll::Pending`, the generator contains a future returned from either `rx.next()` or `tx.send(t)`. Those futures both contain a reference to one of the arguments initially provided to `forward` (`rx` and `tx`, respectively), which must also be stored in the generator. But when `try_forward` returns the entire generator, the fields of the generator also move. Thus, `rx` and `tx` no longer reside at the same locations in memory, and the references stored in the stashed-away future are no longer pointing to the right data!

What we’ve run into here is a case of a self-referential data structure: one that holds both data and references into that data. With generators, these self-referential structures are very easy to construct, and being unable to support them would be a significant blow to ergonomics because it would mean you wouldn’t be able to hold references across any yield point. The (ingenious) solution for supporting self-referential data structures in Rust comes in the form of the `Pin` type and the `Unpin` trait. Very briefly, `Pin` is a wrapper type that prevents the wrapped type from being (safely) moved, and `Unpin` is a marker trait that says that the implementing type can be removed safely from a `Pin`.

## Pin

There’s a lot of nuance to cover here, so let’s start with a concrete example of the wrapper `Pin`. In Listing 9-2 I gave you a simplified version of the `Future` trait, but we’re now ready to peel back one part of the simplification. Listing 9-8 shows the `Future` trait somewhat closer to its final form.

```rust
trait Future {
    type Output;
    fn poll(self: Pin<&mut Self>) -> Poll<Self::Output>;
}
```

*Listing 9-8: A less simplified view of the `Future` trait with `Pin`*

In particular, this definition requires that you call `poll` on `Pin<&mut Self>`. Once you have a value behind a `Pin`, that constitutes a contract that that value will never move again. This means that you can construct self-references internally to your heart’s delight, exactly as you want for generators.

> **NOTE** While `Future` makes use of `Pin`, `Pin` is not tied to the `Future` trait—you can use `Pin` for any self-referential data structure.

But how do you get a `Pin` to call `poll`? And how can `Pin` ensure that the contained value won’t move? To see how this magic works, let’s look at the definition of `std::pin::Pin` and some of its key methods, shown in Listing 9-9.

```rust
struct Pin<P> { pointer: P }
impl<P> Pin<P> where P: Deref {
    pub unsafe fn new_unchecked(pointer: P) -> Self;
}
impl<'a, T> Pin<&'a mut T> {
    pub unsafe fn get_unchecked_mut(self) -> &'a mut T;
}
impl<P> Deref for Pin<P> where P: Deref {
    type Target = P::Target;
    fn deref(&self) -> &Self::Target;
}
```

*Listing 9-9: `std::pin::Pin` and its key methods*

There’s a lot to unpack here, and we’re going to have to go over the definition in Listing 9-9 a few times before all the bits make sense, so please bear with me.

First, you’ll notice that `Pin` holds a *pointer type*. That is, rather than hold some `T` directly, it holds a type `P` that dereferences through `Deref` into `T`. This means that rather than have a `Pin<MyType>`, you’ll have a `Pin<Box<MyType>>`, `Pin<Rc<MyType>>`, or `Pin<&mut T>`. The reason for this design is simple—`Pin`’s primary goal is to make sure that once you place a `T` behind a `Pin`, that `T` won’t move, as doing so might invalidate self-references stored in the `T`. If the `Pin` just held a `T` directly, then simply moving the `Pin` would be enough to invalidate that invariant! In the remainder of this section, I’ll refer to `P` as the *pointer* type and `T` as the *target* type.

Next, notice that `Pin`’s constructor, `new_unchecked`, is unsafe. This is because the compiler has no way to actually check that the pointer type indeed promises that the pointed-to (target) type won’t move again. Consider, for example, a variable `foo` on the stack. If `Pin`’s constructor were safe, we could do `Pin::new(&mut foo)`, call a method that requires `Pin<&mut Self>` (and thus assumes that `Self` won’t move again), and then drop the `Pin`. At this point, we could modify `foo` as much as we liked, since it is no longer borrowed—including moving it! We could then pin it again and call the same method, which would be none the wiser that any self-referential pointers it may have constructed the first time around would now be invalid.





## PIN 构造器的安全性

Pin 的构造器之所以不安全，另一个原因是其安全性取决于其自身安全的 trait 的实现。例如，Pin<P> 实现 get_unchecked_mut 的方式是使用 P 的 DerefMut::deref_mut 的实现。虽然对 get_unchecked_mut 的调用是不安全的，但为 P 实现的 DerefMut 并不是不安全的。然而它接收一个 &mut self，因此可以自由地（并且无需不安全代码）移动 T。Drop 也是如此。因此 Pin::new_unchecked 的安全要求不仅是指针类型不会让目标类型再次被移动（如 Pin<&mut T> 示例中那样），而且其 Deref、DerefMut 和 Drop 实现也不会通过它们接收到的 &mut self 移动指向的值。

我们随后得到了 get_unchecked_mut 方法，它会给你一个指向 Pin 指针类型后面 T 的可变引用。此方法也是不安全的，因为一旦我们给出一个 &mut T，调用者就必须承诺不会使用该 &mut T 来移动 T 或以其他方式使它的内存失效，否则任何自引用都会失效。如果此方法不是不安全的，调用者就可以调用一个接受 Pin<&mut Self> 的方法，然后对两个 Pin<&mut _> 调用安全的 get_unchecked_mut 变体，然后使用 mem::swap 交换 Pin 后面的值。如果我们随后再次对任一 Pin 调用接受 Pin<&mut Self> 的方法，其关于 Self 未被移动的假设将被违反，并且它存储的任何内部引用都会失效！

也许令人惊讶的是，Pin<P> 总是实现 Deref<Target = T>，并且这是完全安全的。原因在于 &T 不允许你在不编写其他不安全代码的情况下移动 T（例如 UnsafeCell，我们将在第 10 章讨论）。这是一个很好的例子，说明不安全块的作用域超出了它所包含的代码。如果你在一个应用程序的某个部分编写了代码（不安全地）使用 UnsafeCell 替换了 & 后面的 T，那么该 &T 最初可能来自 Pin<&mut T>，而你现在已经违反了 Pin 后面的 T 永不移动的不变性，即使你在不安全地替换 &T 的地方甚至没有提到 Pin！

## NOTE

> 如果你在阅读本章的同时浏览过 Pin 文档，你可能注意到了 Pin::set，它接受一个 &mut self 和一个 <P as Deref>::Target，并安全地更改 Pin 后面的值。这之所以可能，是因为 set 不会返回之前被固定的值——它只是就地丢弃它并在那里存储新值。因此，它不会违反固定不变性：旧值在被放置在那里之后从未在 Pin 之外被访问过。

## Unpin：安全固定的关键

到目前为止，你可能会问：既然获取可变引用本身就是不安全的，为什么不让 Pin 直接持有 T 呢？也就是说，与其通过指针类型进行间接，不如让 `get_unchecked_mut` 的契约是只有在你没有移动 Pin 时调用它才是安全的。对此问题的答案在于指针设计所实现的一个巧妙的安全使用 Pin。回想一下，我们想要 Pin 的全部原因是为了能够拥有可能包含自身引用的目标类型（如生成器），并向其方法保证目标类型未被移动，因此内部自引用仍然有效。Pin 让我们能够使用类型系统来强制执行该保证，这很棒。但不幸的是，以目前的设计，Pin 在使用上非常笨拙。这是因为即使你正在处理的目标类型不包含任何自引用，因此不在乎是否被移动，它也总是需要不安全代码。

这就是标记 trait Unpin 发挥作用的地方。对某个类型的 Unpin 实现仅仅断言该类型在使用为目标类型时可以安全地从 Pin 中移出。也就是说，该类型承诺它永远不会使用 Pin 关于被引用对象不再被移动的保证，当它用作目标类型时，因此这些保证可以被破坏。Unpin 是一个 auto-trait，像 Send 和 Sync 一样，因此编译器会自动为仅包含 Unpin 成员的任何类型实现它。只有明确选择退出 Unpin 的类型（如生成器）以及包含这些类型的类型才是 !Unpin。

对于 Unpin 的目标类型，我们可以为 Pin 提供一个更简单的安全接口，如清单 9-10 所示。

```
impl<P> Pin<P> where P: Deref, P::Target: Unpin {
    pub fn new(pointer: P) -> Self;
}

impl<P> DerefMut for Pin<P> where P: DerefMut, P::Target: Unpin {
    fn deref_mut(&mut self) -> &mut Self::Target;
}
```

*清单 9-10：针对 Unpin 目标类型的 Pin 的安全 API*

要理解清单 9-10 中的安全 API，请思考清单 9-9 中不安全方法的安全要求：函数 `Pin::new_unchecked` 是不安全的，因为调用者必须承诺被引用对象不能在 Pin 外被移动，并且指针类型的 Deref、DerefMut 和 Drop 实现不会通过它们接收到的引用移动被引用对象。这些要求是为了确保一旦我们给出一个指向 T 的 Pin，我们就永远不会再次移动该 T。但如果 T 是 Unpin，它已声明即使它之前被固定过，被移动也没关系，所以调用者是否满足这些要求都没问题！

同样，`get_unchecked_mut` 是不安全的，因为调用者必须保证不会从 `&mut T` 中移出 T——但对于 T: Unpin，T 已声明即使在被固定后被移动也没问题，所以该安全要求不再重要。这意味着对于 `Pin<P>` 其中 P::Target: Unpin，我们可以简单地为这两个方法提供安全变体（DerefMut 是 get_unchecked_mut 的安全版本）。事实上，我们甚至可以提供一个 `Pin::into_inner`，如果目标类型是 Unpin，就简单地返回拥有的 P，因为 Pin 本质上无关紧要！

## 获取一个 Pin

有了我们对 Pin 和 Unpin 的新理解，我们现在可以开始使用清单 9-8 中要求 Pin<&mut Self> 的新 Future 定义。第一步是构造所需类型。如果 future 类型是 Unpin，那一步很容易——我们只需使用 `Pin::new(&mut future)`。如果它不是 Unpin，我们可以通过两种主要方式固定 future：堆上固定或栈上固定。

让我们从堆上固定开始。Pin 的主要契约是一旦某物被固定，它就不能移动。固定 API 会确保 Pin 的所有方法和 trait 都满足这一点，因此任何构造 Pin 的函数的主要作用是确保如果 Pin 本身移动，被引用值也不会移动。确保这一点最简单的方法是将被引用对象放在堆上，然后只将指向被引用对象的指针放在 Pin 中。你可以随意移动 Pin，但目标仍会保持原位。这就是（安全）方法 `Box::pin` 背后的原理，它接受一个 T 并返回一个 `Pin<Box<T>>`。它没有魔法；它只是断言 Box 遵循 Pin 构造器、Deref 和 Drop 契约。

> **UNPIN BOX**
> 
> 说到 Box，看看 Box 的 Unpin 实现。Box 类型无条件地为任何 T 实现 Unpin，即使该 T 不是 Unpin。这可能让你觉得奇怪，鉴于之前断言 Unpin 是一个 auto-trait，通常只有当类型的全部成员也是 Unpin 时才会为该类型实现。Box 在这方面是一个例外，原因与它能够提供安全 Pin 构造器的原因相同：如果移动一个 Box<T>，你并不会移动 T。换句话说，无条件的实现断言你可以将一个 Box<T> 从 Pin 中移出，即使 T 不能从 Pin 中移出。注意，这并不会让你能够将一个 !Unpin 的 T 从 Pin<Box<T>> 中移出。

另一个选项，栈上固定，要复杂一些，并且在撰写本文时需要一点点不安全代码。我们必须确保在 Pin 被丢弃后，不能再通过 `&mut` 访问被固定的值。我们通过在清单 9-11 中所示的宏中对值进行遮蔽来实现这一点，或使用提供此宏的 crate 中的一个。有一天它甚至可能进入标准库！

```
macro_rules! pin_mut {
    ($var:ident) => {
        let mut $var = $var;
        let mut $var = unsafe { Pin::new_unchecked(&mut $var) };
    }
}
```

*清单 9-11：用于栈上固定的宏*





通过将要钉在栈上的变量名作为参数，宏确保调用者已经在栈上的某个位置拥有想要钉住的值。`$var` 的遮蔽确保调用者无法丢弃 `Pin` 后继续使用未钉住的值（这对于任何 `!Unpin` 的目标类型都会破坏 `Pin` 契约）。通过移动 `$var` 中存储的值，宏还确保调用者无法在不丢弃原始变量的情况下丢弃宏声明的 `$var` 绑定。具体来说，如果没有那一行，调用者可以写（注意额外的作用域）：

```rust
let foo = /* */; { pin_mut!(foo); foo.poll() }; foo.mut_self_method();
```

在这里，我们向 `poll` 提供了 `foo` 的钉住实例，但随后我们又在没有 `Pin` 的情况下使用 `foo` 的 `&mut`，这违反了 `Pin` 契约。另一方面，有了额外的重新赋值，这段代码也会将 `foo` 移动到新的作用域中，使其在作用域结束后再也无法使用。

因此，栈上钉住需要不安全代码，不像 `Box::pin`，但避免了 `Box` 引入的额外分配，并且在 `no_std` 环境中也能工作。

## 回到未来

我们现在有了钉住的 future，并且我们知道这意味着什么。但你可能已经注意到，大多数使用 `async` 和 `await` 编写的异步代码中都没有出现这些重要的钉住操作。而这是因为编译器把它隐藏起来了。

还记得我们讨论清单 9-5 时，我告诉你 `<expr>.await` 会反糖化成类似这样的代码吗：

```rust
loop {
    if let Poll::Ready(r) = expr.poll() {
        break r
    } else {
        yield
    }
}
```

那是一个稍微简化的说法，因为正如我们所见，只有在你拥有 future 的 `Pin<&mut Self>` 时才能调用 `Future::poll`。反糖化实际上更复杂一些，如清单 9-12 所示。

```rust
① match expr {
    mut pinned => loop {
        ② match unsafe { Pin::new_unchecked(&mut pinned) }.poll() {
            Poll::Ready(r) => break r,
            Poll::Pending => yield,
        }
    }
}
```

清单 9-12：`<expr>.await` 更准确的反糖化

`match` ① 是一个巧妙的简写，确保展开后仍然是一个有效表达式，同时将表达式结果移动到一个我们可以随后在栈上钉住的变量中。除此之外，主要的新增部分是对 `Pin::new_unchecked` ② 的调用。该调用是安全的，因为要对包含的 async 块进行轮询，它必须已经因 `Future::poll` 的签名而被钉住。而我们正是由于对 async 块进行了轮询才到达了对 `Pin::new_unchecked` 的调用，因此生成器状态是被钉住的。由于 `pinned` 存储在对应 async 块的生成器中（它必须是这样才能使 `yield` 正确恢复），我们知道 `pinned` 不会再移动。

此外，一旦进入循环，`pinned` 只能通过 `Pin` 访问，因此没有任何代码能够从 `pinned` 中的值中移出。我们满足了 `Pin::new_unchecked` 的所有安全要求，代码是安全的。

## 去睡觉

我们对 `Pin` 深入到了细节中，但现在走出来后，关于 futures 的另一个问题可能会让你感到困惑。如果对 `Future::poll` 的调用返回 `Poll::Pending`，你需要有东西在稍后时间再次调用 `poll` 来检查是否可以取得进展。这个东西通常被称为 executor。你的 executor 可以是一个简单的循环，不断轮询你正在等待的所有 futures，直到它们都返回 `Poll::Ready`，但这会浪费大量本可用于更有用事情的 CPU 周期，比如运行你的网页浏览器。相反，我们希望 executor 做它能做的任何有用工作，然后去睡觉。它应该保持睡眠，直到其中一个 future 可以取得进展，然后才醒来再做一次遍历，然后再次去睡觉。

## 醒来

决定何时重新检查某个 future 的条件差异很大。它可能是“当网络数据包到达此端口时”、“当鼠标光标移动时”、“当有人向此通道发送时”、“当 CPU 收到特定中断时”，甚至是“在经过这段时间后”。除此之外，开发者可以编写自己的 future 来包装多个其他 futures，因此可能有多个唤醒条件。某些 futures 甚至可能引入自己完全自定义的唤醒事件。

为了适应这些众多用例，Rust 引入了 `Waker` 的概念：一种唤醒 executor 以发出可以取得进展的信号的方式。`Waker` 是使 futures 周围整个机制工作的关键。executor 构造一个与 executor 用于睡觉的机制集成的 `Waker`，并在轮询任何 `Future` 时将其传入。怎么做？通过我到目前为止一直对你隐瞒的 `Future::poll` 的额外参数。抱歉。清单 9-13 给出了 `Future` 的最终真实定义——不再有谎言了！

```rust
trait Future {
    type Output;
    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}
```

清单 9-13：带有 `Context` 的实际 `Future` trait

`&mut` `Context` 包含 `Waker`。参数是 `Context` 而不是直接的 `Waker`，这样我们就可以在必要时为 futures 增加额外的上下文。

`Waker` 的主要方法是 `wake`（以及按引用变体 `wake_by_ref`）。`wake` 方法不接受参数，其效果完全由构造该 `Waker` 的 executor 定义。你看，`Waker` 实际上是关于 executor 的泛型。或者更准确地说，构造 `Waker` 的东西可以决定在调用 `Waker::wake` 时发生什么，在克隆 `Waker` 时发生什么，以及在丢弃 `Waker` 时发生什么。所有这些都通过手动实现的 vtable 实现，其功能类似于我们在第 3 章很久以前讨论的动态分发。

构造 `Waker` 是一个相当复杂的过程，其机制对于使用它来说并不是那么重要，但你可以在标准库中的 `RawWakerVTable` 类型中看到构建块。它有一个构造函数，接受 `wake` 和 `wake_by_ref` 的函数指针，以及 `Clone` 和 `Drop`。`RawWakerVTable` 通常在 executor 的所有 wakers 之间共享，与一个旨在保存每个 `Waker` 实例特定数据（比如它属于哪个 future）的原始指针捆绑在一起，并被转换为 `RawWaker`。然后它被传递给 `Waker::from_raw` 以生成一个安全的 `Waker`，可以传递给 `Future::poll`。

## Poll 契约

到目前为止，我们一直回避 future 实际上如何使用 `Waker`。这个想法相当简单：如果 `Future::poll` 返回 `Poll::Pending`，那么它是 future 的责任确保 *某物* 在 future 下次能够取得进展时调用提供的 `Waker` 上的 `wake`。大多数 futures 通过仅在其他 future 也返回 `Poll::Pending` 时才返回 `Poll::Pending` 来维护此属性；以这种方式它可以轻易满足 `poll` 的契约，因为内部 future 必须遵循相同的契约。但不能无止境地向下有乌龟。最终，你会遇到一个不轮询其他 futures 的 future，而是做类似写入网络套接字或尝试从通道接收之类的操作。这些通常被称为 *叶子 future*，因为它们没有子 future。叶子 future 没有内部 future，而是直接表示某个可能尚未准备好返回结果的资源。

叶子 future 通常有两种形状：等待来自同一进程内事件的（如通道接收器），以及等待来自进程外部事件的（如 TCP 数据包接收）。等待内部事件的那些通常遵循相同的模式：将 `Waker` 存储在唤醒你的代码可以找到它的地方，并让该代码在生成相关事件时调用 `wake`。例如，考虑一个必须等待内存通道上消息的叶子 future。它将 `Waker` 存储在发送者和接收者之间共享的通道部分中，然后返回 `Poll::Pending`。当发送者稍后到来并向通道注入消息时，它会注意到等待接收者留下的 `Waker` 并在 `Waker` 上调用 `wake`


在从 `send` 返回之前。现在，接收者被唤醒，轮询契约得到了履行。

处理外部事件的叶 future 要复杂得多，因为生成它们所等待事件的代码对 future 或唤醒器一无所知。最典型的情况是，生成代码是操作系统内核，它知道磁盘何时就绪或定时器何时到期；但也可能是某个 C 库，在操作完成时回调 Rust，或者其他类似的外部实体。包装此类外部资源的叶 future 可以专门启动一个线程来执行阻塞式系统调用（或等待 C 回调），然后使用内部唤醒机制，但这会很浪费；每次操作需要等待时你都要启动一个线程，最终会留下大量一次性线程闲在那里等待各种事情。

相反，执行器往往会提供叶 future 的实现，这些实现在后台与执行器通信，以安排与操作系统的适当交互。具体如何编排取决于执行器和操作系统，但粗略地说，执行器会跟踪所有它在下一次进入睡眠时需要监听的事件源。当叶 future 意识到自己必须等待某个外部事件时，它会更新执行器的状态（它知道这个状态，因为该状态由执行器 crate 提供），将那个外部事件源连同它的 `Waker` 一起纳入其中。当执行器无法再取得进展时，它会收集各个处于挂起状态的叶 future 正在等待的所有事件源，然后向操作系统发起一次大型阻塞调用，告诉操作系统：只要有叶 future 等待的资源出现新事件，就返回。在 Linux 上，这通常通过 `epoll` 系统调用实现；Windows、各种 BSD、macOS 以及几乎所有其他操作系统都提供类似的机制。当该调用返回时，执行器会对所有与操作系统报告了事件的事件源相关联的唤醒器调用 `wake`，轮询契约由此得到履行。

> **注意**  
> 反应堆 *是* 执行器的一部分，叶 future 会向它注册事件源，执行器在没有更多有用工作可做时也会等待它。将执行器与反应堆分开是可能的，不过将它们捆绑在一起通常会提升性能，因为两者更容易协同优化。

叶 future 与执行器之间的紧密集成有一个连锁效应：来自某个执行器 crate 的叶 future 往往无法与不同的执行器一起使用。或者至少，除非叶 future 对应的执行器 *也* 在运行，否则它们无法使用。当叶 future 去存储自己的 `Waker` 并注册它正在等待的事件源时，它针对其构建的执行器必须已经设置了相应状态并处于运行状态，这样事件源才会真正被监控，`wake` 也最终会被调用。有一些办法可以绕过这个问题，例如如果没有执行器在运行，就让叶 future 生成一个执行器，但这并不总是可取的，因为这意味着应用程序会不知不觉地同时运行多个执行器，从而降低性能，也意味着调试时你必须检查多个执行器的状态。

希望支持多个执行器的库 crate 必须在其叶资源上做到泛型化。例如，库可以不使用某个特定执行器的 `TcpStream` 或 `File` future 类型，而是存储一个泛型 `T: AsyncRead + AsyncWrite`。然而，生态尚未确定这些 trait 到底应该是什么样、需要哪些 trait，所以目前要让代码真正跨执行器泛型是相当困难的。例如，虽然 `AsyncRead` 和 `AsyncWrite` 在生态中多少算是常见（或者必要时可以轻松适配），但目前还没有用于在后台运行 future（即 spawn，我们稍后会讨论）或用于表示定时器的 trait。

## “唤醒”这个词并不恰当

你可能已经意识到，`Waker::wake` 看起来未必会唤醒任何东西。例如，对于外部事件（如上一节所述），执行器本就已经处于唤醒状态，再让它对一个本来就属于该执行器的 `Waker` 调用 `wake`，看起来确实有点傻！事实上，`Waker::wake` 这个名称多少有些名不副实——实际上，它表示某个特定的 future 已可运行。也就是说，它告诉执行器：当执行器有空处理它时，应确保轮询这个特定的 future，而不是再次进入睡眠，因为这个 future 可以取得进展。如果执行器当前正在睡眠，这可能会唤醒执行器，使其去轮询那个 future，但这更像是副作用，而非其主要目的。

执行器必须知道哪些 future 是可运行的，原因有二。第一，它需要知道何时可以停止轮询某个 future 并进入睡眠；仅仅轮询每个 future 直到其返回 `Poll::Pending` 是不够的，因为轮询后一个 future 可能会让前一个 future 得以推进。考虑这样一个场景：两个 future 通过通道来回向对方传递消息。当你轮询其中一个时，另一个变得就绪，反之亦然。在这种情况下，执行器绝不应该进入睡眠，因为总是有更多工作要做。

第二，知道哪些 future 可运行能让执行器避免不必要的轮询。如果一个执行器管理着数千个挂起的 future，它不应该仅仅因为一个事件使其中某一个 future 可运行，就去轮询所有 future。如果真那样做，执行异步代码会变得非常慢。

## 任务

异步程序中的 future 会形成一棵树：一个 future 可能包含任意数量的其他 future，而这些 future 又可能包含其他 future，一路向下直到与唤醒器交互的叶 future。每棵树的根，就是你交给执行器主“run”函数的那个 future。这些根 future 被称为任务，它们是执行器与 future 树之间唯一的接触点。执行器对任务调用 `poll`，从那时起，每个被包含的 future 的代码都必须确定作为回应要轮询哪些内部 future，一路向下直到相关的叶。

执行器通常会为它们轮询的每个任务构造一个独立的 `Waker`，这样稍后调用 `wake` 时，它们就知道是哪个任务刚刚变为可运行状态，并将其标记为可运行。这正是 `RawWaker` 中裸指针的用途——在共享各种 `Waker` 方法的代码的同时，区分不同的任务。

当执行器最终轮询一个任务时，该任务会从 `Future::poll` 实现的顶部开始运行，并且必须从那里决定如何到达更深层的、现在可以取得进展的 future。由于每个 future 只知道自己的字段，对整棵树一无所知，这一切都是通过一次次 `poll` 调用完成的，每次调用遍历树中的一条边。

选择轮询哪个内部 future 通常显而易见，但并非总是如此。在 async/await 的情况下，要轮询的 future 就是我们被阻塞等待的那个。但在一个等待多个 future 中第一个取得进展（通常称为 select）或等待一组 future 全部完成（通常称为 join）的 future 中，就有很多选择了。必须做出这种选择的 future 基本上就是一个子执行器。它可以轮询所有内部 future，但这样做可能相当浪费。因此，这些子执行器通常会在对任何内部 future 调用 `poll` 之前，先用它们自己的 `Waker` 类型包装在 `poll` 的 `Context` 中收到的 `Waker`。在包装代码中，它们会先在自己的内部状态中将刚刚轮询过的 future 标记为可运行，然后再对原始的 `Waker` 调用 `wake`。这样一来，当执行器最终再次轮询该子执行器 future 时，子执行器就可以查看自己的内部状态，弄清楚是哪个内部 future 导致了当前这次 `poll` 调用，然后只轮询这些 future。

## 用 `spawn` 将所有内容串联起来

在使用异步执行器时，你可能会遇到一个生成 future 的操作。现在我们终于可以来探讨它的含义了！让我们通过一个例子来说明。首先，看一下清单 9-14 中的简单服务器实现。

```rust
async fn handle_client(socket: TcpStream) -> Result<()> {
    // Interact with the client over the given socket.
}

async fn server(socket: TcpListener) -> Result<()> {
    while let Some(stream) = socket.accept().await? {
        handle_client(stream).await?;
    }
}
```

清单 9-14：顺序处理连接

顶层 server 函数本质上是一个大型 future，它监听新连接，并在新连接到来时执行某些操作。你把这个 future 交给执行器，说“运行这个”，而且由于你不想让程序随后立即退出，你可能会让


你的客户端 future 是 Send，它可以并行运行，因为它可以持有对不同任务的 &mut。清单 9-16 给出了这可能是什么样子的示例。

```
async fn server(socket: TcpListener) -> Result<()> {
    while let Some(stream) = socket.accept().await? {
        // Spawn a new task with the Future that represents this client.
        // The current task will continue to just poll for more connections,
        // and will run concurrently (and possibly in parallel) with handle_client.
        spawn(handle_client(stream));
    }
}
```

清单 9-16：通过生成 future 来创建可以并发轮询的更多任务

当你生成一个 future，从而使其成为一个任务时，这有点像生成一个线程。future 会在后台继续运行，并在执行器中与任何其他任务并发多路复用。然而，与生成的线程不同，生成的任务仍然依赖于被执行器轮询。如果执行器停止运行，无论是因为你丢弃了它还是因为你的代码不再运行执行器的代码，那么这些生成的任务将停止取得进展。在服务器示例中，想象一下如果主服务器 future 因某种原因得到解决会发生什么。既然执行器已将控制权交还给你的代码，它就无法继续做，嗯，任何事情。多线程执行器通常会生成在执行器将控制权交还给用户代码后仍继续轮询任务的后台线程，但并非所有执行器都这样做，因此在依赖这种行为之前请先检查你的执行器！

## ASYNC 代码中的阻塞

你必须小心在异步代码中调用同步代码，因为执行器线程在执行当前任务所花费的任何时间，都是它没有花费在运行其他任务的时间。如果一个任务在一段较长时间内占用当前线程而没有将控制权交还给执行器，这可能发生在执行阻塞系统调用（如 std::sync::sleep）或在没有 await 的紧密循环中运行时，那么当前执行器线程负责的其他任务在那段时间内将无法运行。通常，这表现为某些任务能够取得进展的时间（如客户端连接时）与它们实际执行之间存在很长的延迟。

一些多线程执行器实现了工作窃取技术，其中空闲的执行器线程会从繁忙的执行器线程窃取任务，但这更多是一种缓解措施，而非解决方案。最终，你可能会陷入所有执行器线程都被阻塞的情况，因此在其中一个阻塞操作完成之前，没有任务会被运行。

总的来说，你应该非常小心在异步上下文中执行计算密集型操作或调用可能阻塞的函数。

continued

此类操作应尽可能转换为异步操作，或在专用线程上执行，然后使用支持异步的原语（如通道）进行通信。一些执行器还提供了用于指示异步代码的特定段可能会阻塞的机制，或用于在可能否则不会让出的循环上下文中自愿让出，这可以构成解决方案的一部分。一个好的经验法则是，没有任何 future 应该能够在不返回 Poll::Pending 的情况下运行超过 1 ms。

## 总结

在本章中，我们深入了解了 Rust 中可用的异步构造的幕后实现。我们看到了编译器如何实现生成器和自引用类型，以及为什么这项工作对于支持我们现在所知的 async/await 是必要的。我们还探讨了 future 如何被执行，以及唤醒器如何允许执行器在任何给定时刻只有其中一些可以取得进展时在任务之间进行多路复用。在下一章中，我们将 tackling 也许是 Rust 中最深奥和最受讨论的领域：unsafe 代码。深呼吸，然后翻页。

