This is once again the performance of our REST server (though on slightly different hardware than before, so the baseline OPS is only 20.5 OPS instead of 24.4).

It’s interesting to note the progression here: JDK 11 was built with a pretty early version of the Graal compiler, so the performance of that compiler lags the C2 compiler. The Graal compiler improved through its early access builds, though even its latest early access (1.0) build wasn’t as fast as the standard VM. Graal versions in late 2019 (released as production version 19.2.1), though, got substantially faster. The early access release of JDK 13 has one of those later builds and achieves close to the same performance with the Graal compiler, even while its C2 compiler is only modestly improved since JDK 11.

# Precompilation

We began this chapter by discussing the philosophy behind a just-in-time compiler. Although it has its advantages, code is still subject to a warm-up period before it executes. What if in our environment a traditional compiled model would work better: an embedded system without the extra memory the JIT requires, or a program that completes before having a chance to warm up?

In this section, we’ll look at two experimental features that address that scenario. Ahead-of-time compilation is an experimental feature of the standard JDK 11, and the ability to produce a fully native binary is a feature of the Graal VM.

# Ahead-of-Time Compilation

*Ahead-of-time (AOT) compilation* was first available in JDK 9 for Linux only, but in JDK 11 it is available on all platforms. From a performance standpoint, it is still a work in progress, but this section will give you a sneak peek at it.¹

AOT compilation allows you to compile some (or all) of your application in advance of running it. This compiled code becomes a shared library that the JVM uses when starting the application. In theory, this means the JIT needn’t be involved, at least in the startup of your application: your code should initially run at least as well as the C1 compiled code without having to wait for that code to be compiled.

In practice, it’s a little different: the startup time of the application is greatly affected by the size of the shared library (and hence the time to load that shared library into the JVM). That means a simple application like a “Hello, world” application won’t run any faster when you use AOT compilation (in fact, it may run slower depending on

> ¹ One benefit of AOC compilation is faster startup, but application class data sharing gives—at least for now—a better benefit in terms of startup performance and is a fully supported feature; see “Class Data Sharing” on page 377 for more details.