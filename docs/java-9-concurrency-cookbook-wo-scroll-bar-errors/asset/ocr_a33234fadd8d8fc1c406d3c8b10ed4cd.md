# Interrupting a thread

A Java program with more than one execution thread only finishes when the execution of all of its threads end (more specifically, when all its non-daemon threads end their execution or when one of the threads uses the System.exit() method). Sometimes, you may need to finish a thread because you want to terminate a program or when a user of the program wants to cancel the tasks that a thread object is doing.

Java provides an interruption mechanism that indicates to a thread that you want to finish it. One peculiarity of this mechanism is that thread objects have to check whether they have been interrupted or not, and they can decide whether they respond to the finalization request or not. A thread object can ignore it and continue with its execution.

In this recipe, we will develop a program that creates a thread and forces its finalization after 5 seconds, using the interruption mechanism.

# Getting ready

The example for this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class called PrimeGenerator that extends the Thread class:
+   2. Override the run() method including a loop that will run indefinitely. In this loop, process consecutive numbers beginning from one. For each number, calculate whether it's a prime number; if yes, as in this case, write it to the console:

```
public class PrimeGenerator extends Thread{
```

```
@Override
public void run() {
    long number=1L;
    while (true) {
        if (isPrime(number)) {
            System.out.printf("Number %d is Prime\n",number);
        }
    }
```