# Technical requirements

To examine and run the code examples provided in this chapter, the minimum recommended hardware is listed here:

+   - x86_64-based desktop PC or laptop
+   - 1 gigabyte (GB) free disk space
+   - 4 GB of random-access memory (RAM)
+   - 500 kilobits per second (Kbps) or faster internet connection
+   - In addition, you need to install the following software:
+   - Docker
+   - Docker Compose

Please refer to the *Technical requirements* section of *Chapter 1, Introducing New PHP 8 OOP Features*, for more information on Docker and Docker Compose installation, as well as how to build the Docker container used to demonstrate code explained in this book. Throughout, we refer to the directory in which you restored the sample code for this book as `/repo`.

The source code for this chapter is located here: `https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices.`

We can now begin our discussion by examining new PHP 8 operators.

# Working with new PHP 8 operators

A number of new **operators** have been introduced with PHP 8. In addition, PHP 8 generally introduces a uniform and consistent manner in which these operators can be used. In this section, we examine the following operators:

+   - Variadics operator
+   - Nullsafe operator
+   - Concatenate operator
+   - Ternary operator

Let's start with a discussion of the variadics operator.