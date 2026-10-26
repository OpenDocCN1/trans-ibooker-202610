The expected output should be similar to the following:

![](bbox=[0.285, 0.11, 0.78, 0.27])

Figure 1.19: Final output

Note

The solution for this activity can be found on page 620.

In the following section, we will talk about how to write unit tests for our projects. It is common to divide projects into many classes and functions that work together to achieve the desired goal. You must manage the behavior of these classes and functions with unit tests to ensure that they behave in expected ways.

# Unit Testing

Unit tests are an important part of programming in general. Basically, unit tests are little programs that use our classes in various scenarios with expected results, live in a parallel file hierarchy in our project, and do not end up in the actual executable but are executed separately by us during development to ensure that our code is behaving in expected ways. We should write unit tests for our C++ programs to ensure that they behave as they are supposed to after each change.

# Preparing for the Unit Tests

There are several C++ test frameworks that we can use with CMake. We will use Google Test, which has several benefits over other options. In the next exercise, we will prepare our project for unit testing with Google Test.