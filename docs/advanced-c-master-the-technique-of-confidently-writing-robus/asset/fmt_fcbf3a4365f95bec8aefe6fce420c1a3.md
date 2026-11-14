
# Activity: Date Library Development

Your team is in charge of developing a library to help with calculations that are related to dates. In particular, we want to be able to determine the number of days between two dates and given a date, add (or subtract from it) a number of days to get a new date. This activity will develop two new types and enhance them to ensure that the programmer cannot accidentally have them interact with built-in types. Follow these steps to achieve this:

## Steps

1.  Design and implement a `Date` class that stores the `day`, `month`, and `year` as integers.

2.  Add methods to access the internal `day`, `month`, and `year` values.

3.  Define a type, `date_t` to represent the number of days since the **epoch date** of 1-Jan-1970.

4.  Add a method to the `Date` class to convert this into `date_t`.

5.  Add a method to set the `Date` class from a `date_t` value.

6.  Create a `Days` class that stores the days value.

7.  Add the `addition` operator to `Date` that takes `Days` as an argument.

8.  Use `explicit` to prevent the addition of numbers.

9.  Add the `subtraction` operator to return a `Days` value from the `difference` of two `Dates`.

After following these steps, you should receive the following output:

![Figure 3.51: Output of a successful Date sample application](image/C14583_03_51.jpg)

###### Figure 3.51: Output of a successful Date sample application

#### Note

The solution for this activity can be found on page 664.

# Summary

In this chapter, we explored the lifetime of variables - both automatic and dynamic, where they are stored, and when they are destructed. We then used this information to develop `RAII` techniques that allow us to almost ignore resource management because the automatic variables will clean them up when they are destructed even in the presence of an exception. Then, we looked at throwing exceptions and catching them so that we can deal with abnormal conditions at the right level. From `RAII`, we went into a discussion on the ownership of resources and how `STL` smart pointers help us in this area. We discovered that just about everything is treated as a function call, thus allowing operator overloading and implicit conversions. We discovered the wonderful (or is it awful?) world of `argument-dependent lookup` (`ADL`) and how it can potentially trip us up in the future. We now have a good understanding of the fundamental features of C++. In the next chapter we will start to explore function objects and how they are realized and implemented using lambda functions. We will delve further into the offerings of `STL` and explore `PIMPL`s as we re-visit encapsulation.

# Chapter 4: Separation of Concerns - Software Architecture, Functions, and Variadic Templates

## Learning Objectives

By the end of this chapter, you will be able to:

*   Develop classes using the `PIMPL` idiom to implement object-level encapsulation
*   Implement a callback system using functors, `std::function`, and lambda expressions
*   Implement lambdas using the right capture technique for the situation
*   Develop variadic templates to implement C# style delegates for event handling

This chapter will show you how to implement the `PIMPL` idiom, and how to develop a callback mechanism for your own programs.

## Introduction

In the previous chapter, we learned how to implement classes to properly manage resources even when the exceptions occurred using `RAII`. We also learned about `ADL` (`Argument Dependent Lookup`) and how it determines the function to be called. Finally, we talked about how the `explicit` keyword can be used to prevent automatic conversion between types by the compiler, known as implicit conversion.

In this chapter, we will look into dependencies, both physical and logical, and see how they can affect build times adversely. We'll also learn how to separate the visible interface class from the implementation details to increase the speed of the build times. We will then learn to capture functions and contexts so that we can call them later using `functors`, `std::function` and `lambda expressions`. Finally, we will implement a variadic template to deliver an event-based callback mechanism.

## The Pointer to Implementation (PIMPL) Idiom

As projects implemented in C++ get larger and larger, there is the likelihood that the build times will grow at a faster rate than the number of files. This is because of the textual inclusion model used by the C++ build model. This is done so that the compiler can determine the size and layout of the class, causing a coupling between the `caller` and the `callee`, but allowing for optimization. Remember that everything must be defined before it can be used. A future feature called `Modules` promises to address this issue, but for now we need to understand the issue and the techniques used to address the problem.

## Logical and Physical Dependencies

When we wish to access a class from another class, we have a logical dependency. One class is logically dependent upon another class. If we consider the `Graphics` classes, `Point3d` and `Matrix3d` that we developed in *Chapter 2A*, *No Ducks Allowed – Types and Deduction* and *Chapter 3*, *The Distance between Can and Should – Objects, Pointers and Inheritance*, we have two logically independent classes `Matrix3d` and `Point3d`. However, because of how we implemented the multiplication operator between the two, we created a compile-time or **physical dependency**.

![Figure 4.1: Physical Dependencies of Matrix3d and Point3d](image/C14583_04_01.jpg)

###### Figure 4.1: Physical Dependencies of Matrix3d and Point3d

As we can see with these relatively simple classes, the physical dependencies between the header files and the implementation files can quickly get complicated. It is this complexity that contributes to the build times of large projects because the number of physical (and logical) dependencies grows to thousands. We have shown only 13 dependencies in the preceding diagram as indicated by the arrows. But there are actually many more, as including a standard library header typically pulls in a hierarchy of include files. This means that if one header file is modified, then all files that are dependent upon it, either directly or indirectly, will need to be re-compiled to account for the change. This trigger for rebuilding also occurs if the change is to the private class members definition that users of the class cannot even access.

To help speed compile times, we have used the guard technique to prevent header files from being processed multiple times:

```
#if !defined(MY_HEADER_INCLUDED)
#define MY_HEADER_INCLUDED
// definitions
#endif // !defined(MY_HEADER_INCLUDED)
```

And lately, most compilers now support the `#pragma once` directive which achieves the same result.

These relationships between entities (files, classes, and so on) are known as **coupling**. A file/class is **highly coupled** to another file/class if changes to the file/class result in changes to the other file/class. A file/class is **loosely coupled** to another file/class if changes to the file/class do not result in changes to the other file/class.

Highly coupled code (files/classes) introduce problems into a project. Highly coupled code is hard to change (inflexible), hard to test and hard to understand. Loosely coupled code on the other hand is easier to change (only modify one class), highly testable (only need the class being tested) and easier to read and understand. Coupling reflects, and is related to, the logical and physical dependencies.

## The Pointer to Implementation (PIMPL) Idiom
