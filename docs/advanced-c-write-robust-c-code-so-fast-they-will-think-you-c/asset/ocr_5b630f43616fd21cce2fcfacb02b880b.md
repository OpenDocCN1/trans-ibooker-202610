The interesting part occurs if the T class has the `find()` method as both of the `test_find()` methods are generated. But the specialized version takes an argument of the `int` type while the default takes an argument of the `long` type. As we "call" the function with zero (0), it will match the specialized version and use it. The argument difference is important because you cannot have two functions with the same argument types and only differ by return type. If you want to check this behavior, then change the argument from 0 to 0L to force the long version.

# Class Templates

We have only dealt with function templates so far. But templates can also be used to provide blueprints for classes. The general structure of a templated class declaration is as follows:

```
template<class T>

class MyClass {

    // variables and methods that use T.

};
```

Whereas template functions allow us to produce generic algorithms, template classes allow us to produce generic data types and their associated behaviors.

When we introduced the Standard Template Library, we highlighted that it includes templates for containers – **vector**, **deque**, **stack**, and so on. These templates allow us to store and manage any data type that we want, but still behave as we would expect.

# Exercise 4: Writing a Class Template

Two of the most commonly used data structures in computing science are stack and queue. Both currently have implementations in the STL. But to get our feet wet with a templated class, we are going to write a stack template class that can be used for any type. Let's get started:

+   1. Open the **Lesson2B** project in Eclipse, and then in the **Project Explorer**, expand **Lesson2B**, then **Exercise04**, and double-click on **Exercise4.cpp** to open the file for this exercise in the editor.
+   2. Configure a new **Launch Configuration, L2BExercise4**, to run with the name **Exercise4**.
+   3. Also, configure a new C/C++ Unit Run Configuration, **L2BEx4Tests**, to run **L2BEx4tests**. Set the **Google Tests Runner**.
+   4. Click on the **Run** option for the test, which we have to run for the first time: