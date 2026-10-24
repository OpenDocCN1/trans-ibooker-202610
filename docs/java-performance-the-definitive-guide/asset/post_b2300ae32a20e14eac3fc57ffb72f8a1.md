
Similarly, every time a loop completes an execution, the branching counter is incremented and inspected. If the branching counter has exceeded its individual threshold, the loop (and not the entire method) becomes eligible for compilation.

Tunings affect these thresholds. When tiered compilation is disabled, standard compilation is triggered by the value of the `-XX:CompileThreshold=N` flag. The default value of N is 10,000. Changing the value of the CompileThreshold flag will cause the compiler to choose to compile the code sooner (or later) than it normally would have. Note, however, that although there is one flag here, the threshold is calculated by adding the sum of the back-edge loop counter plus the method entry counter.

You can often find recommendations to change the CompileThreshold flag, and several publications of Java benchmarks use this flag (e.g., frequently after 8,000 iterations). Some applications still ship with that flag set by default.

But remember that I said this flag works when tiered compilation is disabled—which means that when tiered compilation is enabled (as it normally is), this flag does nothing at all. Use of this flag is really just a holdover from JDK 7 and earlier days.

This flag used to be recommended for two reasons: first, lowering it would improve startup time for an application using the C2 compiler, since code would get compiled more quickly (and usually with the same effectiveness). Second, it could cause some methods to get compiled that otherwise never would have been compiled.

That last point is an interesting quirk: if a program runs forever, wouldn’t we expect all of its code to get compiled eventually? That’s not how it works, because the counters the compilers use increase as methods and loops are executed, but they also decrease over time. Periodically (specifically, when the JVM reaches a safepoint), the value of each counter is reduced.

Practically speaking, this means that the counters are a relative measure of the *recent* hotness of the method or loop. One side effect is that somewhat frequently executed code may never be compiled by the C2 compiler, even for programs that run forever. These methods are sometimes called *lukewarm* (as opposed to hot). Before tiered compilation, this was one case where reducing the compilation threshold was beneficial.

Today, however, even the lukewarm methods will be compiled, though perhaps they could be ever-so-slightly improved if we could get them compiled by the C2 compiler rather than the C1 compiler. Little practical benefit exists, but if you’re really interested, try changing the flags `-XX:Tier3InvocationThreshold=N` (default 200) to get C1 to compile a method more quickly, and `-XX:Tier4InvocationThreshold=N` (default 5000) to get C2 to compile a method more quickly. Similar flags are available for the back-edge threshold.

![](img/1711f142ae3cfd740ef106afbbba2046_122_0.png)

## Quick Summary

- The thresholds at which methods (or loops) get compiled are set via tunable parameters.
- Without tiered compilation, it sometimes made sense to adjust those thresholds, but with tiered compilation, this tuning is no longer recommended.

## Compilation Threads

“Compilation Thresholds” on page 105 mentioned that when a method (or loop) becomes eligible for compilation, it is queued for compilation. That queue is processed by one or more background threads.

These queues are not strictly first in, first out; methods whose invocation counters are higher have priority. So even when a program starts execution and has lots of code to compile, this priority ordering helps ensure that the most important code will be compiled first. (This is another reason the compilation ID in the PrintCompilation output can appear out of order.)

The C1 and C2 compilers have different queues, each of which is processed by (potentially multiple) different threads. The number of threads is based on a complex formula of logarithms, but Table 4-2 lists the details.

Table 4-2. Default number of C1 and C2 compiler threads for tiered compilation

| CPUs | C1 threads | C2 threads |
|---|---|---|
| 1 | 1 | 1 |
| 2 | 1 | 1 |
| 4 | 1 | 2 |
| 8 | 1 | 2 |
| 16 | 2 | 6 |
| 32 | 3 | 7 |
| 64 | 4 | 8 |
| 128 | 4 | 10 |

