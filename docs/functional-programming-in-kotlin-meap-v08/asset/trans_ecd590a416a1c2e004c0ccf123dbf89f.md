
originate from here, it will release the latch and make the result available immediately.

### Listing 7.11 实现 `run` 方法以适应非阻塞 `Par`

```kotlin
fun <A> run(es: ExecutorService, pa: Par<A>): A {
    val ref = AtomicReference<A>()
    val latch = CountDownLatch(1)
    pa(es).invoke { a: A ->
        ref.set(a)
        latch.countDown()
    }
    latch.await()
    return ref.get()
}
```

- 1. 创建一个可变、线程安全的引用来存储结果。
- 2. `CountDownLatch` 会阻塞线程，直到倒计时达到 0。
- 3. 在接收到结果时设置结果并释放闩锁。
- 4. 等待结果可用且闩锁被释放。
- 5. 一旦我们通过了闩锁，就知道 `ref` 已被设置并返回其值。

在我们当前的实现中，`run` 会在等待 `latch` 被释放时阻塞调用线程。事实上，不可能编写一个不阻塞的 `run` 实现。我们的 方法必须等待 A 的值 materialize 之后才能返回任何内容。出于这个原因，我们希望 API 的使用者在确实想要等待结果时才调用 `run`。我们甚至可以进一步将 `run` 完全从 API 中移除，并公开 `Par` 上的 `invoke` 方法，以便使用者注册异步回调。那当然是一个有效的设计选择，但我们目前仍保持 API 不变。

如果 `latch` 只需要释放一次，上面的函数可以使用 `CompletableFuture` 简化。`CompletableFuture` 类是自 Java 8 起 JDK 中 `Future` 接口的非抽象实现。它使开发人员能够完全控制结果的可用性，同时包含 `ExecutorService` 方法返回的任何 `Future` 实现提供的所有线程阻塞管理。

### Listing 7.12 使用 `CompletableFuture` 实现 `run` 方法

```kotlin
fun <A> run2(es: ExecutorService, pa: Par<A>): A {
    val ref = CompletableFuture<A>()
    pa(es).invoke { a: A ->
        ref.complete(a)
    }
    return ref.get()
}
```

- 1. 创建一个 `CompletableFuture` 来管理当前线程的阻塞并存储结果。
- 2. 设置结果。这会解锁 `CompletableFuture` 并使其结果可用。

等待结果可用，然后返回其值。

从此片段可以看出，`CountDownLatch` 不再必要，因为线程阻塞由 `CompletableFuture` 管理。

我们已经在列表 7.10 中看到过 unit，但 fork 呢？这就是我们引入实际并行的地方：

### Listing 7.13 fork 函数分叉出一个任务来评估惰性参数

```kotlin
fun <A> fork(a: () -> Par<A>): Par<A> =
  { es: ExecutorService ->
      object : Future<A>() {
        override fun invoke(cb: (A) -> Unit) =
          eval(es) { a()(es).invoke(cb) }
      }
  }

fun eval(es: ExecutorService, r: () -> Unit) {
  es.submit(Callable { r() })
}
```

调用 eval 会分叉出 a 的求值并立即返回。

一个辅助函数，用于使用某个 ExecutorService 异步求值一个动作。

当 fork 返回的 Future 接收到其 continuation cb 时，它会分叉出一个任务来评估惰性参数 a。一旦参数被求值并调用以产生一个 Future<A>，我们就会注册 cb，以便在该 Future 具有其结果 A 时调用它。

让我们考虑 map2。回想一下此组合子的签名。

```
fun <A, B, C> map2(pa: Par<A>, pb: Par<B>, f: (A, B) -> C): Par<C>
```

这里，非阻塞实现要复杂得多。概念上，我们希望 map2 并行运行两个 Par 参数。当两个结果都到达时，我们希望调用 f，然后将结果 c 传递给 continuation。但这里有几个竞态条件需要担心，并且仅使用 java.util.concurrent 提供的低级原语就很难实现一个正确的非阻塞实现。

## 简要插曲演示 ACTORS 的使用

为了实现 map2，我们将使用一种称为 actors 的非阻塞并发原语。Actor 本质上是一个并发进程，它不会 constantly 占用一个线程。相反，它只有在接收到消息时才会占用一个线程。重要的是，虽然多个线程可能并发地向 actor 发送消息，但 actor 一次只处理一条消息，将其他消息排队以供后续处理。这使得它们在编写必须被多个线程访问且否则容易出现竞态条件或死锁的棘手代码时作为并发原语很有用。

