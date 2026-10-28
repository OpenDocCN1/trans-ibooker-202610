The four words shown in the following diagram characterize a lambda expression:

![](bbox=[0.15, 0.15, 0.85, 0.38])

# Figure 17.2 – Lambda characteristics

As a rule of thumb, keep in mind that lambdas sustain the Behavior Parameterization design pattern (a behavior is passed as parameter of a function) and it can be used only in the context of a functional interface.

# Coding challenge 2 – Functional interface

Problem: What is a functional interface?

Solution: In Java, a functional interface is an interface that contains only one abstract method. In other words, a functional interface contains only one method that is not implemented. So, a functional interface wraps a function as an interface and the function is represented by a single abstract method on the interface.

Optionally, besides this abstract method, a functional interface can have default and/or static methods as well. Commonly, a functional interface is annotated with @FunctionalInterface. This is just an informative annotation type that's used to mark a functional interface.

Here is an example of a functional interface:

```
@FunctionalInterface
public interface Callable<V> {
	V call() throws Exception;
}
```

As a rule of thumb, if an interface has more methods without implementation (that is, abstract methods), then it is no longer a functional interface. This means that such an interface cannot be implemented by a Java lambda expression.