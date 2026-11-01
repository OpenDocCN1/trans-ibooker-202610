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

# 7.3 Refining the API with the end user in mind

The way we’ve worked so far is a bit artificial. In practice, there aren’t such clear cut boundaries between designing the API and choosing a representation with one preceding the other. Ideas for a representation can drive the API design, but the opposite may also happen—the choice of API can drive the representation. It is natural to shift fluidly between these two perspectives, run experiments when questions arise, build prototypes, and so on.

We’ll devote this section to exploring and refining our API. Though we’ve already obtained a lot of mileage out of evolving this simple example, let’s try to learn more about what we can express using the primitive operations that we already built before adding any new ones. With our primitives and their chosen meanings, we’ve carved out a small universe for ourselves. We now get to discover what ideas can be expressed in this universe. This can, and should be a fluid process—we can change the rules of our universe at any time, make a fundamental change to our representation or introduce a new primitive, all while observing how our creation subsequently behaves.

We will begin by implementing the functions of the API that we’ve developed up to this point. Now that we have a representation for Par, a first attempt should be straightforward. What follows is a naive implementation using this initial representation.