最好用一个例子来说明这一点。许多 actor 的实现都足以满足我们的目的，但为了简单起见，我们将使用随章节代码一起提供的我们自己的最小 actor 实现，文件为 actor.kt。我们将通过一些客户端代码与之交互，以了解其工作原理。我们首先启动 actor。

### Listing 7.14 设置一个 actor 来处理客户端请求

```kotlin
val es: ExecutorService = Executors.newFixedThreadPool(4)
val s = Strategy.from(es)
val echoer = Actor<String>(s) {
    println("got message: $it")
}
```

- 创建一个 `ExecutorService` 实例 `es` 来支持我们的 actor
- 将 `es` 包装在名为 `s` 的 `Strategy` 中
- 使用 `Strategy` 启动一个 actor，并向其传递一个匿名处理函数。

现在我们有了由 echoer 引用的 actor 实例，我们可以发送一些消息。

```kotlin
echoer.send("hello")
//got message: hello

echoer.send("goodbye")
//got message: goodbye

echoer.send("You're just repeating everything I say, aren't you?")
//got message: You're just repeating everything I say, aren't you?
```

- 向 actor 发送 "hello" 消息。
- 生成的进程会调用处理程序，立即释放当前线程以处理下一个消息。
- 在未等待 "hello" 处理程序完成的情况下，向 Actor 发送新的 "goodbye" 消息。

理解 Actor 实现并非必需。正确且高效的实现相当微妙，但如果你好奇，请参阅章节代码中的 actor.kt 文件。实现只有不到 100 行普通的 Kotlin 代码。理解 actor 实现最困难的部分在于多个线程可能同时向 actor 发送消息。实现需要确保消息一次处理一条，并且发送给 actor 的所有消息最终都会被处理，而不是无限期地排队。即使如此，代码仍然简洁。

## 通过 ACTORS 实现 MAP2

我们现在可以使用 Actor 来收集两个参数的结果来实现 map2。代码相当直接，并且无需担心竞态条件，因为我们知道 Actor 将一次只处理一条消息。

```kotlin
fun <A, B, C> map2(pa: Par<A>, pb: Par<B>, f: (A, B) -> C): Par<C> =
  { es: ExecutorService ->
    object : Future<C>() {
      override fun invoke(cb: (C) -> Unit) {
        val ar = AtomicReference<Option<A>>(None)
        val br = AtomicReference<Option<B>>(None)
        val combiner =
          Actor<Either<A, B>>(Strategy.from(es)) { eab ->
            when (eab) {
              is Left<A> ->
                br.get().fold(
                  { ar.set(Some(eab.a)) },
                  { b -> eval(es) { cb(f(eab.a, b)) } }
                )
              is Right<B> ->
                ar.get().fold(
                  { br.set(Some(eab.b)) },
                  { a -> eval(es) { cb(f(a, eab.b)) } }
                )
            }
          }
        pa(es).invoke { a: A -> combiner.send(Left(a)) }
        pb(es).invoke { b: B -> combiner.send(Right(b)) }
      }
    }
  }
```

- 1. 使用两个 `AtomicReference` 实例来存储可变结果
- 2. 一个 actor 等待两个结果，使用 `f` 将它们组合，并将结果传递给 `cb`
- 3. 接收到 `Left(a)` 时执行的分支，如果 `br` 中之前已设置 `Right(b)` 则进行组合。
- 4. 接收到 `Right(b)` 时执行的分支，如果 `ar` 中之前已设置 `Left(a)` 则进行组合。
- 5. 将 actor 作为 continuation 传递给两侧。

我们在 `combiner` actor 中有四个需要处理的可能场景。让我们逐一查看：

- 如果 A 结果先到达，则将其存储在 ar 中，actor 现在等待 B 到达。
- 如果 A 结果最后到达且 B 已存在，则结果 a 和 b 由 f 组合为类型 C 并传递给回调 cb。
- 如果 B 结果先到达，则将其存储在 br 中，actor 现在等待 A 到达。
- 如果 B 结果最后到达且 A 已存在，则结果 a 和 b 由 f 组合为类型 C 并传递给回调 cb。

然后 actor 被作为 continuation 传递给两侧。它在是 A 的情况下被包装为 `Left`，在是 B 时为 `Right`。我们使用 `Either` 数据类型，为此联合的每一侧调用 `Left(a)` 和 `Right(b)` 构造函数。它们用于指示结果的来源给 actor。

鉴于这些实现，我们现在应该能够运行任意复杂度的 `Par` 值，而无需担心用完线程，即使 actor 只有单个 JVM 线程可用。
