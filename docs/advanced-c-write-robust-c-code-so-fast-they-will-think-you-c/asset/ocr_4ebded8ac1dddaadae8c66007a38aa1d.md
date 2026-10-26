```
```cpp
// nitty gritty of the multiplication
}

private:

// Declaration of the 2D array to store matrix.
};
```
```

A rule of thumb is to use a class if there is at least one private member as this implies that the details of the implementation will be behind the public member functions.

# Visibility, Lifetime, and Access

We have talked about creating our own types and declaring variables and functions while mainly focusing on simple functions and a single file. We will now look at what happens when there are multiple source files (translation units) that contain classes and function definitions. Also, we'll check which variables and functions can be visible from the other parts of the source files, how long the variables live, and look at the difference between internal and external linkage. In *Chapter 1, Anatomy of Portable C++ Software*, we saw how the toolchain works to compile the source files and produce the object files and that the linker puts it all together to form an executable program.

When a compiler processes a source file, it generates an object file that contains the translated C++ code and enough information for the linker to resolve any references from the compiled source file to another source file. In *Chapter 1, Anatomy of Portable C++ Software*, **CxxTemplate.cpp** called **sum()**, which is defined in the **SumFunc.cpp** file. When the compiler constructs an object file, it creates the following segments:

+   - **Code segment** (also known as text): This is the translation of the C++ functions into the target machine instructions.
+   - **Data segment**: This contains all the variables and data structures that are declared in the program, not local or allocated off the heap or stack, and are initialized.
+   - **BSS segment**: This contains all the variables and data structures that are declared in the program, not local or allocated off the heap or stack, and are not initialized (but will be initialized to zero).
+   - **Database of exported symbols**: A list of variables and functions that are in this object file and their location.