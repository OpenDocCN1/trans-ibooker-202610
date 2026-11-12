
- Pass functions to functions
- Create functions within functions
- Return functions from functions

We've already seen an example of passing a function to another function, and we'll see examples of creating and returning functions later. Let's look at our “passing a function to a function” example again.

```
prices.stream()
	.filter(price -> price > 20)
	.mapToDouble(price -> price * 0.9)
	.sum();
```

In this example, we're sending a function, price -> price * 0.9, as an argument to mapToDouble. The function being passed is created just in time, at the point of call to the higher-order function mapToDouble. Generally, a function has a body, a name, a parameter list, and a return type. The just-in-time function created here has a parameter list followed by an arrow (->), and then the short body. The type of the parameter may be inferred by the Java compiler here and the return type is implicit. This function is anonymous; it has no name. Rather than referring to these as anonymous functions, we call them lambda expressions.

Passing anonymous functions isn’t a totally unknown concept in Java; we’re used to passing instances of anonymous classes. If our anonymous class had only one method, we still had to go through the ceremony of creating a class, albeit anonymous, and instantiating it. Instead, we can now enjoy a lightweight syntax in Java with lambda expressions. Additionally, we’re accustomed to abstracting concepts with objects. Now we can combine that with abstracting behavior using lambda expressions.

It takes some rethinking to design applications with this style of programming. We have to tune our imperative-ingrained minds to think functionally. This may seem a bit difficult at the beginning, but we'll get used to it in no time and can leave those dysfunctional APIs far behind as we move forward.

Let's now switch gears and look at how Java handles lambda expressions. We're used to passing objects to methods, but now we can store functions and pass them around. Let's look at the magic behind how Java accepts a function as an argument.

## A Little Sugar to Sweeten

We could implement all the ideas with what was already available in Java, but lambda expressions remove the ceremony and sweeten our efforts by adding a little syntax sugar. This quickly translates into code that’s faster to create and makes it easier to express our ideas.

In the past, we’ve used a number of interfaces that only have single methods: Runnable, Callable, Comparable, and so on. These interfaces are common in the JDK library and often appear where just a single function is expected. All these existing library methods that expect a single-method interface can now accept lightweight functions, thanks to the brilliant syntax sugar provided through functional interfaces.

A functional interface is an interface with one abstract—unimplemented—method. Again, think of single-method interfaces like Runnable, Callable, Comparable, and so on, which all fit that definition. The modern JDK has more of these types of interfaces—Function, Predicate, Consumer, Supplier, and so on (for a summary of the starter set of functional interfaces see Appendix 1, Starter Set of Functional Interfaces). A functional interface may also have zero or more static methods and default methods, which are implemented right within the interface.

We can mark a functional interface with the @FunctionalInterface annotation. The compiler doesn’t require this annotation, but it’s helpful to explicitly state the purpose that the interface serves. Besides, if we mark an interface with this annotation, the compiler will enforce the rules for the interface to qualify as a functional interface.

If a method takes a functional interface as a parameter, then we can pass any of the following:

- An anonymous inner class, the old-fashioned way (but why would we?)
- A lambda expression, as we did when we called the map method
- A method or constructor reference (as we’ll see later)

The compiler readily accepts a lambda expression or a method/constructor reference as an argument if the method’s corresponding parameter is a reference to a functional interface.

When we pass a lambda expression to a method, the compiler will convert the lambda expression to an instance of the appropriate functional interface. This conversion isn’t a mere generation of an inner class in place. The synthesized method of this instance conforms to the abstract method of the functional interface that corresponds to the argument. For example, the map method takes the functional interface Function<T, R> as its parameter. In a call to the map method, the Java compiler synthesizes it, as the following figure shows.

```
Call:
prices.stream().map(price -> price * 0.9);
```

```
//synthesized by the Java Compiler
...
... Double apply(Integer param) {
int price = Integer.intValue(param);
return Double.valueOf(price * 0.9d);
}
```

```
In Stream...
Stream<R> map(Function<T, R> mapper) {
...
... = mapper.apply(...);
...
}
```

The parameters of the lambda expression must match the parameters of the interface's abstract method. This synthesized method returns the lambda expression's result. If the return type doesn't directly match that of the abstract method, the synthesized method may convert the return value to a proper assignable type.

We took a peek at how lambda expressions are passed as arguments to methods. Let's quickly review what we covered and move on to explore lambda expressions.

## Wrapping Up

It's a whole new world in modern Java. We can now program in an elegant and fluent functional style, with higher-order functions. This can lead to concise code that has fewer errors and is easier to understand, maintain, and parallelize. The Java compiler works its magic so we can send lambda expressions or method references where functional interfaces are expected.

We're all set to dive into the fun parts of lambda expressions and the JDK library that's been fine-tuned to work with lambda expressions. In the next chapter we'll start by using lambda expressions in one of the most fundamental programming tasks: working with collections.

## Using Collections

We often use collections of numbers, strings, and objects. They're so commonplace that removing even a small amount of ceremony from coding collections can reduce code clutter greatly. In this chapter we explore the use of lambda expressions to manipulate collections. We use them to iterate collections, transform them into new collections, extract elements from them, and easily concatenate their elements.

After this chapter, our Java code to manipulate collections will never be the same—it'll be concise, expressive, elegant, and more extensible than ever before.

### Iterating through a List

Iterating through a list is a basic operation on a collection, but over the years it’s gone through a few significant changes. We’ll begin with the old style and evolve an example—enumerating a list of names—to the elegant style.

We can easily create a list of names with the following code:

```
final List<String> friends =
Arrays.asList("Brian", "Nate", "Neal", "Raju", "Sara", "Scott");
```

To create an immutable list, we may use the more recently introduced function List.of() instead of Arrays.asList().

Here's the habitual, but not so desirable, way to iterate and print each of the elements.

```
collections/fpij/Iteration.java
for(int i = 0; i < friends.size(); i++) {
    System.out.println(friends.get(i));
}
```

I call this style *the self-inflicted wound pattern*—it's verbose and error-prone. We have to stop and wonder, “is it i < or i <=?”
This is useful only if we need to manipulate elements at a
particular index in the collection, but even then, we can opt to
use a functional style that favors immutability, as we will
discuss soon.

Java also offers a construct that’s a little bit more civilized
than the good old for loop.

```
collections/fpij/Iteration.java
for(String name : friends) {
```
