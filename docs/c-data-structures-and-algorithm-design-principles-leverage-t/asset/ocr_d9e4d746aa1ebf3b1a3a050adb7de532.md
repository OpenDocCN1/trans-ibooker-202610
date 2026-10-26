An algorithm is said to be efficient if, for the size of input N, it solves the problem in a number of steps that is a polynomial of N.

The problems that express **polynomial-time algorithms** as solutions are also said to belong to the class P (polynomial) of computational complexity. There are several other computational complexities that problems can be divided into, a few examples of which are given here:

+   - **NP (Non-Deterministic Polynomial Time)** problems have solutions that can be verified in polynomial time, but do not have any known polynomial-time solutions.
+   - **EXPTIME (Exponential Time)** problems have solutions that run in time exponential to the size of the input.
+   - **PSPACE (Polynomial Space)** problems require a polynomial amount of space.

Finding out whether the set of problems in P is exactly the same as the set of problems in NP is the famous P = NP problem, which remains unsolved after decades of efforts and even carries a $1 million prize for anyone who can solve it. We shall take another look at P and NP-type problems in Chapter 9, Dynamic Programming II.

Algorithms have been studied as mathematical objects by computer scientists for several decades and a set of general approaches (or **paradigms**) to design efficient algorithms have been identified that can be used to solve a wide variety of problems. One of the most widely applicable algorithm design paradigms is called *divide and conquer* and shall be our subject of study in this chapter.

A **divide-and-conquer** type algorithm breaks the given problem into smaller parts, tries to solve the problem for each part, and, finally, combines the solution for each part into the solution for the whole problem. Several widely used algorithms fall into this category, for example, binary search, quicksort, merge sort, matrix multiplication, Fast Fourier Transform, and the skyline algorithms. These algorithms appear in almost all the major applications that are used today, including databases, web browsers, and even language runtimes such as the Java Virtual Machine and the V8 JavaScript engine.

In this chapter, we will show you what it means to solve problems using divide and conquer, and how you can identify whether your problem is amenable to such a solution. Next, we will practice thinking recursively and show you the tools that the modern C++ Standard Library gives you so that you can solve your problems using divide and conquer. We'll end this chapter by looking at MapReduce, including a discussion on why and how it scales, and how you can use the same paradigm to scale up your programs using both CPU-level and machine-level parallelization.

Let's dive into a basic algorithm that uses the divide-and-conquer approach – binary search.