
![](img/6507d435e2edfa3c00c220d0ccfcaf94_57_0.png)

Figure 1.37: The full build output

Here are some of the lines from this output. The following lines are the important ones related to the compilation and linkage of the main executable:

/usr/bin/c++   -g   -pthread -std=gnu++1z -o CMakeFiles/CxxTemplate.dir/src/CxxTemplate.cpp.o -c /home/username/Packt/Cpp2019/CxxTemplate/src/CxxTemplate.cpp

/usr/bin/c++   -g   -pthread -std=gnu++1z -o CMakeFiles/CxxTemplate.dir/src/ANewClass.cpp.o -c /home/username/Packt/Cpp2019/CxxTemplate/src/ANewClass.cpp

/usr/bin/c++   -g   -pthread -std=gnu++1z -o CMakeFiles/CxxTemplate.dir/src/SumFunc.cpp.o -c /home/username/Packt/Cpp2019/CxxTemplate/src/SumFunc.cpp

/usr/bin/c++  -g  -pthread -std=gnu++1z -o CMakeFiles/CxxTemplate.dir/src/LinearMotion1D.cpp.o -c /home/username/Packt/Cpp2019/CxxTemplate/src/LinearMotion1D.cpp

/usr/bin/c++ -g  CMakeFiles/CxxTemplate.dir/src/CxxTemplate.cpp.o CMakeFiles/CxxTemplate.dir/src/ANewClass.cpp.o CMakeFiles/CxxTemplate.dir/src/SumFunc.cpp.o CMakeFiles/CxxTemplate.dir/src/LinearMotion1D.cpp.o  -o CxxTemplate -pthread

4. The c++ command here is just a symbolic link to the g++ compiler. To see that it's actually a chain of symbolic links, type the following command:

```
namei /usr/bin/c++
```

You will see the following output:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_58_0.png)

Figure 1.38: The chain of symbolic links for /usr/bin/c++

Therefore, we will use c++ and g++ interchangeably throughout our discussion. In the build output that we quoted earlier, the first four lines are compiling each .cpp source file and creating the corresponding .o object file. The last line is linking together these object files to create the CxxTemplate executable. The following figure visually presents this process:

8. In the next step, link the object files together to form our executable. Type the following command:

/usr/bin/c++  CxxTemplate.o ANewClass.o SumFunc.o LinearMotion1D.o  -o
CxxTemplate

9. Now, let's see our executable among the list of files here by typing the following command:

```
ls
```

This shows the new CxxTemplate file in the following figure:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_60_0.png)

Figure 1.41: Linked executable file

10. Now, run our executable by typing the following command:

```
./CxxTemplate
```

And see the output that we had before:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_60_1.png)

Figure 1.42: Executable file output

Now that you have examined the details of the build process and have recreated them yourself, in the next section, let's explore the linking process.

## The Linking Step

In this section, let's look at a connection between two source files and how they end up in the same executable. Look at the sum function in the following figure:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_61_0.png)

Figure 1.43: The linking process

The sum function's body is defined in SumFunc.cpp. It has a forward declaration in SumFunc.h. This way, the source files that want to use the sum function can know about its signature. Once they know its signature, they can call it and trust that the actual function definition will be there during runtime, without actually having any interaction with SumFunc.cpp where the function is defined.

After compilation, CxxTemplate.cpp, which calls the sum function, carries that call into its object file. Still, it does not know where the function definition is. The object file of SumFunc.cpp has that definition, but it has nothing to do with CxxTemplate.o, yet.

During the linking step, the linker matches the call in CxxTemplate.o with the definition in SumFunc.o. The call works fine in the executable as a result. Had the linker not found the definition of the sum function, it would have given a linker error.

The linker finds the sum function by its name and parameters. This is called resolving a symbol. The classes, functions, and variables defined in object files are placed in a symbol table and each reference to a symbol is resolved via a lookup at this table. When the symbol is not there, you receive a symbol could not be resolved error.

This took us through the two stages of the build process: **compilation** and **linking**. Notice that we used rather simpler commands compared to that of CMake, when we compiled our sources manually. Feel free to type `man g++` to see all the options there. Later, we discussed linking and how symbols are resolved. We also talked about possible issues with the linking step. In the next section, we will learn about object files.

## Diving Deeper: Viewing Object Files

For the linking step to work without errors, we need to have all our symbol references match our symbol definitions. Most of the time, we can analyze how things will be resolved just by looking at the source files. Sometimes, in complex situations, we may have a difficult time understanding why a symbol is not being resolved. In such situations, looking at the contents of object files to investigate references and definitions can be useful to resolve the problem. Besides linker errors, understanding object file contents and how linking works in general is useful for a C++ programmer. Knowing what is happening under the hood may help programmers understand the whole process in a better way.

When our source code is compiled to object files, our statements and expressions are converted to assembly code, which is the low-level language that the CPU understands. Each instruction in assembly contains an operation, followed by operators, which are registers of the CPU. There are instructions to load data to and from registers and operate on values in registers. The `objdump` command in Linux helps us view the contents of these object files.

> **Note**
> 
> We will utilize Compiler Explorer, a nice online tool that is easier to use, where you can write code on the window to the left, and on the right, you can see the compiled assembly code. Here is the link to the Compiler Explorer: https://godbolt.org.

While it's not very clear, you can more or less make out what it is doing. We are not going to dive into the details of assembly code, but we will focus on how symbols are resolved during the linker stage. Let's focus on the following lines for now:

```
sum(int, int):
```

```
...
```

```
callSum():
```

```
...
    call  sum(int, int)
```

```
...
```

The **call sum(int, int)** line does what you expect: it calls the preceding **sum** function and places the arguments in some registers. The important point here is that the functions are identified by their names and the types of their parameters in order. The linker looks for the appropriate function with this signature. Note that the return value is not a part of the signature.

- 2. Disable the **Demangle** checkbox and see how these function names are actually stored:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_65_0.png)

# Figure 1.45: Compiled code without demangling

Here, our lines became this:

```
_Z3sumii:

...

_Z7callSumv:

...
    call    _Z3sumii

...
```

Here is the compiled version of these added lines:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_67_0.png)

**Figure 1.46: The compiled version**

A debugger is a better tool to fight runtime errors. A debugger can let you run code line by line, continue running and pause on the lines that you want, investigate the values of memory, and pause on errors, among other things. This lets you watch what exactly is going on with memory as your program is running and identify the line of code that results in the unwanted behavior.

gdb is the canonical command-line debugger that can debug C++ programs. However, it may be difficult to use as debugging is inherently a visual task—you want to be able to look at lines of code, values of variables, and the output of the program at the same time. Luckily, Eclipse CDT includes a visual debugger that is easy to use.

## Exercise 9: Debugging with Eclipse CDT

You have been simply running your projects and viewing the output. Now you want to learn how to debug your code in detail. In this exercise, we will explore Eclipse CDT's debugging capabilities. Perform the following steps to complete the exercise:

- 1. Open the CMake project in Eclipse CDT.
- 2. To ensure that we have an existing run configuration, click Run | Run Configurations. There, you should see a CxxTemplate entry under C/C++ Application.
- 3. Close the dialog box to continue.

> Note
> Since we ran our project before, it should be there. If not, please go back and create it again.
