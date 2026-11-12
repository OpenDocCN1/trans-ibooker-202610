Let's call this modified version of factorial.

recur/fpij/BigFactorial.java

```
public static void main(final String[] args) {
    System.out.println(factorial(FIVE));
    System.out.println(String.format("%.10s...", factorial(TWENTYK)));
}
```

Now that we used BigInteger, the operation should go well.

120
1819206320...

We see the correct value of the factorial for the number 5 and the trimmed output value for the large input.

With only a few lines of code we turned an unoptimized recursion into a tail recursion and averted stack overflow, thanks to lambda expressions, functional interfaces, and infinite Streams.

With this technique on hand, we can boldly implement recursive solutions, with a minor redesign to turn them into tail calls.

The approach we used here made recursions feasible for large input. Next, we’ll see how to make them practical from a performance point of view.