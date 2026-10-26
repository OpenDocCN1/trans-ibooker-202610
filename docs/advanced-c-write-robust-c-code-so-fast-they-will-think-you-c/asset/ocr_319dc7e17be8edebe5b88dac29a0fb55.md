This took us through the two stages of the build process: **compilation** and **linking**. Notice that we used rather simpler commands compared to that of CMake, when we compiled our sources manually. Feel free to type `man g++` to see all the options there. Later, we discussed linking and how symbols are resolved. We also talked about possible issues with the linking step. In the next section, we will learn about object files.

# Diving Deeper: Viewing Object Files

For the linking step to work without errors, we need to have all our symbol references match our symbol definitions. Most of the time, we can analyze how things will be resolved just by looking at the source files. Sometimes, in complex situations, we may have a difficult time understanding why a symbol is not being resolved. In such situations, looking at the contents of object files to investigate references and definitions can be useful to resolve the problem. Besides linker errors, understanding object file contents and how linking works in general is useful for a C++ programmer. Knowing what is happening under the hood may help programmers understand the whole process in a better way.

When our source code is compiled to object files, our statements and expressions are converted to assembly code, which is the low-level language that the CPU understands. Each instruction in assembly contains an operation, followed by operators, which are registers of the CPU. There are instructions to load data to and from registers and operate on values in registers. The `objdump` command in Linux helps us view the contents of these object files.

> **Note**
> 
> We will utilize Compiler Explorer, a nice online tool that is easier to use, where you can write code on the window to the left, and on the right, you can see the compiled assembly code. Here is the link to the Compiler Explorer: https://godbolt.org.