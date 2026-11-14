
# Variable Roles

Its value can, however, also be calculated directly in functional languages or languages that encompass certain functional aspects, i.e. `functional_total = sum(list)`. Gatherers look quite differently based on the programming language in question. In imperative pieces of code, they can only occur in conjunction with a loop, while in functional languages they can be expressed with pattern matching also.

## Container

A container is any data structure that holds multiple elements that can be added and removed.

Examples of containers are lists, arrays, stacks, and trees. In many cases we can recognize containers syntactically, since they are initialized with special symbols, like `customers = []` or `symbols = {}`.

## Follower

Some algorithms require you to keep track of a previous or subsequent value. A variable in this role is called a follower and is always coupled to another variable.

Examples of follower variables are a pointer that points to a previous element in a linked list when traversing the list, or the lower index in a binary search.

## Organizer

Often data has to be processed in some way. For example, in some languages, you cannot access individual characters in a string without converting the string to a character array first. Or you retrieved a collection of elements but you also want to store a sorted version of that list to filter in various ways later on, and sorting once is more effective than sorting each time. These are examples of organizers, which are variables that are only used for rearranging or storing values differently. Often, organizers are temporary variables.

Examples are sorting or filtering variables, for example in pandas:

```
filtered = df[df["variable"] == "A"]
```

Organizers are always connected to other variables whose values they reshape or transform.

## Temporary

Temporary variables are variables that are used only briefly and are often called `temp` or `t`. These variables may be used to swap data or to store the result of a computation that is used multiple times in a method or function.

Examples: `temp`, `foo`, `bar`, `thing`

Temporary variables are often recognized based on their names that might have `temp` at the beginning or the end, but this is not always the case. Another way to recognize temporary variables is to notice that they are only used in a few lines of code that are located closely together.

# Exercise 4.3: Examine Roles of All Variable Names in the Codebase

For this exercise, examine your list of variables in the codebase again and try to determine their roles. Once you have found all roles, you can fill out the final column of the table documenting the programming concepts related to the variables.

| Variable name | Role | Related variables | Programming concept(s) |
| --- | --- | --- | --- |
| `template` in `template = self._parse(template)` | organizers | `template` | parsing |
| `res` in `res.update(matches)` | gatherer | `matches` | Tree templates |

# Understanding Concepts Using Dependencies on Frameworks and Libraries

To fully understand the concepts that play a role in a piece of code, it is also important to pay attention to what concepts are imported from frameworks and libraries. For example, consider these import statements from the Lark code snippet in Python you examined before:

```
from typing import Union, Optional, Mapping, Dict, Tuple, Iterator
from lark import Tree, Transformer
from lark.exceptions import MissingVariableError
```

The first line of code immediately provides some insights into the programming concepts that are being used: container types such as `Tuple` and `Mapping`, and (functional) data structures like `Union` and `Optional`. The import line of code also tells you, if you have some prior knowledge of Python and this library, that you are importing typed versions of built-in types like `Dictionary` and `Tuple` so Python can give type hints.

The second line tells you that `Tree`s are being used and transformed, and the final line tells you that from all exceptions that Lark supports, only the `MissingVariableError` is imported. So if you want to dive deeper into reading this specific code base, it would be really interesting to find out what other exceptions exist and why only this one is needed here.

It can also be very helpful to examine where in the code snippet the imports are used. For example in the Lark code snippet, the imported `Tree` `Transformer` is only used in the `_ReplaceVars` class. This helps you to see that that class is the only transformer in this file, and thus plays a large role in the code snippet.

# Exercise 4.4: Examine Dependencies

For this exercise, examine the dependencies and (optionally) also read these dependencies. What can you learn about the code from the imported concepts? What places in the code use these concepts?

| Imported concept | Imported from | Programming concept(s) | Used in |
| --- | --- | --- | --- |
| `Transformer` | `lark` | (parse)tree transformation | `_ReplaceVars` class |

# After Reading

Like in other chapters, there are a number of things you can do after examining the programming concepts in the code.

## Expand Conceptual Documentation

Some of the information about programming concepts and behaviour can serve as excellent documentation to add to a readme file. While the core maintainers of the project will probably know most of the programming concepts intimately, explaining basics in the project documentation can be a very valuable addition for beginners in a project.

## Document Roles of Variables

Another contribution you could make after examining the roles of variables in the code base is to add that information to the code base. The table you made in Exercise 4.3 can be a great start for that. Depending on the style of the code base, roles of variables could be encoded into variable names, added as comments or documented outside of the code base. As a concrete first step, clearly marking all temporary variables (e.g. with a `temp_` prefix) can make it a lot easier to perform refactorings in the future.

## Expand Dependency Documentation

Exercise 4.4 has helped us to understand the dependencies in the code base …more to come.

# Summary

Code can be read in different dimensions: structure, domain, concepts, context, and collaboration. Like Chapter 4, this chapter looked at how variables, functions and objects are named, but from the perspective of programming concepts. In addition to names, operations on code, both as user defined functions like `.parse()` and in syntax, such as `[]` can also help us understand the programming concepts present in a code base.

The next chapter will explore how to understand the context of a code snippet in more depth, including comments and documentation.

^[1] https://www.ppig.org/files/2005-PPIG-17th-sajaniemi.pdf

# Chapter 5: Context Examination

Chapters 5-X dive into five different dimensions of code reading: structure, domain, concepts, context, and collaboration.

In the previous chapters, you examined code from three different perspectives: structure, domain and programming concepts. Hopefully by now you will feel more confident when reading unfamiliar code. However, in practice you will probably won't read small code snippets only. Snippets are part of a code base and as such have a lot of *context*, which is what we will examine in more depth in this chapter.

# Running Example

As in previous chapters, we will use one example code snippet in this chapter. In this chapter, you are learning how to read documents that surround the actual code itself.
