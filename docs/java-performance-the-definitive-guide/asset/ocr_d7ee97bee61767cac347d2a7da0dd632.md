```
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

# Table 12-8. Time to calculate across the JNI boundary

| calculateError | Calc | Random | JNI transitions | Total time |
|---|---|---|---|---|
| Java | Java | Java | 0 | 0.104 ± 0.01 seconds |
| Java | Java | C | 10,000,000 | 1.96 ± 0.1 seconds |
| Java | C | C | 10,000 | 0.132 ± 0.01 seconds |
| C | C | C | 0 | 0.139 ± 0.01 seconds |

Implementing only the innermost method in C provides the most crossings of the JNI boundary (numberOfTrials × numberOfLoops, or 10 million). Reducing the number of crossings to numberOfTrials (10,000) reduces that overhead substantially, and reducing it further to 0 provides the best performance.

JNI code performs worse if the parameters involved are not simple primitives. Two aspects are involved in this overhead. First, for simple references, an address translation is needed. Second, operations on array-based data are subject to special handling in native code. This includes String objects, since the string data is essentially a character array. To access the individual elements of these arrays, a special call must be made to pin the object in memory (and for String objects, to convert from Java's UTF-16 encoding into UTF-8 in JDK 8). When the array is no longer needed, it must be explicitly released in the JNI code.

While the array is pinned, the garbage collector cannot run—so one of the most expensive mistakes in JNI code is to pin a string or array in code that is long-running. That prevents the garbage collector from running, effectively blocking all the application threads until the JNI code completes. It is extremely important to make the critical section where the array is pinned as short as possible.