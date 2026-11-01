```
fun <A> listOfN(n: Int, a: Gen<A>): List<Gen<A>> = TODO()
```

This would certainly be a useful combinator, but not having to explicitly specify sizes is powerful as well. It means that whatever function runs the tests has the freedom to choose test case sizes, which opens up the possibility of doing the test case minimization we mentioned earlier. If the sizes are always fixed and specified by the programmer, the test runner won’t have this flexibility. Keep this concern in mind as we get further along in our design.

What about the rest of this example? The forAll function looks interesting. We can see that it accepts a Gen<List<Int>> and what looks to be a corresponding predicate, (List<Int>) -> Boolean. But again, it doesn’t seem like forAll should care about the types of the generator and the predicate, as long as they match up. We can express this with the type:

```
fun <A> forAll(a: Gen<A>, f: (A) -> Boolean): Prop = TODO()
```

Here, we’ve simply invented a new type, Prop (short for property) for the result of binding a Gen to a predicate. We might not know the internal representation of Prop or what other functions it supports, but based on our prior discussion in section 8.1, it should be combined with other Prop instances through the use of an and method. Let’s introduce that as a new interface:

```
interface Prop {
    fun and(p: Prop): Prop
}
```

# 8.2.2 Exploring the meaning and API of properties

Now that we have a few fragments of an API, let’s discuss what we want our types and functions to entail. First, let’s consider Prop. We know of functions forAll (for creating a property), and (for composing properties), and will now learn about check. Here we deviate further from KotlinTest’s property design as it doesn’t have such a method in its API. We’ll imagine this to be a method that runs our property and has a side effect of printing to the console. We will expose this as a convenience function on Prop, giving it a return type of Unit for now.

```
interface Prop {
    fun check(): Unit
    fun and(p: Prop): Prop
}
```

This return type does raise an issue in that we can’t chain together multiple checked Props using the and operator. This might remind you of a similar problem that we encountered in chapter 7 when we looked at using Thread and Runnable for parallelism.

Since check has a side effect, the only option for implementing and in this case would be to run check on both Prop instances. So if check prints out a test report we would get two of them,