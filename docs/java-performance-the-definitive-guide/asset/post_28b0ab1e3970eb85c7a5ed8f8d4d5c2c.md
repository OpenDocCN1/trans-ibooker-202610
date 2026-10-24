
than the same application written in Java (though often those examples contain poorly written Java code). However, that debate misses the point of this section: when an application is already written in Java, calling native code for performance reasons is almost always a bad idea.

Still, at times JNI is quite useful. The Java platform provides many common features of operating systems, but if access to a special, operating-system-specific function is required, so is JNI. And why build your own library to perform an operation, when a commercial (native) version of the code is readily available? In these and other cases, the question becomes how to write the most efficient JNI code.

The answer is to avoid making calls from Java to C as much as possible. Crossing the JNI boundary (the term for making the cross-language call) is expensive. Because calling an existing C library requires writing glue code in the first place, take the time to create new, coarse-grained interfaces via that glue code: perform many, multiple calls into the C library in one shot.

Interestingly, the reverse is not necessarily true: C code that calls back into Java does not suffer a large performance penalty (depending on the parameters involved). For example, consider the following code excerpt:

```java
@Benchmark
public void testJavaJavaJava(Blackhole bh) {
    long l = 0;
    for (int i = 0; i < nTrials; i++) {
        long a = calcJavaJava(nValues);
        l += a / nTrials;
    }
    bh.consume(l);
}

private long calcJavaJava(int nValues) {
    long l = 0;
    for (int i = 0; i < nValues; i++) {
        l += calcJava(i);
    }
    long a = l / nValues;
    return a;
}

private long calcJava(int i) {
    return i * 3 + 15;
}
```

This (completely nonsensical) code has two main loops: one inside the benchmark method and then one inside the calcJavaJava() method. That is all Java code, but we can choose instead to use a native interface and write the outer calculation method in C:

```java
@Benchmark
public void testJavaCC(Blackhole bh) {
    long l = 0;
    for (int i = 0; i < nTrials; i++) {
        long a = calcCC(nValues);
        l += 50 - a;
    }
    bh.consume(l);
}

private native long calcCC(int nValues);
```

Or we could just implement the inner call in C (the code for which should be obvious).

Table 12-8 shows the performance from various permutations, given 10,000 trials and 10,000 values.

**Table 12-8. Time to calculate across the JNI boundary**

| Calculate | Calc | Random | JNI transitions | Total time |
|---|---|---|---|---|
| Java | Java | Java | 0 | 0.104 ± 0.01 seconds |
| Java | Java | C | 10,000,000 | 1.96 ± 0.1 seconds |
| Java | C | C | 10,000 | 0.132 ± 0.01 seconds |
| C | C | C | 0 | 0.139 ± 0.01 seconds |

Implementing only the innermost method in C provides the most crossings of the JNI boundary (numberOfTrials × numberOfLoops, or 10 million). Reducing the number of crossings to numberOfTrials (10,000) reduces that overhead substantially, and reducing it further to 0 provides the best performance.

JNI code performs worse if the parameters involved are not simple primitives. Two aspects are involved in this overhead. First, for simple references, an address translation is needed. Second, operations on array-based data are subject to special handling in native code. This includes String objects, since the string data is essentially a character array. To access the individual elements of these arrays, a special call must be made to pin the object in memory (and for String objects, to convert from Java's UTF-16 encoding into UTF-8 in JDK 8). When the array is no longer needed, it must be explicitly released in the JNI code.

While the array is pinned, the garbage collector cannot run—so one of the most expensive mistakes in JNI code is to pin a string or array in code that is long-running. That prevents the garbage collector from running, effectively blocking all the application threads until the JNI code completes. It is extremely important to make the critical section where the array is pinned as short as possible.

Often, you will see the term “GC Locker Initiated GC” in your GC logs. That’s an indication that the garbage collector needed to run but it couldn’t, because a thread had pinned data in a JNI call. As soon as that data is unpinned, the garbage collector will run. If you see this GC cause frequently, look into making the JNI code faster; your other application threads are experiencing delays waiting for GC to run.

Sometimes, the goal of pinning objects for a short period of time conflicts with the goal of reducing the calls that cross the JNI boundary. In that case, the latter goal is more important even if it means making multiple crossings of the JNI boundary, so make the sections that pin arrays and strings as short as possible.

![](img/1711f142ae3cfd740ef106afbbba2046_401_0.png)

## Quick Summary

- JNI is not a solution to performance problems. Java code will almost always run faster than calling into native code.
- When JNI is used, limit the number of calls from Java to C; crossing the JNI boundary is expensive.
- JNI code that uses arrays or strings must pin those objects; limit the length of time they are pinned so that the garbage collector is not impacted.

## Exceptions

Java exception processing has the reputation of being expensive. It is somewhat more expensive than processing regular control flows, though in most cases, the extra cost isn’t worth the effort to attempt to bypass it. On the other hand, because it isn’t free, exception processing shouldn’t be used as a general mechanism either. The guideline is to use exceptions according to the general principles of good program design: mainly, code should throw an exception only to indicate something unexpected has happened. Following good code design means that your Java code will not be slowed down by exception processing.

Two things can affect the general performance of exception processing. First is the code block itself: is it expensive to set up a try-catch block? While that might have been the case a long time ago, it has not been the case for years. Still, because the internet has a long memory, you will sometimes see recommendations to avoid exceptions simply because of the try-catch block. Those recommendations are out-of-date; modern JVMs can generate code that handles exceptions quite efficiently.

The second aspect is that exceptions involve obtaining a stack trace at the point of the exception (though you’ll see an exception to that later in this section). This operation can be expensive, particularly if the stack trace is deep.

Since the performance penalty comes from filling in the stack traces, the -XX:-StackTraceInThrowable flag (which is true by default) can be set to disable the generation of the stack traces.

This is rarely a good idea: the stack traces are present to enable analysis of what unexpectedly went wrong. That capability is lost when this flag is enabled. And there is code that actually examines the stack traces and determines how to recover from the exception based on what it finds there. That’s problematic in itself, but the upshot is that disabling the stack trace can mysteriously break code.

There are some APIs in the JDK itself where exception handling can lead to performance issues. Many collection classes will throw an exception when nonexistent items are retrieved from them. The Stack class, for example, throws an EmptyStackException if the stack is empty when the pop() method is called. It is usually better to utilize defensive programming in that case by checking the stack length first. (On the other hand, unlike many collection classes, the Stack class supports null objects, so it’s not as if the pop() method could return null to indicate an empty stack.)

The most notorious example within the JDK of questionable use of exceptions is in classloading: the loadClass() method of the ClassLoader class throws a ClassNotFoundException when asked to load a class that it cannot find. That’s not actually an exceptional condition. An individual classloader is not expected to know how to load every class in an application, which is why there are hierarchies of classloaders.

In an environment with dozens of classloaders, this means a lot of exceptions are created as the classloader hierarchy is searched for the one classloader that knows how to load the given class. In very large application servers I’ve worked with, disabling stack trace generation can speed up start time by as much as 3%. Those servers load more than 30,000 classes from hundreds of JAR files; this is certainly a YMMV kind of thing.³
