
the path that’s passed to `get_stored_username()` doesn’t exist, the function returns `None` ❷. This is good practice: a function should either return the value you’re expecting, or it should return `None`. This allows us to perform a simple test with the return value of the function. We print a welcome back message to the user if the attempt to retrieve a username is successful ❸, and if it isn’t, we prompt for a new username.

We should factor one more block of code out of `greet_user()`. If the username doesn’t exist, we should move the code that prompts for a new username to a function dedicated to that purpose:

```
```python
from pathlib import Path
import json

def get_stored_username(path):
    """Get stored username if available."""
    --snip--

def get_new_username(path):
    """Prompt for a new username."""
    username = input("What is your name? ")
    contents = json.dumps(username)
    path.write_text(contents)
    return username

def greet_user():
    """Greet the user by name."""
    path = Path('username.json')
    username = get_stored_username(path)
    if username:
        print(f"Welcome back, {username}!")
    else:
        username = get_new_username(path)
        print(f"We'll remember you when you come back, {username}!")

greet_user()
```
```

Each function in this final version of remember_me.py has a single, clear purpose. We call greet_user(), and that function prints an appropriate message: it either welcomes back an existing user or greets a new user. It does this by calling get_stored_username(), which is responsible only for retrieving a stored username if one exists. Finally, if necessary, greet_user() calls get_new_username(), which is responsible only for getting a new username and storing it. This compartmentalization of work is an essential part of writing clear code that will be easy to maintain and extend.

## TRY IT YOURSELF

- **10-11. Favorite Number:** Write a program that prompts for the user’s favorite number. Use json.dumps() to store this number in a file. Write a separate program that reads in this value and prints the message “I know your favorite number! It’s _____.”

- **10-12. Favorite Number Remembered:** Combine the two programs you wrote in Exercise 10-11 into one file. If the number is already stored, report the favorite number to the user. If not, prompt for the user’s favorite number and store it in a file. Run the program twice to see that it works.

- **10-13. User Dictionary:** The remember_me.py example only stores one piece of information, the username. Expand this example by asking for two more pieces of information about the user, then store all the information you collect in a dictionary. Write this dictionary to a file using json.dumps(), and read it back in using json.loads(). Print a summary showing exactly what your program remembers about the user.

- **10-14. Verify User:** The final listing for remember_me.py assumes either that the user has already entered their username or that the program is running for the first time. We should modify it in case the current user is not the person who last used the program.

Before printing a welcome back message in greet_user(), ask the user if this is the correct username. If it’s not, call get_new_username() to get the correct username.

## Summary

In this chapter, you learned how to work with files. You learned to read the entire contents of a file, and then work through the contents one line at a time if you need to. You learned to write as much text as you want to a file. You also read about exceptions and how to handle the exceptions you're likely to see in your programs. Finally, you learned how to store Python data structures so you can save information your users provide, preventing them from having to start over each time they run a program.

In Chapter 11, you'll learn efficient ways to test your code. This will help you trust that the code you develop is correct, and it will help you identify bugs that are introduced as you continue to build on the programs you've written.

*[OceanofPDF.com](http://OceanofPDF.com)*

# 11
## TESTING YOUR CODE

![](img/8f15563c16d2a98c4d3115150679b092_352_0.png)

When you write a function or a class, you can also write tests for that code. Testing proves that your code works as it’s supposed to in response to all the kinds of input it’s designed to receive. When you write tests, you can be confident that your code will work correctly as more people begin to use your programs. You’ll also be able to test new code as you add it, to make sure your changes don’t break your program’s existing behavior. Every programmer makes mistakes, so every programmer must test their code often, to catch problems before users encounter them.

In this chapter, you’ll learn to test your code using `pytest`. The `pytest` library is a collection of tools that will help you write your first tests quickly and simply, while supporting your tests as they grow in complexity along with your projects. Python doesn’t include `pytest` by default, so you’ll learn to install external libraries. Knowing how to install external libraries will make a wide variety of well-designed code available to you. These libraries will expand the kinds of projects you can work on immensely.

You’ll learn to build a series of tests and check that each set of inputs results in the output you want. You’ll see what a passing test looks like and what a failing test looks like, and you’ll learn how a failing test can help you improve your code. You’ll learn to test functions and classes, and you’ll start to understand how many tests to write for a project.

## Installing pytest with pip

While Python includes a lot of functionality in the standard library, Python developers also depend heavily on third-party packages. A *third-party package* is a library that’s developed outside the core Python language. Some popular third-party libraries are eventually adopted into the standard library, and end up being included in most Python installations from that point forward. This happens most often with libraries that are unlikely to change much once they’ve had their initial bugs worked out. These kinds of libraries can evolve at the same pace as the overall language.

Many packages, however, are kept out of the standard library so they can be developed on a timeline independent of the language itself. These packages tend to be updated more frequently than they would be if they were tied to Python’s development schedule. This is true of `pytest` and most of the libraries we’ll use in the second half of this book. You shouldn’t blindly trust every third-party package, but you also shouldn’t be put off by the fact that a lot of important functionality is implemented through such packages.

## Updating pip

Python includes a tool called pip that’s used to install third-party packages. Because pip helps install packages from external resources, it’s updated often to address potential security issues. So, we’ll start by updating pip.

Open a new terminal window and issue the following command:

```
$ python -m pip install --upgrade pip
① Requirement already satisfied: pip in /.../python3.11/site-packages (22.0.4)
--snip--
② Successfully installed pip-22.1.2
```

The first part of this command, python -m pip, tells Python to run the module pip. The second part, install --upgrade, tells pip to update a package that’s already been installed. The last part, pip, specifies which third-party package should be updated. The output shows that my current version of pip, version 22.0.4 ①, was replaced by the latest version at the time of this writing, 22.1.2 ②.

You can use this command to update any third-party package installed on your system:

```
$ python -m pip install --upgrade package_name
```

> **NOTE**
> 
> *If you’re using Linux, pip may not be included with your installation of Python. If you get an error when trying to upgrade pip, see the instructions in Appendix A.*

### Installing pytest

Now that pip is up to date, we can install pytest:

```
```bash
$ python -m pip install --user pytest
Collecting pytest
  --snip--
Successfully installed attrs-21.4.0 iniconfig-1.1.1 ...pytest-7.x.x
```
```

We’re still using the core command `pip install`, without the `--upgrade` flag this time. Instead, we’re using the `--user` flag, which tells Python to install this package for the current user only. The output shows that the latest version of pytest was successfully installed, along with a number of other packages that pytest depends on.

You can use this command to install many third-party packages:

```
```bash
$ python -m pip install --user package_name
```
```

> **NOTE**
> 
> *If you have any difficulty running this command, try running the same command without the `--user` flag.*

## Testing a Function
