This time we only have one error at the call site to **test()**, but not for the if condition. We can fix this error by using a C-style case (bool) or a C++ **static_cast<bool>(real)** (this is the preferred method). When we add the cast, the program compiles and runs once more.

So, if the bool cast is explicit, then why does the condition for the if expression not need a cast?

The C++ standard allows in certain contexts where the **bool** type is expected and the declaration for a bool conversion exists (whether marked explicit or not). If this happens, then an implicit conversion is allowed. This is known as **contextually converting into bool** and can occur in the following contexts:

+   - The condition (or controlling expression) of **if**, **while**, **for**
+   - The operands of the built-in logical operators **!** (not), **&&** (and) and **||** (or)
+   - The first operand of the ternary (or conditional) operator **?:**

# Exercise 6: Implicit and Explicit Conversions

In this exercise we will experiment with calling functions, implicit conversions, preventing them, and enabling them. Follow these steps to implement this exercise:

+   1. Open the **Lesson3** project in Eclipse. Then in the **Project Explorer**, expand **Lesson3** then **Exercise06** and double click on **Exercise6.cpp** to open the file for this exercise into the editor.
+   2. Click on the **Launch Configuration** drop down menu and select **New Launch Configuration...** Configure the **L3Exercise6** application from the **Search Project** menu so that it runs with the name **L3Exercise6**.
+   3. Click on the **Run** button to run Exercise 6. This will produce the following output:

![](bbox=[0.32, 0.71, 0.59, 0.82])

Figure 3.44: Default output from Exercise 6