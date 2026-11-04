# Understanding the PHP async programming model

Before we get into the details of how to develop PHP applications using asynchronous libraries, it's important to step back and have a look at the PHP **asynchronous programming model**. Understanding the difference between this and the conventional **synchronous programming model** opens a new world of high performance for you to utilize when developing PHP applications. Let's first have a look at the synchronous programming model, after which we'll dive into async.

# Developing synchronous programming code

In traditional PHP programming, code executes in a linear fashion. Once the code has been compiled into machine code, the CPU executes the code one line after another in a sequential manner until the code ends. This is certainly true of PHP *procedural programming*. Surprising to some, this is also true for **object-oriented programming (OOP)** as well! Regardless of whether or not you use objects as part of your code, the OOP code gets compiled into first-byte code and then machine code, and is processed in a synchronous manner in exactly the same manner as is procedural code.

Using OPcache and the **Just-in-Time (JIT)** compiler has no bearing on whether or not the code operates in a synchronous manner. The only thing that OPcache and the JIT compiler bring to the table is the ability to operate synchronous code faster than was otherwise possible.

> **Important note**
> 
> Please do not get the impression that there is something wrong with writing code using the synchronous programming model! This approach is not only tried and true but also quite successful. Furthermore, synchronous code is supported by many ancillary tools such as PHPUnit, Xdebug, numerous frameworks, and many others.

There is a major drawback to the synchronous programming model, however. Using this model, the CPU has to constantly wait for certain tasks to complete before the program is allowed to move along. To a large extent, such tasks include access to an external resource, such as making a database query, writing to a log file, or sending an email. Such tasks are referred to as **blocking operations** (operations that block progress).