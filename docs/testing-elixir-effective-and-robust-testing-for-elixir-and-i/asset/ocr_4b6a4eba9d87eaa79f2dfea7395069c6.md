# Test Cases

Every module whose name ends with a trailing Test and that contains a call to use ExUnit is considered a test case. A test case is essentially a collection of setup callbacks and tests.

The most important thing to know here is that test cases can be executed either concurrently or sequentially in relation to other test cases. All the test cases that use the async: true option in use ExUnit are executed concurrently. Then, the rest of the test cases are executed sequentially (in the order they were defined), by default in random order.

![](bbox=[0.14, 0.43, 0.56, 0.77])

As we mentioned, a test case by itself doesn’t contain any logic. It’s only a container for setup callbacks and tests. The execution of a test case goes like this:

+   1. All `setup_all` callbacks are executed.