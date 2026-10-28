or

```
from guitar import Guitar, Pickup, ElectricGuitar
```

The **guitar** module itself imports from two other modules.

```
from abc import ABC, abstractmethod
from enum import Enum
```

Put the `import` statements at the top of the module file after the documentation. List the standard modules first, then the third-party modules, followed by your own modules. Sort the `import` statements by module name within each group.

You should start a module with a one-line summary docstring, though I recommend you include an extended summary after that. Also, consider including **Notes**, **References**, and **Examples** sections.

# 7.18 Summary

In this chapter, we learned how to create new kinds of objects with Python `class` definitions. A class brings together data in the form of instance and class variables with functions that operate on them. These functions can be instance methods operating on objects created by the class or class and static methods that work across all instances.

Class inheritance is a crucial feature of Python’s support for object-oriented programming. Inheritance gives us the ability to reuse code within a logical hierarchy of objects while specializing those objects with their own data and operations.

Commenting and documenting Python code is a critical activity so that other developers can understand and use what you created. It also allows you to go back and fix or improve the code when you may not remember how it works.

We finish the first part of this book in the next chapter, where we learn how to read and write information to and from files.