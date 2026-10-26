# Introduction

In the previous chapter, we studied some commonly used data structures. Data structures are organizations of data in different forms, and a data structure enables and controls the cost of access to the data stored inside it. However, what makes software useful is not just the ability to store and retrieve data in various formats, but the ability to make transformations on data in order to solve computational problems. For a given problem, the precise definition and order of transformations on data is determined by a sequence of instructions called an **algorithm**.

An algorithm takes in a set of inputs that define an instance of a problem, applies a series of transformations, and outputs a set of results. If these results are the correct solutions to the computational problem at hand, our algorithm is said to be correct. The goodness of an algorithm is determined by its efficiency, or how few instructions the algorithm needs to perform to produce correct results:

![](bbox=[0.097, 0.391, 0.891, 0.729])

**Figure 4.1:** Scaling of steps taken by an algorithm with respect to the size of the input

The preceding diagram shows the growth in the number of steps required by an algorithm as a function of the size of the input. Algorithms that are more complex grow more quickly with the size of the input, and with sufficiently large inputs they can become infeasible to run, even on modern computer systems. For instance, let's assume that we have a computer that can perform a million operations per second. For an input of size 50, an algorithm that takes N log(N) steps will take 283 microseconds to complete; an algorithm that takes N² steps will take 2.5 milliseconds; and an algorithm that takes N! (factorial of N) steps would take approximately 9,637,644,561,599,544,267,027,654,516,581,964,749,586,575,812,734.82 centuries to run!