The number of compiler threads can be adjusted by setting the -XX:CICompilerCount=N flag. That is the total number of threads the JVM will use to process the queue(s); for tiered compilation, one-third (but at least one) will be used to process the C1 compiler queue, and the remaining threads (but also at least one) will be used to process the C2 compiler queue. The default value of that flag is the sum of the two columns in the preceding table.

![](img/1711f142ae3cfd740ef106afbbba2046_124_0.png)

## Quick Summary

- Compilation occurs asynchronously for methods that are placed on the compilation queue.
- The queue is not strictly ordered; hot methods are compiled before other methods in the queue. This is another reason compilation IDs can appear out of order in the compilation log.

## Inlining

One of the most important optimizations the compiler makes is to inline methods. Code that follows good object-oriented design often contains attributes that are accessed via getters (and perhaps setters):

```
public class Point {
    private int x, y;

    public void getX() { return x; }
    public void setX(int i) { x = i; }
}
```

The overhead for invoking a method call like this is quite high, especially relative to the amount of code in the method. In fact, in the early days of Java, performance tips often argued against this sort of encapsulation precisely because of the performance impact of all those method calls. Fortunately, JVMs now routinely perform code inlining for these kinds of methods. Hence, you can write this code:

```
Point p = getPoint();
p.setX(p.getX() * 2);
```

The compiled code will essentially execute this:

```
Point p = getPoint();
p.x = p.x * 2;
```

Inlining is enabled by default. It can be disabled using the -XX:-Inline flag, though it is such an important performance boost that you would never actually do that (for example, disabling inlining reduces the performance of the stock batching test by over 50%). Still, because inlining is so important, and perhaps because we have many other knobs to turn, recommendations are often made regarding tuning the inlining behavior of the JVM.

Unfortunately, there is no basic visibility into how the JVM inlines code. If you compile the JVM from source, you can produce a debug version that includes the flag -XX:+PrintInlining. That flag provides all sorts of information about the inlining decisions that the compiler makes.) The best that can be done is to look at profiles of

the code, and if any simple methods near the top of the profiles seem like they should be inlined, experiment with inlining flags.

The basic decision about whether to inline a method depends on how hot it is and its size. The JVM determines if a method is hot (i.e., called frequently) based on an internal calculation; it is not directly subject to any tunable parameters. If a method is eligible for inlining because it is called frequently, it will be inlined only if its bytecode size is less than 325 bytes (or whatever is specified as the -XX:MaxFreqInlineSize=N flag). Otherwise, it is eligible for inlining only if it is smaller than 35 bytes (or whatever is specified as the -XX:MaxInlineSize=N flag).

Sometimes you will see recommendations that the value of the MaxInlineSize flag be increased so that more methods are inlined. One often overlooked aspect of this relationship is that setting the MaxInlineSize value higher than 35 means that a method might be inlined when it is first called. However, if the method is called frequently—in which case its performance matters much more—then it would have been inlined eventually (assuming its size is less than 325 bytes). Otherwise, the net effect of tuning the MaxInlineSize flag is that it might reduce the warm-up time needed for a test, but it is unlikely that it will have a big impact on a long-running application.

## Quick Summary

![](img/1711f142ae3cfd740ef106afbbba2046_125_0.png)

- Inlining is the most beneficial optimization the compiler can make, particularly for object-oriented code where attributes are well encapsulated.
- Tuning the inlining flags is rarely needed, and recommendations to do so often fail to account for the relationship between normal inlining and frequent inlining. Make sure to account for both cases when investigating the effects of inlining.

## Escape Analysis

The C2 compiler performs aggressive optimizations if escape analysis is enabled (-XX:+DoEscapeAnalysis, which is true by default). For example, consider this class to work with factorials:

```
public class Factorial {
    private BigInteger factorial;
    private int n;
    public Factorial(int n) {
        this.n = n;
    }
    public synchronized BigInteger getFactorial() {
        if (factorial == null)
            factorial = ...;
```
