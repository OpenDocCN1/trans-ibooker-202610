
Now try to imagine how we could modify run in our Par data type if we had access to an instance of the ExecutorService.

```
fun <A> run(es: ExecutorService, a: Par<A>): A = TODO()
```

The simplest possible way of expressing Par<A> might be to turn it into a type alias of a function such as (ExecutorService) -> A. If we invoke this function with an instance of an ExecutorService, it produces something of type A, making the implementation trivial. We could improve this further by giving the caller of run the ability to defer the decision of how long to wait for a computation, or even to cancel it all together. With this in mind, Par<A> becomes (ExecutorService) -> Future<A> with run simply returning the Future<A>.

```
typealias Par<A> = (ExecutorService) -> Future<A>

fun <A> run(es: ExecutorService, a: Par<A>): Future<A> = a(es)
```

Note that since Par is now represented by a function that needs an ExecutorService, the creation of the Future doesn’t actually happen until this ExecutorService is provided.

Is it really that simple? Let’s assume it is for now, and revise our model if we find that it doesn’t fulfill our requirements in the future.

## 7.3 Refining the API with the end user in mind

The way we’ve worked so far is a bit artificial. In practice, there aren’t such clear cut boundaries between designing the API and choosing a representation with one preceding the other. Ideas for a representation can drive the API design, but the opposite may also happen—the choice of API can drive the representation. It is natural to shift fluidly between these two perspectives, run experiments when questions arise, build prototypes, and so on.

We’ll devote this section to exploring and refining our API. Though we’ve already obtained a lot of mileage out of evolving this simple example, let’s try to learn more about what we can express using the primitive operations that we already built before adding any new ones. With our primitives and their chosen meanings, we’ve carved out a small universe for ourselves. We now get to discover what ideas can be expressed in this universe. This can, and should be a fluid process—we can change the rules of our universe at any time, make a fundamental change to our representation or introduce a new primitive, all while observing how our creation subsequently behaves.

We will begin by implementing the functions of the API that we’ve developed up to this point. Now that we have a representation for Par, a first attempt should be straightforward. What follows is a naive implementation using this initial representation.

## Listing 7.8 Primitive operations for Par can be found in the Pars object

```
object Pars {
    fun <A> unit(a: A): Par<A> =
        { es: ExecutorService -> UnitFuture(a) }

    data class UnitFuture<A>(val a: A) : Future<A> {
        override fun get(): A = a
        override fun get(timeout: Long, timeUnit: TimeUnit): A = a
        override fun cancel(evenIfRunning: Boolean): Boolean = false
        override fun isDone(): Boolean = true
        override fun isCancelled(): Boolean = false
    }

    fun <A, B, C> map2(
        a: Par<A>,
        b: Par<B>,
        f: (A, B) -> C
    ): Par<C> =
        { es: ExecutorService ->
            val af: Future<A> = a(es)
            val bf: Future<B> = b(es)
            UnitFuture(f(af.get(), bf.get()))
        }

    fun <A> fork(
        a: () -> Par<A>
    ): Par<A> =
        { es: ExecutorService ->
            es.submit(Callable<A> { a()(es).get() })
        }
}
```

- 1. `unit` represented as a function that returns a `UnitFuture`
- 2. `map2` responsible only for combinatorial logic, no implicit threading
- 3. Timeouts are not respected due to the calls to `get()`
- 4. `fork` is not truly running in parallel due to blocking call `a()`

As stated before, this example is a naive solution to the problem. We will now identify the issues and address them one by one. The `unit` operator is represented as a function that returns a `UnitFuture`, which is a simple implementation of `Future` that simply wraps a constant value and never uses the `ExecutorService`. It is always executed and can’t be canceled. Its `get` method simply returns the value that we gave it.

Next up is the `map2` operator. It doesn’t evaluate the call to `f` in a separate logical thread, in accordance with our design choice of having `fork` be the sole function in the API that controls parallelism. We could always wrap `map2` with a call to `fork` if we wanted the evaluation of `f` to occur in a separate thread.

