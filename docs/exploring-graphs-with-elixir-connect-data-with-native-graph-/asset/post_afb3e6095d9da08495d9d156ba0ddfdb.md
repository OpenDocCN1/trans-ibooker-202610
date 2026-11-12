
can model the behavior or state transformations, business workflows, and data processing as a series of functions to form a function composition.

## Joe asks: Does Concise Just Mean Less Code?

Concise is short, devoid of noise, and boiled down to its essence to convey the intent effectively. The benefits are far-reaching.

Writing code is like throwing ingredients together; making it concise is like turning that into a sauce. It often takes more effort to write concise code. It’s less code to read, but effective code is transparent. A short code listing that’s hard to understand or hides details is terse rather than concise.

Concise code aids design agility and has less ceremony. This means we can quickly try out our design ideas and move forward if they’re good, or move on if they turn sour.

## Why Code in the Functional Style?

We saw the general benefits of the functional style of programming, but is it worth picking up this new style? Should we expect a marginal improvement, or is it life-altering? Those are genuine questions that we need answered before we commit our time and effort.

Writing Java code isn’t that hard; the syntax is simple. We’ve gotten quite familiar and comfortable with the libraries and their APIs. What really gets us is the effort required to code and maintain the typical enterprise applications that we develop with Java.

We must ensure that fellow programmers have closed the database connections at the right time, that they’re not holding on to transactions any longer than needed, that they’re handling the exceptions well and at the right level, that they’re securing and releasing locks properly...and the list goes on.

Each one of these in isolation may not seem like a big deal. But when combined with the domain’s inherent complexities, things get overwhelming, labor-intensive, and hard to maintain.

What if we could encapsulate each of these decisions into tiny pieces of code that can manage the constraints well? Then we wouldn’t have to continuously expend time, effort, and energy to enforce policies. That would be a big win—a less cognitive load—so let’s see how the functional style can help.

## Iteration on Steroids

We write iterations all the time to process a list of objects and to work with sets and maps. The iterators we're used to in Java are familiar and primitive, but not simple. Not only do they take a few lines of code to work with but they're also hard to compose.

How do we iterate and print each element in a collection? We could use a for loop. How do we select some elements from a collection? With the same for loop, but some extra mutable variables have to step in to support the operation. Now after selecting the values, how do we reduce the results to a single value, such as a minimum, a maximum, or an average? More looping, more mutable variables.

That's like having a jack-of-all-iterations, but a master of none. Java now provides specialized internal iterators for various operations: one to simply loop, one to map data values, one to filter out select values, one to reduce, and several convenience functions to pick the min, max, average, and so on. In addition, these operations are easy to compose so we can combine a variety of them to implement the business logic with greater ease and less code. When we're done, the code is easier to understand as it logically flows through the sequence described in the problem. We'll see several examples of this in Chapter 2, Using Collections, and throughout the book.

## Enforcing Policies

Policies rule enterprise applications. For instance, we may have to ensure an operation has proper security credentials. We may have to ensure that transactions run fast and update audit trails properly. These tasks often turn into mundane service-tier code like the following pseudocode form:

```
Transaction transaction = getFromTransactionFactory();
//... operation to run within the transaction ...
checkProgressAndCommitOrRollbackTransaction();
UpdateAuditTrail();
```

There are two issues with this kind of approach. First, it often leads to duplication of effort and, in turn, increases maintenance costs. Second, it's easy to forget about exceptions that may be thrown in the application code, thus jeopardizing the transaction lifetime and the update of audit trails. We could implement a proper try and finally block, but every time someone touches that code, we'd have to reverify that it's not broken.

Alternatively, we could get rid of the factory and turn this code on its head. Instead of receiving a transaction, we could send the processing code to a well-managed function, like so (in pseudocode):

```
runWithinTransaction((Transaction transaction) -> {
//... operation to run within the transaction ...
});
```

This is such a small step with huge savings. The policy to check the status and update the audit trails is abstracted and encapsulated within the runWithinTransaction method. To this method we send a piece of code that needs to run in the context of a transaction. We no longer have to worry about forgetting to perform the steps or about the exceptions being handled well. The policy-enforcing function takes care of all that.

We'll cover how to use lambda expressions to enforce such policies in Chapter 6, *Working with Resources*.

## Extending Policies

Policies seem to grow around us—beyond their being enforced, enterprise applications require ways to extend them. Based on some configuration information, we may have to add or remove a series of operations that, in turn, may have to be processed before core logic in a module is executed. This is a common task in Java, but it requires much forethought and design.

The machinery for extensibility is often one or more interfaces. We could carefully design these interfaces and the hierarchy of classes that will implement them. The result may be effective, but this effort possibly leaves a number of interfaces and classes that we have to maintain. The design can easily become heavyweight and hard to maintain, jeopardizing the very goal of extensibility we set out for.

There's an alternative—functional interfaces and lambda expressions, which let us design extensible policies. This way we're not forced to create extra interfaces or conform to a method name, but instead, we can focus on the core behaviors we'd like to provide, as we'll see in *Decorating Using Lambda Expressions*.

## Hassle-Free Concurrency

A big application is close to its delivery milestone when a huge performance issue comes to the surface. The team quickly figures out that the bottleneck is in the titanic module of the application, which involves processing large volumes of data. Someone on the team suggests that we can improve performance if we more effectively exploit the available multiple cores. But the excitement from the suggestion is likely short-lived if the titanic module is like typical old-style Java code.

The team quickly realizes that converting the titanic module’s code from a sequential to a concurrent version would take substantial effort, create additional complexity, and open doors for many multithreading-related bugs. Isn’t there an easier way to get better performance?

What if there is no difference between sequential and concurrent code, and the effort to run it sequentially versus concurrently is merely the flip of a switch to clearly express our intent?

That may seem possible only in Narnia, but it’s quite real if we develop our modules with functional purity. The internal iterators and functional style remove the last roadblock to easy parallelism. The JDK library has been designed to make the switch between serial and parallel execution require only a small and explicit but unobtrusive code change, as we’ll see in Taking a Leap to Parallelize.

## Telling the Story

So much is lost in the translation between what the business wants and how the code implements it. The bigger that gap, the greater the chance of errors and the higher the cost of maintenance. If the code reads more like the way the business states the problem, it becomes easier to read,
