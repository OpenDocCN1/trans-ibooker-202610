loop is still running—it would be inefficient to wait until the loop and enclosing method exit (which may not even happen). Hence, when the code for the loop has finished compiling, the JVM replaces the code (on stack), and the next iteration of the loop will execute the much faster compiled version of the code. This is OSR.

The next two attributes should be self-explanatory. The blocking flag will never be printed by default in current versions of Java; it indicates that compilation did not occur in the background (see “Compilation Threads” on page 107 for more details). Finally, the native attribute indicates that the JVM generated compiled code to facilitate the call into a native method.

If tiered compilation has been disabled, the next field (tiered_level) will be blank. Otherwise, it will be a number indicating which tier has completed compilation.

Next comes the name of the method being compiled (or the method containing the loop being compiled for OSR), which is printed as ClassName::method.

Next is the size (in bytes) of the code being compiled. This is the size of the Java bytecodes, not the size of the compiled code (so, unfortunately, this can’t be used to predict how large to size the code cache).

Finally, in some cases a message at the end of the compilation line will indicate that some sort of deoptimization has occurred; these are typically the phrase made not entrant or made zombie. See “Deoptimization” on page 101 for more details.

# Inspecting Compilation with jstat

Seeing the compilation log requires that the program be started with the -XX:+PrintCompilation flag. If the program was started without that flag, you can get limited visibility into the working of the compiler by using jstat.

jstat has two options to provide information about the compiler. The -compiler option supplies summary information about the number of methods compiled (here 5003 is the process ID of the program to be inspected):

```
```text
% jstat -compiler 5003
Compiled Failed Invalid   Time   FailedType FailedMethod
     206      0       0   1.97            0
```
```

Note this also lists the number of methods that failed to compile and the name of the last method that failed to compile; if profiles or other information lead you to suspect that a method is slow because it hasn’t been compiled, this is an easy way to verify that hypothesis.

Alternatively, you can use the -printcompilation option to get information about the last method that is compiled. Because jstat takes an optional argument to repeat its