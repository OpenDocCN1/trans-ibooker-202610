
Languages like PHP and Perl, on the other hand, are interpreted. The same program source code can be run on any CPU as long as the machine has the correct interpreter (that is, the program called php or perl). The interpreter translates each line of the program into binary code as that line is executed.

Each system has advantages and disadvantages. Programs written in interpreted languages are portable: you can take the same code and drop it on any machine with the appropriate interpreter, and it will run. However, it might run slowly. As a simple case, consider what happens in a loop: the interpreter will retranslate each line of code when it is executed in the loop. The compiled code doesn't need to repeatedly make that translation.

A good compiler takes several factors into account when it produces a binary. One simple example is the order of the binary statements: not all assembly language instructions take the same amount of time to execute. A statement that adds the values stored in two registers might execute in one cycle, but retrieving (from main memory) the values needed for the addition may take multiple cycles.

Hence, a good compiler will produce a binary that executes the statement to load the data, executes other instructions, and then—when the data is available—executes the addition. An interpreter that is looking at only one line of code at a time doesn't have enough information to produce that kind of code; it will request the data from memory, wait for it to become available, and then execute the addition. Bad compilers will do the same thing, by the way, and it is not necessarily the case that even the best compiler can prevent the occasional wait for an instruction to complete.

For these (and other) reasons, interpreted code will almost always be measurably slower than compiled code: compilers have enough information about the program to provide optimizations to the binary code that an interpreter simply cannot perform.

Interpreted code does have the advantage of portability. A binary compiled for an ARM CPU obviously cannot run on an Intel CPU. But a binary that uses the latest AVX instructions of Intel's Sandy Bridge processors cannot run on older Intel processors either. Hence, commercial software is commonly compiled to a fairly old version of a processor and does not take advantage of the newest instructions available to it. Various tricks around this exist, including shipping a binary with multiple shared libraries that execute performance-sensitive code and come with versions for various flavors of a CPU.

Java attempts to find a middle ground here. Java applications are compiled—but instead of being compiled into a specific binary for a specific CPU, they are compiled into an intermediate low-level language. This language (known as Java bytecode) is then run by the java binary (in the same way that an interpreted PHP script is run by the php binary). This gives Java the platform independence of an interpreted language. Because it is executing an idealized binary code, the java program is able to compile the code into the platform binary as the code executes. This compilation occurs as the program is executed: it happens “just in time.”

This compilation is still subject to platform dependencies. JDK 8, for example, cannot generate code for the latest instruction set of Intel’s Skylake processors, though JDK 11 can. I’ll have more to say about that in “Advanced Compiler Flags” on page 105.

The manner in which the Java Virtual Machine compiles this code as it executes is the focus of this chapter.

## HotSpot Compilation

As discussed in Chapter 1, the Java implementation discussed in this book is Oracle’s HotSpot JVM. This name (HotSpot) comes from the approach it takes toward compiling the code. In a typical program, only a small subset of code is executed frequently, and the performance of an application depends primarily on how fast those sections of code are executed. These critical sections are known as the hot spots of the application; the more the section of code is executed, the hotter that section is said to be.

Hence, when the JVM executes code, it does not begin compiling the code immediately. There are two basic reasons for this. First, if the code is going to be executed only once, then compiling it is essentially a wasted effort; it will be faster to interpret the Java bytecodes than to compile them and execute (only once) the compiled code.

But if the code in question is a frequently called method or a loop that runs many iterations, then compiling it is worthwhile: the cycles it takes to compile the code will be outweighed by the savings in multiple executions of the faster compiled code. That trade-off is one reason that the compiler executes the interpreted code first—the compiler can figure out which methods are called frequently enough to warrant their compilation.

The second reason is one of optimization: the more times that the JVM executes a particular method or loop, the more information it has about that code. This allows the JVM to make numerous optimizations when it compiles the code.

Those optimizations (and ways to affect them) are discussed later in this chapter, but for a simple example, consider the equals() method. This method exists in every Java object (because it is inherited from the Object class) and is often overridden. When the interpreter encounters the statement b = obj1.equals(obj2), it must look up the type (class) of obj1 in order to know which equals() method to execute. This dynamic lookup can be somewhat time-consuming.

Over time, say, the JVM notices that each time this statement is executed, obj1 is of type java.lang.String. Then the JVM can produce compiled code that directly calls the String.equals() method. Now the code is faster not only because it is compiled but also because it can skip the lookup of which method to call.

It’s not quite as simple as that; it is possible the next time the code is executed that obj1 refers to something other than a String. The JVM will create compiled code that deals with that possibility, which will involve deoptimizing and then reoptimizing the code in question (you’ll see an example in “Deoptimization” on page 101). Nonetheless, the overall compiled code here will be faster (at least as long as obj1 continues to refer to a String) because it skips the lookup of which method to execute. That kind of optimization can be made only after running the code for a while and observing what it does: this is the second reason JIT compilers wait to compile sections of code.

## Registers and Main Memory

One of the most important optimizations a compiler can make involves when to use values from main memory and when to store values in a register. Consider this code:

```
public class RegisterTest {
    private int sum;

    public void calculateSum(int n) {
        for (int i = 0; i < n; i++) {
            sum += i;
        }
    }
}
```

At some point, the sum instance variable must reside in main memory, but retrieving a value from main memory is an expensive operation that takes multiple cycles to complete. If the value of sum were to be retrieved from (and stored back to) main memory on every iteration of this loop, performance would be dismal. Instead, the compiler will load a register with the initial value of sum, perform the loop using that value in the register, and then (at an indeterminate point in time) store the final result from the register back to main memory.

This kind of optimization is very effective, but it means that the semantics of thread synchronization (see Chapter 9) are crucial to the behavior of the application. One thread cannot see the value of a variable stored in the register used by another thread; synchronization makes it possible to know exactly when the register is stored to main memory and available to other threads.

Register usage is a general optimization of the compiler, and typically the JIT will aggressively use registers. We’ll discuss this more in-depth in “Escape Analysis” on page 110.
