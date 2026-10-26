```
```cpp
return 0;
}
```
```

Then we get the following output:

![](bbox=[0.257, 0.169, 0.607, 0.272])

**Figure 4.16:** Program output using std::bind() and std::function

Several points to note:

+   - The function **method()** is referenced using the class as a scope specifier;
+   - The address of the instance of **Binder** is passed as the second argument to **std::bind()** which makes it the first argument passed to **method()**. This is necessary as all non-static members have an implicit **this** pointer passed as the first argument.
+   - Using the **std::placeholders** definitions, we can bind arguments used when invoking the bound method and even change the order passed (as seen by **func3**).

C++11 introduced some syntactic sugar, known as lambda expressions, that make it easier to define anonymous functions that can also be used to bind methods and assign them to **std::function** instances expression. We'll cover more about it in the topic *What is a Lambda Expression?*

# Exercise 3: Implementing callbacks with std::function

In this exercise, we will implement function callbacks utilizing the **std::function<>** template. Follow the below steps to implement this exercise:

+   1. Open the **Lesson4** project in Eclipse, and then in the **Project Explorer**, expand **Lesson4** then **Exercise03** and double click on **Exercise3.cpp** to open the file for this exercise into the editor.
+   2. As this is a CMake based project, change the current builder to be CMake Build (portable).
+   3. Click on the **Launch Configuration** drop down menu and select **New Launch Configuration...**. Configure **L4Exercise3** to run with the name **Exercise3**.