It is essential to have a vocabulary of algorithms. We do not need to be an algorithm expert, but we need to at least be aware of the existence of efficient algorithms in various domains, even if we are not capable of implementing them from scratch. A slightly deeper knowledge of algorithms will help us find parts of our program that perform similar, if not exactly the same, computations as well-known algorithms. Certain code features such as nested loops or linear scanning of data are often obvious candidates for improvement, provided we can verify that these constructs are within hotspots in the code. A **hotspot** is a section of code that runs very often and affects performance significantly. The C++ standard library includes a lot of basic algorithms that can be used as building blocks to improve the efficiency of many common operations.

# Studying Generated Assembly Code

**Assembly language** is a human readable representation of the binary machine code that actually executes on the processor. For any serious programmer of a compiled language such as C++, a basic understanding of assembly language is a great asset.

Studying the generated assembly code for a program can give us some good insights into how the compiler works and estimates of code efficiency. There are many cases where this is the only approach possible to determine efficiency bottlenecks.

Apart from this, a basic knowledge of assembly language is essential to be able to debug C++ code since some of the most difficult bugs to catch are those related to the low-level generated code.

A very powerful and popular online tool that's used for analyzing compiler-generated code is the **Compiler Explorer** that we will be using in this chapter.

> **Note**
> 
> The **Godbolt compiler explorer** can be found at https://godbolt.org.