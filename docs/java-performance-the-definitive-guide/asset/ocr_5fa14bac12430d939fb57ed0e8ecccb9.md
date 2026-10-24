in two ways: first, you can set the system property -Djava.security.egd=file:/dev/urandom.²

A third option is to change this setting in $JAVA_HOME/jre/lib/security/java.security:

```
securerandom.source=file:/dev/random
```

That line defines the interface used for seeding operations and can be set to /dev/urandom if you want to ensure that the secure random number generator never blocks.

However, the better solution is to set up the operating system so that it supplies more entropy, which is done by running the rngd daemon. Just make sure that the rngd daemon is configured to use reliable hardware sources of entropy (e.g., /dev/hwrng if it is available) and not something like /dev/urandom. This solution has the advantage of solving entropy issues for all programs on the machine, not just Java programs.

![](bbox=[0.15, 0.365, 0.25, 0.475])

# Quick Summary

+   - Java's default Random class is expensive to initialize, but once initialized, it can be reused.
+   - In multithreaded code, the ThreadLocalRandom class is preferred.
+   - Sometimes, the SecureRandom class will show arbitrary, completely random performance. Performance tests on code using that class must be carefully planned.
+   - Issues with the SecureRandom class blocking can be avoided with configuration changes, but it is better to solve them at the OS level by adding entropy to the system.

# Java Native Interface

Performance tips about Java SE (particularly in the early days of Java) often say that if you want really fast code, you should use native code. In truth, if you are interested in writing the fastest possible code, avoid the Java Native Interface (JNI).

Well-written Java code will run at least as fast on current versions of the JVM as corresponding C or C++ code (it is not 1996 anymore). Language purists will continue to debate the relative performance merits of Java and other languages, and you can find doubtless examples of an application written in another language that is faster

> 2 Because of a bug in earlier versions of Java, it is sometimes recommended to set this flag to /dev//urandom or other variations. In Java 8 and later JVMs, you can simply use /dev/urandom.