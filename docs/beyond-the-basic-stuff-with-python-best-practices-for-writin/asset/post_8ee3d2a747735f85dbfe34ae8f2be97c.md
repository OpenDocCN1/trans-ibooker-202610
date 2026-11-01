
## 10 WRITING EFFECTIVE FUNCTIONS

![](img/ab52fe24e511e29be73786864ebe0410_187_0.png)

Functions are like mini programs within our programs that allow us to break code into smaller units. This spares us from having to write duplicate code, which can introduce bugs. But writing effective functions requires making many decisions about naming, size, parameters, and complexity.

This chapter explores the different ways we can write functions and the benefits and drawbacks of different trade-offs. We'll delve into how to trade off between small and large functions, how the number of parameters affects the function's complexity, and how to write functions with variable numbers of arguments using the * and ** operators. We'll also explore the functional programming paradigm and the benefits of writing functions according to this paradigm.

### Function Names

Function names should follow the same convention we use for identifiers in general, as described in Chapter 4. But they should usually include a verb, because functions typically perform some action. You might also include a noun to describe the thing being acted on. For example, the names refreshConnection(), setPassword(), and extract_version() clarify what the function does and to what.

You might not need a noun for methods that are part of a class or module. A reset() method in a SatelliteConnection class or an open() function in the webbrowser module already provides the necessary context. You can tell that a satellite connection is the item being reset and that a web browser is the item being opened.

It’s better to use long, descriptive names rather than an acronym or a name that’s too short. A mathematician might immediately understand that a function named gcd() returns the greatest common divisor of two numbers, but everyone else would find getGreatestCommonDenominator() more informative.

Remember not to use any of Python’s built-in function or module names, such as all, any, date, email, file, format, hash, id, input, list, min, max, object, open, random, set, str, sum, test, and type.

### Function Size Trade-Offs

Some programmers say that functions should be as short as possible and no longer than what can fit on a single screen. A function that is only a dozen lines long is relatively easy to understand, at least compared to one that is hundreds of lines long. But making functions shorter by splitting up their code into multiple smaller functions can also have its downsides. Let’s look at some of the advantages of small functions:

- The function’s code is easier to understand.
- The function likely requires fewer parameters.
- The function is less likely to have side effects, as described in “Functional Programming” on page 172.
- The function is easier to test and debug.
- The function likely raises fewer different kinds of exceptions.

But there are also some disadvantages to short functions:

- Writing short functions often means a larger number of functions in the program.
- Having more functions means the program is more complicated.
- Having more functions also means having to come up with additional descriptive, accurate names, which is a difficult task.
- Using more functions requires you to write more documentation.
- The relationships between functions become more complicated.

Some people take the guideline “the shorter, the better” to an extreme and claim that all functions should be three or four lines of code at most. This is madness. For example, here’s the getPlayerMove() function from Chapter 14’s Tower of Hanoi game. The specifics of how this code works are unimportant. Just look at the function’s general structure:

```
def getPlayerMove(towers):
    """Asks the player for a move. Returns (fromTower, toTower)."""

    while True:  # Keep asking player until they enter a valid move.
        print('Enter the letters of "from" and "to" towers, or QUIT.')
        print("(e.g. AB to move a disk from tower A to tower B.)")
        print()
        response = input("> ").upper().strip()

        if response == "QUIT":
            print("Thanks for playing!")
            sys.exit()

    # Make sure the user entered valid tower letters:
    if response not in ("AB", "AC", "BA", "BC", "CA", "CB"):
        print("Enter one of AB, AC, BA, BC, CA, or CB.")
        continue  # Ask player again for their move.

    # Use more descriptive variable names:
    fromTower, toTower = response[0], response[1]

    if len(towers[fromTower]) == 0:
        # The "from" tower cannot be an empty tower:
        print("You selected a tower with no disks.")
        continue  # Ask player again for their move.
    elif len(towers[toTower]) == 0:
        # Any disk can be moved onto an empty "to" tower:
        return fromTower, toTower
    elif towers[toTower][-1] < towers[fromTower][-1]:
        print("Can't put larger disks on top of smaller ones.")
        continue  # Ask player again for their move.
    else:
        # This is a valid move, so return the selected towers:
        return fromTower, toTower
```

