compiler). Those names are more apt now, since any distinction between a client and server computer is long gone, so we’ll adopt those names throughout.

The primary difference between the two compilers is their aggressiveness in compiling code. The C1 compiler begins compiling sooner than the C2 compiler does. This means that during the beginning of code execution, the C1 compiler will be faster, because it will have compiled correspondingly more code than the C2 compiler.

The engineering trade-off here is the knowledge the C2 compiler gains while it waits: that knowledge allows the C2 compiler to make better optimizations in the compiled code. Ultimately, code produced by the C2 compiler will be faster than that produced by the C1 compiler. From a user’s perspective, the benefit to that trade-off is based on how long the program will run and how important the startup time of the program is.

When these compilers were separate, the obvious question was why there needed to be a choice at all: couldn’t the JVM start with the C1 compiler and then use the C2 compiler as code gets hotter? That technique is known as tiered compilation, and it is the technique all JVMs now use. It can be explicitly disabled with the -XX:-TieredCompilation flag (the default value of which is true); in “Advanced Compiler Flags” on page 105, we’ll discuss the ramifications of doing that.

# Common Compiler Flags

Two commonly used flags affect the JIT compiler; we’ll look at them in this section.

# Tuning the Code Cache

When the JVM compiles code, it holds the set of assembly-language instructions in the code cache. The code cache has a fixed size, and once it has filled up, the JVM is not able to compile any additional code.

It is easy to see the potential issue here if the code cache is too small. Some hot methods will get compiled, but others will not: the application will end up running a lot of (very slow) interpreted code.

When the code cache fills up, the JVM spits out this warning:

```
Java HotSpot(TM) 64-Bit Server VM warning: CodeCache is full.
    Compiler has been disabled.
Java HotSpot(TM) 64-Bit Server VM warning: Try increasing the
    code cache size using -XX:ReservedCodeCacheSize=
```

It is sometimes easy to miss this message; another way to determine if the compiler has ceased to compile code is to follow the output of the compilation log discussed later in this section.