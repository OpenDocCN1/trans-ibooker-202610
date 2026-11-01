We can now write some client code to try out this fancy new machinery.

```
val p: (ExecutorService) -> Future<List<Double>> =
    parMap((1..10).toList()) { sqrt(it.toDouble()) }

val x: List<Double> =
    run(Executors.newFixedThreadPool(2), p)

println(x)
```

Running this code will yield a result as follows:

```
[1.0, 1.4142135623730951, 1.7320508075688772, 2.0, 2.23606797749979,
2.449489742783178, 2.6457513110645907, 2.8284271247461903, 3.0,
3.1622776601683795...]
```

That will call `fork` about 100,000 times, starting that many actors to combine these values two at a time. Thanks to our non-blocking `Actor` implementation we don’t need 100,000 JVM threads to perform this processing, but managed to do it with a fixed thread pool size of 2!

And so we have proved that our law of forking now holds for fixed-size thread pools.

**EXERCISE 7.9 (Hard/Optional)**

Our non-blocking representation doesn’t currently handle errors at all. If at any point our computation throws an exception, the `run` implementation’s `latch` never counts down and the exception is simply swallowed. Can you fix that?

Taking a step back, the purpose of this section hasn’t necessarily been to figure out the best non-blocking implementation of `fork`, but more to show that laws are important. They give us another angle to consider when thinking about the design of a library. If we hadn’t tried writing out some of the laws of our API, we may not have discovered the thread resource leak in our first implementation until much later.

In general, there are multiple approaches you can consider when choosing laws for your API. You can think about your conceptual model, and reason from there to postulate laws that should hold true. You can also just *invent* laws you think might be useful or instructive (like we did with our `fork` law), and see if it’s possible and even sensible to ensure that they hold for your model. And lastly, you can look at your *implementation* and come up with laws you expect to hold based on that.³⁷