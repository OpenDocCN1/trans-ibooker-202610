You’ll find the full list of these types online at https://docs.python.org/3/library/typing.html#classes-functions-and-decorators.

# Backporting Type Hints with Comments

Backporting is the process of taking features from a new version of software and porting (that is, adapting and adding) them to an earlier version. Python’s type hints feature is new to version 3.5. But in Python code that might be run by interpreter versions earlier than 3.5, you can still use type hints by putting the type information in comments. For variables, use an inline comment after the assignment statement. For functions and methods, write the type hint on the line following the def statement. Begin the comment with type:, followed by the data type. Here’s an example of some code with type hints in the comments:

```
from typing import List

spam = 42  # type: int
def sayHello():
    # type: () -> None
    """The docstring comes after the type hint comment."""
    print('Hello!')

def addTwoNumbers(listOfNumbers, doubleTheSum):
    # type: (List[float], bool) -> float
    total = listOfNumbers[0] + listOfNumbers[1]
    if doubleTheSum:
        total *= 2
    return total
```

Note that even if you’re using the comment type hint style, you still need to import the typing module ①, as well as any type aliases that you use in the comments. Versions earlier than 3.5 won’t have a typing module in their standard library, so you must install typing separately by running this command:

```
python -m pip install --user typing
```

Run python3 instead of python on macOS and Linux.

To set the spam variable to an integer, we add # type: int as the end-of-line comment ②. For functions, the comment should include parentheses with a comma-separated list of type hints in the same order as the parameters. Functions with zero parameters would have an empty set of parentheses ③. If there are multiple parameters, separate them inside the parentheses with commas ④.

The comment type hint style is a bit less readable than the normal style, so use it only for code that might be run by versions of Python earlier than 3.5.