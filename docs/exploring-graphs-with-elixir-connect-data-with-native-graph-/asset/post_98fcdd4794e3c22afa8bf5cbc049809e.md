
easier to discuss with the business folks, and easier to evolve to meet their changing demands.

For instance, you hear the business say, “Get the prices for all the tickers, find the prices that are less than $500, and total the net asset value of only the stocks that make the cut.” Using the new facilities available, we can write something like this:

```
tickers.stream()
.map(StockUtil::getprice)
.filter(StockUtil::priceIsLessThan500)
.sum()
```

There’s little chance of losing something in translation here, as there’s not much to translate. This is function composition at work, and we’ll see more of it in this book, especially in Chapter 9, Composing Functions with Lambda Expressions.

## Separation of Concerns

A common need in applications is the separation of the core computations from the fine-grained logic the computations depend on. For example, an order-processing system may want to apply different tax computations based on the origin of the transaction. Separating the tax-computation logic from the rest of the processing will help us create more reusable and extensible code.

In OOP we call this separation of concern and often use the strategy pattern to solve it. The effort typically involves creating one or more interfaces and a bunch of classes to implement them.

We can achieve the same now but with far less code. And we can try out our design ideas really fast without being bogged down by a hierarchy of code that we have to lay out first. We'll cover how to create this pattern and separate concerns using lightweight functions in Separating Concerns Using Lambda Expressions.

## Delaying Evaluation

When creating enterprise applications, we may have to interact with web services, make database calls, process XML...the list goes on. There are so many operations that we have to perform, but not all of them are necessary all the time. Avoiding some operations or at least postponing the ones that don't have to be performed yet is one of the easiest ways to improve performance and application start-up or response time.

It's a simple goal, but one that may be hard to implement using a pure OOP approach. We would have to fuss with object references and null checks to postpone the initialization of heavyweight objects, for instance.

Alternatively, we can minimize our effort and make the intent more explicit by using the new Optional class and the functional-style API it provides, as we'll see in Delayed Initialization.

## Improving Testability

Fewer things tend to break in code that has few moving parts. By nature, functional-style code is more resilient to change and requires relatively less testing effort.

In addition, as we'll see in Chapter 5, Designing with Lambda Expressions, and Chapter 6, Working with Resources, lambda expressions can stand in as lightweight mocks or stubs and can help create highly expressive exception tests. Lambda expressions can also serve as a great testing aid. A common set of test cases can receive and exercise lambda expressions. The tests can capture the essence of behaviors that need to be tested for regression. At the same time, the lambda expressions being passed in can serve as variations of implementations that need to be exercised.

The automated tests that are part of the JDK itself are great examples of this. These tests show how lambda expressions help parameterize the test cases' key behaviors; for example, they help compose the tests as “make a container for the results” followed by “assert some parameterized postconditions.”

We've discussed how the functional style not only helps us write better quality code but also solves elegantly so many of our common application development challenges. That means we can create applications more quickly, with less effort and fewer errors—as long as we follow a few guidelines, as we will discuss next.

## Evolution, Not Revolution

To reap the benefits of functional style, we don't have to switch over to another language; we simply have to change the way we use Java.

Languages like C++, Java, and C# started out with support for imperative and object-oriented programming. Now all these languages also embrace the functional style of programming. We've seen examples of these two styles and discussed the benefits we derived from the functional style. Now let's look into some key concepts and practices that will help us adopt the new style.

The Java language team has put in substantial time and effort to bring functional capabilities to the language and the JDK. To reap the benefits, we have to pick up a few new concepts. We can improve our code if we follow some guidelines:

- Be declarative.
- Promote immutability.
- Avoid side effects.
- Prefer expressions over statements.
- Design with higher-order functions.

Let's quickly look at these practices.

## Be Declarative

At the core of the familiar imperative style are mutability and command-driven programming. We create variables or objects and modify their state along the way. We also provide detailed commands or instructions to execute, such as create a loop index, increment its value, check if we reached the end, update the nth element of an array, and so on. It made sense for us to program this way in the past due to the nature of the tools and the hardware limitations.

We saw how the declarative use of the contains method—when used on a collection—was far easier to work with than the imperative style. All the hard work and lower-level details were moved into the library function, and we didn't have to deal with them. We would prefer doing everything this way if it were only easier. Immutability and declarative programming are the essence of the functional style of programming, and Java now makes them quite approachable.

## Promote Immutability

Mutable code has many moving parts. The more things change, the easier it is for components to break and for errors to creep in. Code where multiple variables change is hard to understand and difficult to parallelize. Immutability removes all these problems at the root.

Java supports immutability but doesn't enforce it—but we can. We need to change our old habits of mutating objects’ states. As much as possible, we must use immutable objects.

When declaring variables, fields, and parameters, lean toward declaring them final, following the sage advice “Treat objects as immutable” from Effective Java [Blo18], by Joshua Bloch.

When creating objects, promote immutable objects such as the String class. When working with collections, create immutable or unmodifiable collections using functions like List.of() or the Collections class’s unmodifiableList method, for example.

By avoiding mutability we can create pure functions—that is, functions with no side effects.

## Avoid Side Effects

Imagine writing a piece of code to go out to the Web to fetch a stock price and then update a shared variable. If we have a number of prices to fetch, we’re forced to run these time-consuming operations sequentially. If we resort to multithreading, then we have to burden ourselves with threading and synchronization issues to prevent race conditions. The net result is poor application performance and/or lack of sleep trying to manage multiple threads. We can totally eliminate the problems by removing the side effect.

A function with no side effects honors immutability and doesn’t change its input or anything in its reach. These functions are easier to understand, have fewer errors, and are easier to optimize. The lack of side effects removes any concerns of race conditions or simultaneous updates. As a result, we can also easily parallelize the execution of such functions, as we’ll see in Taking a Leap to Parallelize.

## Prefer Expressions over Statements

Statements are stubborn and force mutation. Expressions promote immutability and function composition. For example, we first used the for statement to compute the total of the discounted prices. This version promoted mutation and verbose code. By switching over to the more expressive declarative version using the map and sum methods, which are expressions, we avoided mutations and were able to chain or compose functions.

It's better to design with expressions and use them more than statements in our code. This will now make the code concise and easier to understand. The code will flow logically, in the same order in which we would state the problem. The concise version is easier to change if the problem changes.

## Design with Higher-Order Functions

Unlike some functional programming languages, such as Haskell, that enforce immutability, Java lets us modify variables at will. In that regard, Java is not, and will never be, a pure functional programming language. But we can write code in the functional style in Java by using higher-order functions.

A higher-order function takes the concept of reuse to the next level. Instead of solely relying on objects and classes to promote reuse, with higher-order functions, we can easily reuse small, focused, cohesive, and well-written functions.

In OOP we’re used to passing objects to methods, creating objects within methods, and returning objects from within methods. Higher-order functions do to functions what methods do to objects. With higher-order functions, we can do the following:
