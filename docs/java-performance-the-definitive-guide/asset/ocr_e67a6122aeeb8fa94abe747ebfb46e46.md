**Table 12-6. Time to serialize and deserialize Stock object with compression**

| Mode | Time |
|---|---|
| Unbuffered compression/decompression | 21.3 ± 8 ms |
| Buffered compression/decompression | 5.7 ± 0.08 ms |

The failure to properly buffer the I/O resulted in as much as a four times performance penalty.

![](bbox=[0.168, 0.243, 0.252, 0.321])

# Quick Summary

+   - Issues around buffered I/O are common because of the default implementation of the simple input and output stream classes.
+   - I/O must be properly buffered for files and sockets, as well as for internal operations like compression and string encoding.

# Classloading

The performance of classloading is the bane of anyone attempting to optimize either program startup or deployment of new code in a dynamic system.

There are many reasons for that. To begin, the class data (i.e., the Java bytecode) is typically not quickly accessible. That data must be loaded from disk or from the network, it must be found in one of several JAR files on the classpath, and it must be found in one of several classloaders. There are some ways to help this along: some frameworks cache classes they read from the network into a hidden directory so that next time it starts the same application, it can read the classes more quickly. Packaging an application into fewer JAR files will also speed up its classloading performance.

In this section, we’ll look at a new feature of Java 11 to speed up classloading.

# Class Data Sharing

*Class data sharing* (CDS) is a mechanism whereby the metadata for classes can be shared between JVMs. This can be useful for saving memory when running multiple JVMs: normally, each JVM would have its own class metadata, and the separate copies would occupy some physical memory. If that metadata is shared, only one copy needs to reside in memory.

It turns out that CDS is very useful for single JVMs because it can also improve their startup time.