This function is 34 lines long. Although it covers multiple tasks, including allowing the player to enter a move, checking whether this move is valid, and asking the player again to enter a move if the move is invalid, these tasks all fall under the umbrella of getting the player’s move. On the other hand, if we were devoted to writing short functions, we could break the code in getPlayerMove() into smaller functions, like this:

```python
def getPlayerMove(towers):
    """Asks the player for a move. Returns (fromTower, toTower)."""

    while True:  # Keep asking player until they enter a valid move.
        response = askForPlayerMove()
        terminateIfResponseIsQuit(response)
        if not isValidTowerLetters(response):
            continue # Ask player again for their move.

        # Use more descriptive variable names:
        fromTower, toTower = response[0], response[1]

        if towerWithNoDisksSelected(towers, fromTower):
            continue # Ask player again for their move.
        elif len(towers[toTower]) == 0:
            # Any disk can be moved onto an empty "to" tower:
            return fromTower, toTower
        elif largerDiskIsOnSmallerDisk(towers, fromTower, toTower):
            continue # Ask player again for their move.
        else:
            # This is a valid move, so return the selected towers:
            return fromTower, toTower

def askForPlayerMove():
    """Prompt the player, and return which towers they select."""
    print('Enter the letters of "from" and "to" towers, or QUIT.')
    print("(e.g. AB to move a disk from tower A to tower B.)")
    print()
    return input("> ").upper().strip()

def terminateIfResponseIsQuit(response):
    """Terminate the program if response is 'QUIT'"""
    if response == "QUIT":
        print("Thanks for playing!")
        sys.exit()

def isValidTowerLetters(towerLetters):
    """Return True if `towerLetters` is valid."""
    if towerLetters not in ("AB", "AC", "BA", "BC", "CA", "CB"):
        print("Enter one of AB, AC, BA, BC, CA, or CB.")
        return False
    return True

def towerWithNoDisksSelected(towers, selectedTower):
    """Return True if `selectedTower` has no disks."""
    if len(towers[selectedTower]) == 0:
        print("You selected a tower with no disks.")
        return True
    return False

def largerDiskIsOnSmallerDisk(towers, fromTower, toTower):
    """Return True if a larger disk would move on a smaller disk."""
    if towers[toTower][-1] < towers[fromTower][-1]:
        print("Can't put larger disks on top of smaller ones.")
        return True
    return False
```

These six functions are 56 lines long, nearly double the line count of the original code, but they do the same tasks. Although each function is easier to understand than the original getPlayerMove() function, the group of them together represents an increase in complexity. Readers of your code might have trouble understanding how they all fit together. The getPlayerMove() function is the only one called by other parts of the program; the other five functions are called only once, from getPlayerMove(). But the mass of functions doesn't convey this fact.

I also had to come up with new names and docstrings (the triple-quoted strings under each def statement, further explained in Chapter 11) for each new function. This leads to functions with confusingly similar names, such as getPlayerMove() and askForPlayerMove(). Also, getPlayerMove() is still longer than three or four lines, so if I were following the guideline “the shorter, the better,” I’d need to split it into even smaller functions!

In this case, the policy of allowing only incredibly short functions might have resulted in simpler functions, but the overall complexity of the program increased drastically. In my opinion, functions should be fewer than 30 lines ideally and definitely no longer than 200 lines. Make your functions as short as reasonably possible but not any shorter.

### Function Parameters and Arguments

A function’s parameters are the variable names between the parentheses of the function’s def statement, whereas the arguments are the values between a function call’s parentheses. The more parameters a function has, the more configurable and generalized its code can be. But more parameters also mean greater complexity.

A good rule to adhere to is that zero to three parameters is fine, but more than five or six is probably too many. Once functions become overly complicated, it’s best to consider how to split them into smaller functions with fewer parameters.

### Default Arguments

One way to reduce the complexity of your function’s parameters is by providing default arguments for your parameters. A default argument is a value used as an argument if the function call doesn’t specify one. If the majority of function calls use a particular parameter value, we can make that value a default argument to avoid having to enter it repeatedly in the function call.

We specify a default argument in the def statement, following the parameter name and an equal sign. For example, in this introduction() function, a parameter named greeting has the value 'Hello' if the function call doesn’t specify it:

```
>>> def introduction(name, greeting='Hello'):
...     print(greeting + ', ' + name)
...
>>> introduction('Alice')
```
