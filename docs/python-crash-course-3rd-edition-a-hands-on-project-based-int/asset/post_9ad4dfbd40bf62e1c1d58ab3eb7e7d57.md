
## Python 2 on Older Versions of macOS

On older versions of macOS, before Monterey (macOS 12), an outdated version of Python 2 was installed by default. On these systems, the command `python` points to the outdated system interpreter. If you’re using a version of macOS with Python 2 installed, make sure you use the **python3** command, and you’ll always be using the version of Python you installed.

## Python on Linux

Python is included by default on almost every Linux system. However, if the default version on your system is earlier than Python 3.9, you should install the latest version. You can also install the latest version if you want the most recent features, like Python’s improved error messages. The following instructions should work for most apt-based systems.

## Using the Default Python Installation

If you want to use the version of Python that `python3` points to, make sure you have these three additional packages installed:

```bash
$ sudo apt install python3-dev python3-pip python3-venv
```

These packages include tools that are useful for developers and tools that let you install third-party packages, like the ones used in the projects section of this book.

## Installing the Latest Version of Python

We’ll use a package called `deadsnakes`, which makes it easy to install multiple versions of Python. Enter the following commands:

```bash
$ sudo add-apt-repository ppa:deadsnakes/ppa
$ sudo apt update
$ sudo apt install python3.11
```

These commands will install Python 3.11 onto your system.

Enter the following command to start a terminal session that runs Python 3.11:

```bash
$ python3.11
>>>
```

Anywhere you see the command `python` in this book, use `python3.11` instead. You'll also want to use this command when you run programs from the terminal.

You'll need to install two more packages to make the most of your Python installation:

```bash
$ sudo apt install python3.11-dev python3.11-venv
```

These packages include modules you’ll need when installing and running third-party packages, like the ones used in the projects in the second half of the book.

> **NOTE**
>
> *The deadsnakes package has been actively maintained for a long time. When newer versions of Python come out, you can use these same commands, replacing `python3.11` with the latest version currently available.*

## Checking Which Version of Python You’re Using

If you’re having any issues running Python or installing additional packages, it can be helpful to know exactly which version of Python you’re using. You may have multiple versions of Python installed and not be clear about which version is currently being used.

Issue the following command in a terminal:

```bash
$ python --version
Python 3.11.0
```

This tells you exactly which version the command `python` is currently pointing to. The shorter command `python -V` will give the same output.

## Python Keywords and Built-in Functions

Python comes with its own set of keywords and built-in functions. It’s important to be aware of these when you’re naming things in Python: your names cannot be the same as these keywords and shouldn’t be the same as the function names, or you’ll overwrite the functions.

In this section, we’ll list Python’s keywords and built-in function names, so you’ll know which names to avoid.

### Python Keywords

Each of the following keywords has a specific meaning, and you’ll see an error if you try to use any of them as a variable name.

| False | await | else | import | pass |
|---|---|---|---|---|
| None | break | except | in | raise |
| True | class | finally | is | return |
| and | continue | for | lambda | try |
| as | def | from | nonlocal | while |
| assert | del | global | not | with |
| async | elif | if | or | yield |

## Python Built-in Functions

You won’t get an error if you use one of the following readily available built-in functions as a variable name, but you’ll override the behavior of that function:

| abs() | hash() | slice() |
|---|---|---|
| aiter() | help() | sorted() |
| all() | hex() | staticmethod() |
| any() | id() | str() |
| anext() | input() | sum() |
| ascii() | int() | super() |
| bin() | isinstance() | tuple() |
| bool() | issubclass() | type() |
| breakpoint() | iter() | vars() |
| bytearray() | len() | zip() |
| bytes() | list() | __import__() |
| callable() | locals() | |
| chr() | map() | |
| classmethod() | max() | |
| compile() | memoryview() | |
| complex() | min() | |
| delattr() | next() | |
| dict() | object() | |
| dir() | oct() | |
| divmod() | open() | |
| enumerate() | ord() | |
| eval() | pow() | |
| exec() | print() | |
| filter() | property() | |
| float() | range() | |
| format() | repr() | |
| frozenset() | reversed() | |
| getattr() | round() | |
| globals() | set() | |
| hasattr() | setattr() | |

## B

## TEXT EDITORS AND IDEs

![](img/8f15563c16d2a98c4d3115150679b092_748_0.png)

Programmers spend a lot of time writing, reading, and editing code, and using a text editor or an IDE (integrated development environment) to make this work as efficient as possible is essential. A good editor will do simple tasks, like highlighting your code’s structure so you can catch common bugs as you’re working. But it won’t do so much that it distracts you from your thinking. Editors also have useful features like automatic indenting, markers to show appropriate line length, and keyboard shortcuts for common operations.

An *IDE* is a text editor with a number of other tools included, like interactive debuggers and code introspection. An IDE examines your code as you enter it and tries to learn about the project you’re building. For example, when you start typing the name of a function, an IDE might show you all the arguments that function accepts. This behavior can be very helpful when everything works and you understand what you’re seeing. But it can also be overwhelming as a beginner and difficult to troubleshoot when you aren’t sure why your code isn’t working in the IDE.

These days, the lines have blurred between text editors and IDEs. Most popular editors have some features that used to be exclusive to IDEs. Likewise, most IDEs can be configured to run in a lighter mode that’s less distracting as you work, but lets you use the more advanced features when you need them.

If you already have an editor or IDE installed that you like, and if it’s already configured to work with a recent version of Python that’s installed on your system, then I encourage you to stick with what you already know. Exploring different editors can be fun, but it’s also a way to avoid the work of learning a new language.

If you don’t already have an editor or IDE installed, I recommend VS Code for a number of reasons:

- It’s free, and it’s released under an open source license.
- It can be installed on all major operating systems.
- It’s beginner-friendly but also powerful enough that many professional programmers use it as their main editor.
- It finds the versions of Python you have installed, and it typically does not require any configuration to run your first programs.

It has an integrated terminal, so your output appears in the same window as your code.

A Python extension is available that makes the editor highly efficient for writing and maintaining Python code.

It’s highly customizable, so you can tune it to match the way you work with code.

In this appendix, you’ll learn how to start configuring VS Code so that it works well for you. You’ll also learn some shortcuts that let you work more efficiently. Being a fast typist is not as important as many people think in programming, but understanding your editor and knowing how to use it efficiently is quite helpful.

With all that said, VS Code doesn’t work for everyone. If it doesn’t work well on your system for some reason, or if it’s distracting you as you work, there are a number of other editors that you might find more appealing. This appendix includes a brief description of some of the other editors and IDEs you should consider.

## Working Efficiently with VS Code

In Chapter 1, you installed VS Code and added the Python extension as well. This section will show you some further configurations you can make, plus shortcuts for working efficiently with your code.

## Configuring VS Code

There are a few ways to change the default configuration settings for VS Code. Some changes can be made through the interface, and some will require changes in configuration files. These changes will sometimes take effect for everything you do in VS Code, while others will affect only the files within the folder that contains the configuration file.
