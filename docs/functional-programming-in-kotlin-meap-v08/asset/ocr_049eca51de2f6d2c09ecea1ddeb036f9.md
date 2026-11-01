# EXERCISE 7.3

Fix the implementation of map2 so that it respects the contract of timeouts on Future.

```
fun <A, B, C> map2(a: Par<A>, b: Par<B>, f: (A, B) -> C): Par<C> =
  { es: ExecutorService ->
    val fa = a(es)
    val fb = b(es)
    TimedMap2Future(fa, fb, f)
  }

data class TimedMap2Future<A, B, C>(
  val pa: Future<A>,
  val pb: Future<B>,
  val f: (A, B) -> C
) : Future<C> {

  override fun isDone(): Boolean = TODO()

  override fun get(): C = TODO()

  override fun get(to: Long, tu: TimeUnit): C {
    val timeoutMillis = TimeUnit.MILLISECONDS.convert(to, tu)

    val start = System.currentTimeMillis()
    val a = pa.get(to, tu)
    val duration = System.currentTimeMillis() - start

    val remainder = timeoutMillis - duration
    val b = pb.get(remainder, TimeUnit.MILLISECONDS)
    return f(a, b)
  }

  override fun cancel(b: Boolean): Boolean = TODO()

  override fun isCancelled(): Boolean = TODO()
}
```

# EXERCISE 7.4

This API already enables a rich set of operations. As an example, using lazyUnit write a function to convert any function (A) -> B to one that evaluates its result asynchronously.

```
fun <A, B> asyncF(f: (A) -> B): (A) -> Par<B> =
  { a: A -> lazyUnit { f(a) } }
```

# EXERCISE 7.5

Write this function, called sequence. No additional primitives are required. Do not call run.

Two implementation are provided. The first is a more naive approach that uses simple recursion to achieve its goals.

```
val <T> List<T>.head: T
  get() = first()

val <T> List<T>.tail: List<T>
  get() = this.drop(1)

val Nil = listOf<Nothing>()
```