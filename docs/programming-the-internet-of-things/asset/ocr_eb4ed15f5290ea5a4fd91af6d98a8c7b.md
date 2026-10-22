# NOTE

What’s the difference between a function and a method? To grossly oversimplify, a function is a named grouping of code that performs a task (such as adding two numbers together) and returns a result. If the function accepts any input, it will be passed as one or more parameters. A method is almost identical to a function, but is attached to an object. In object oriented parlance, an object is simply a class that’s been instantiated, and a class is the formal definition of a component - its methods, parameters, construction, and deconstruction logic. All of the Java examples in this book will be represented in class form with methods defined as part of each class. Python can be written in script form with functions or as classes with methods, but I prefer to write Python classes with methods and will do so for each Python example shown in this book, with only a few exceptions to this.

# Unit, Integration, and Performance Testing

There are many ways to test software applications and systems, and some excellent books, articles and blogs on the subject. Developing a working IoT solution requires careful attention to testing - within an application and between different applications and systems. For the purposes of the solution you’ll develop, I’ll focus on just three: Unit tests, integration tests, and performance tests.

Unit tests are code modules written to test the smallest possible unit of code that’s accessible to the test, such as a function or method. These tests are written to verify a set of