# TESTING PAR

Getting back to proving the property that map(unit(1)) { it + 1 } is equal to unit(2), we can use our new check primitive to express this in a way that doesn’t obscure the intent:

```
val p = check {
  val p1 = map(unit(1)) { it + 1 }
  val p2 = unit(2)
  p1(es).get() == p2(es).get()
}
```

This is now pretty clear. But can we do something about the noise of p1(es).get() and p2(es).get()? This needless repetition obscures the intent of our test and has very little to do with what we are attempting to prove. We’re forcing this code to be aware of the internals of Par so that we can compare two Par values to each other for equality. One improvement is to lift the equality comparison into Par using map2, which means we only have to run a single Par at the end to get our result:

```
fun <A> equal(p1: Par<A>, p2: Par<A>): Par<Boolean> =
  map2(p1, p2, { a, b -> a == b })

val p = check {
  val p1 = map(unit(1)) { it + 1 }
  val p2 = unit(2)
  equal(p1, p2)(es).get()
}
```

This is already a bit better than having to run each side separately. But while we’re at it, why don’t we move the running of Par out into a separate function called forAllPar. This also gives us a good place to insert variation across different parallel strategies, without it cluttering the property we’re specifying:

```
val ges: Gen<ExecutorService> = weighted(
  Gen.choose(1, 4).map {
    Executors.newFixedThreadPool(it)
  } to .75,
  Gen.unit(
    Executors.newCachedThreadPool()
  ) to .25
)

fun <A> forAllPar(ga: Gen<A>, f: (A) -> Par<Boolean>): Prop =
  forAll(
    map2(ges, ga) { es, a -> es to a }
  ) { (es, a) -> f(a)(es).get() }
```

+   ① A weighted generator of executor services
+   ② Create a fixed thread pool 75% of the time
+   ③ Create an unbounded thread pool 25% of the time
+   ④ Create a Pair<Gen<ExecutorService>, Gen<A>> using the to keyword

The value ges is a Gen<ExecutorService> that will vary over fixed-size thread pools from 1–4 threads, as well as consider an unbounded thread pool.