
```
fork { sum(l) },
    fork { sum(r) }
) { lx: Int, rx: Int -> lx + rx }
}
```

With fork, we can now make map2 strict, leaving it up to the programmer to wrap arguments if they wish. A function like fork solves the problem of instantiating our parallel computations too strictly, but more fundamentally it puts the parallelism explicitly under programmer control. We’re addressing two concerns here. The first is that we need some way to indicate that the results of the two parallel tasks should be combined. Apart from this, we have the choice of whether a particular task should be performed asynchronously. By keeping these concerns separate, we avoid having any sort of global policy for parallelism attached to map2 and other combinators we write, which would mean making tough and ultimately arbitrary choices about what global policy is best.

Let’s now return to the question of whether unit should be strict or lazy. With fork, we can now make unit strict without any loss of expressiveness. A non-strict version of it, let’s call it lazyUnit, can be implemented using unit and fork.

## Listing 7.6 A strict unit and fork can be combined to form a lazy variant of unit.

```
fun <A> unit(a: A): Par<A> = Par(a)

fun <A> lazyUnit(a: () -> A): Par<A> =
    fork { unit(a()) }
```

The function lazyUnit is a simple example of a derived combinator, as opposed to a primitive combinator like unit. We were able to define lazyUnit in terms of other operations only. Later, when we pick a representation for Par, lazyUnit won’t need to know anything about this representation—its only knowledge of Par is through the operations fork and unit that are defined on Par.

We know we want fork to signal that its argument gets evaluated in a separate logical thread. But we still have the question of whether it should begin doing so immediately upon being called, or else hold on to its argument to be evaluated in a logical thread later when the computation is forced using something like get. In other words, should evaluation be the responsibility of fork or of get? Should evaluation be eager or lazy? When you’re unsure about a meaning to assign to some function in your API, you can always continue with the design process—at some point later the trade-offs of different choices of meaning may become clear. Here we make use of a helpful trick—we’ll think about what sort of information is required to implement fork and get with various meanings.

If fork begins evaluating its argument immediately in parallel, the implementation must clearly know something, either directly or indirectly, about how to create threads or submit tasks to some sort of thread pool. This implies that the thread pool or whatever resource we use to
implement the parallelism must be globally accessible and properly initialized wherever we want to call fork. This means we lose the ability to control the parallelism strategy used for different parts of our program. And though there’s nothing wrong with having a global resource for executing parallel tasks, we can imagine how it would be useful to have more fine-grained control over what implementations are used and in what context. For instance, we might like each subsystem of a large application to get its own thread pool with different parameters. As we consider this, it seems much more appropriate to give get, the responsibility of creating threads and submitting execution tasks.

Note that coming to these conclusions didn’t require knowing exactly how fork and get would be implemented, nor even what the representation of Par would be. We just reasoned informally about the sort of information required to actually spawn a parallel task, then examined the consequences of having Par values know about this information.

If fork simply holds on to its unevaluated argument until later, it requires no access to the mechanism for implementing parallelism. It just takes an unevaluated Par and “marks” it for concurrent evaluation. With this model, Par itself doesn’t need to know how to actually implement the parallelism. It’s more a description of a parallel computation that gets interpreted at a later time by something like the get function. This is a shift from before, where we were considering Par to be a container of a value that we could simply get when it becomes available. Now it’s more of a first-class program that we can run. Keeping this new discovery in mind, let’s rename our get function to run, and dictate that this is where the parallelism actually gets implemented.

```
fun <A> run(a: Par<A>): A = TODO()
```

Because Par is now just a pure data structure, run has to have some means of implementing the parallelism, whether it spawns new threads, delegates tasks to a thread pool, or uses some other mechanism.

## 7.2 Picking a representation

Just by exploring this simple example and thinking through the consequences of different choices, we’ve arrived at the following API.

```
fun <A> unit(a: A): Par<A> ①

fun <A, B, C> map2(
    a: Par<A>,
    b: Par<B>,
    f: (A, B) -> C
): Par<C> ①

fun <A> fork(a: () -> Par<A>): Par<A> ②

fun <A> lazyUnit(a: () -> A): Par<A> ③

fun <A> run(a: Par<A>): A ⑤
```

- 1. Create a computation that immediately results in the value a
- 2. Combine the results of two parallel computations with a binary function.
- 3. Mark a computation for concurrent evaluation by `run`.
- 4. Wrap expression a for concurrent evaluation by `run`.
- 5. Fully evaluate a given `Par`, spawning computations and extracting value.

## EXERCISE 7.2

At any point while evolving an API, you can start thinking about possible *representations* for the abstract types that appear. Try to come up with a representation for `Par` that makes it possible to implement the functions of our API.

Let’s see if we can come up with a representation together. We know that `run` somehow needs to execute asynchronous tasks. We could write our own low-level API, but there’s already a class that we can use in the Java Standard Library, `java.util.concurrent.ExecutorService`. Here is what the API looks like, roughly paraphrased in Kotlin.

## Listing 7.7 Executor API represented in Kotlin.

```
```kotlin
interface Callable<A> {
    fun call(): A
}

interface Future<A> {
    fun get(): A
    fun get(timeout: Long, timeUnit: TimeUnit): A
    fun cancel(evenIfRunning: Boolean): Boolean
    fun isDone(): Boolean
    fun isCancelled(): Boolean
}

interface ExecutorService {
    fun <A> submit(c: Callable<A>): Future<A>
}
```
```

The `ExecutorService` allows us to submit a `Callable`, this being the equivalent to a lazy argument, or thunk, in Kotlin. The result from calling `submit` will be a `Future`, which is a handle to a computation that is potentially running in a new thread. We can query the `Future` for a result by calling one of its blocking `get` methods. It also sports some additional methods for canceling and querying its current state.

## SIDEBAR

## The problem with using concurrency primitives directly

What of `java.lang.Thread` and `Runnable`? Let’s take a look at these classes. Here’s a partial excerpt of their API, paraphrased in Kotlin:

```
```kotlin
interface Runnable {
    fun run(): Unit
}

class Thread(r: Runnable) {
    fun start(): Unit = TODO()
    fun join(): Unit = TODO()
}
```
```

- ① Begins running r in a separate thread.
- ② Blocks the calling thread until r finishes running.

Already, we can see a problem with both of these types—none of the methods return a meaningful value. Therefore, if we want to get any information out of a `Runnable`, it has to have some side effect, like mutating some state that we can inspect. This is bad for compositionality—we can’t manipulate `Runnable` objects generically since we always need to know something about their internal behavior. `Thread` also has the disadvantage that it maps directly onto operating system threads, which are a scarce resource. It would be preferable to create as many “logical threads” as is natural for our problem, and later deal with mapping these onto actual OS threads.

You can handle this kind of thing by using something like `java.util.concurrent.Future`, `ExecutorService` and friends. Why don’t we use them directly? Here’s a paraphrased portion of their API:

```
```kotlin
class ExecutorService {
    fun <A> submit(a: Callable<A>): Future<A> = TODO()
}

interface Future<A> {
    fun get(): A
}
```
```

Though `java.util.concurrent` is a tremendous help in abstracting over physical threads, these primitives are still at a much lower level of abstraction than the library we want to create in this chapter. A call to `Future.get`, for example, blocks the calling thread until the `ExecutorService` has finished executing it, and its API provides no means of *composing* futures. Of course, we can build the implementation of our library on top of these tools (and this is what we end up doing later in the chapter), but they don’t present the modular and compositional API that we’d want to use directly from functional programs.