This implementation of `map2` also does not respect timeouts. It simply passes the ExecutorService on to both Par values, waits for the results of the Futures af and bf, applies f to them, and finally wraps them in a UnitFuture. In order to respect timeouts, we’d need a new Future implementation that records the amount of time spent evaluating af, and then subtracts that time from the available time allocated for evaluating bf.

The fork operator has the simplest and most natural implementation possible, but there are also some problems here—for one, the outer Callable will block waiting for the "inner" task to complete. Since this blocking occupies a thread or resource backing our ExecutorService, we are losing out on some potential parallelism. This is a symptom of a more serious problem with the implementation that we’ll discuss later in the chapter.

We should note that Future doesn’t have a purely functional interface. This is part of the reason why we don’t want users of our library to deal with Future directly. An important point to make is that even though methods on Future rely on side effects, our entire Par API remains pure. It’s only after the user calls run and the implementation receives an ExecutorService that we expose the Future's machinery. Our users are therefore programming to a pure interface with an implementation that relies on effects. But since our API remains pure, these effects aren’t side effects. In part 4 we’ll discuss this distinction in detail.

EXERCISE 7.3 (Hard)
Fix the implementation of map2 so that it respects the contract of timeouts on Future.

EXERCISE 7.4
This API already enables a rich set of operations. As an example, using lazyUnit write a function to convert any function (A) -> B to one that evaluates its result asynchronously.

```
fun <A, B> asyncF(f: (A) -> B): (A) -> Par<B> = TODO()
```

What else can we express with our existing combinators? Let’s look at a more concrete example.

Suppose we have a Par<List<Int>> representing a parallel computation that produces a List<Int>, and we’d like to convert this to a Par<List<Int>> with a sorted result:

```
fun sortPar(parList: Par<List<Int>>): Par<List<Int>> = TODO()
```

We could of course run the Par, sort the resulting list, and repackage it in a Par with unit. But we want to avoid calling run. The only other combinator we have that allows us to manipulate the value of a Par in any way is map2. So if we passed parList to one side of map2, we’d be able to gain access to the List inside and sort it. And we can pass whatever we want to the other side of map2, so let’s just pass a Unit:

```
fun sortPar(parList: Par<List<Int>>): Par<List<Int>> =
    map2(parList, unit(Unit)) { a, _ -> a.sorted() }
```

That was easy. We can now tell a Par<List<Int>> that we’d like that list sorted. But we might as well generalize this further. We can “lift” any function of type (A) -> B to become a function that takes Par<A> and returns Par<B>; we can map any function over a Par:

```
fun <A, B> map(pa: Par<A>, f: (A) -> B): Par<B> =
    map2(pa, unit(Unit), { a, _ -> f(a) })
```

As a result, sortPar now simply becomes:

```
fun sortPar(parList: Par<List<Int>>): Par<List<Int>> =
    map(parList) { it.sorted() }
```

That’s terse and clear. We just combined the operations to make the types line up. And yet, if you look at the implementations of map2 and unit, it should be clear this implementation of map means something sensible.

Was it cheating to pass the bogus value unit(Unit) as an argument to map2, only to ignore its value? Not at all! The fact that we can implement map in terms of map2, but not the other way around shows that map2 is strictly more powerful than map. This sort of thing happens a lot when we’re designing libraries—often, a function that seems to be primitive will turn out to be expressible using some more powerful primitive.

What else can we implement using our API? Could we map over a list in parallel? Unlike map2, which combines two parallel computations, parMap (let’s call it) needs to combine N parallel computations. It seems like this should somehow be expressible:

```
fun <A, B> parMap(
    ps: List<A>,
    f: (A) -> B
): Par<List<B>> = TODO()
```

We could always just write parMap as a new primitive. Remember that Par<A> is simply a type alias for (ExecutorService) -> Future<A>.

There’s nothing wrong with implementing operations as new primitives. In some cases we can even implement the operations more efficiently by assuming something about the underlying representation of the data types we’re working with. But right now we’re interested in exploring what operations are expressible using our existing API, and grasping the relationships between
