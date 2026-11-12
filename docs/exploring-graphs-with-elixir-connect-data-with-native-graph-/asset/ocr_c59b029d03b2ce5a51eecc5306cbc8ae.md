TailCall objects, we can easily use lazy iteration over a Stream of pending TailCall instances. The technique we used in Creating Infinite, Lazy Collections, will help us here to lazily produce the next pending TailCall instance. Let’s take a closer look at how.

To create a lazy list of pending TailCall instances, we use the Stream interface’s iterate static method. This method takes an initial seed value and a generator. We use the current TailCall instance, this, as the seed. The generator, a UnaryOperator, takes in the current element and produces the next element. For the generator to return the next pending TailCall instance, it can use the apply method of the current TailCall. We use the method reference TailCall::apply for this purpose to create the generator.

In short, we’ve designed the invoke method so that the iteration will start at the seed, the first instance of TailCall, and iterate through subsequent instances of TailCall produced by the generator until it finds an instance of TailCall that signals the termination of recursion.

# Creating the TailCalls Convenience Class

The iteration continues until the isComplete method reports a completion. But the default implementation of this method in the TailCall interface always returns a false value. This is where the companion TailCalls class comes in. It provides two different implementations of the TailCall functional interface: one in the call method and the other in the done method.

![](bbox=[0.14, 0.81, 0.88, 0.84])

```
public class TailCalls {
public static <T> TailCall<T> call(final TailCall<T> nextCall) {
return nextCall;
```