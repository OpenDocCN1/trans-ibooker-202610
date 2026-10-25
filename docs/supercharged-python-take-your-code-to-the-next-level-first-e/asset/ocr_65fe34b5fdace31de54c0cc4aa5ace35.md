# # 14. Multiple Modules and the RPN Example

If you program in Python for any length of time, you’ll reach a point where you want to place your code in more than one source file— that is, a _module_. There are many advantages to using multiple modules in a single project. For example, multiple developers can work on different modules in tandem, even though they’re part of the same project. Think of trying to create something as complex as Microsoft Windows without modular development!

Python modules are easy to work with, for the most part, but there are some gotchas that can crop up and bite you, like a snake. But not to worry. We’ll safely steer you around those gotchas.

This chapter also completes the Reverse Polish Notation example this book has been developing in Chapters 3, 6, 7, and 8. This application will interpret an RPN language powerful enough to support loops and decision making.

# ## 14.1 OVERVIEW OF MODULES IN PYTHON

In Python, modules are objects just as other things are, and each