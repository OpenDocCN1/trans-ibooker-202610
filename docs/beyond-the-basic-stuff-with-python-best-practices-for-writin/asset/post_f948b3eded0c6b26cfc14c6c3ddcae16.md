
functions, but int, str, float, and bool are actually classes, and the int(), str(), float(), and bool() functions are constructor functions that return new integer, string, float, and Boolean objects. Python's style guide recommends using capitalized camelcase for your class names (like WizCoin), although many of Python's built-in classes don't follow this convention.

Note that calling the WizCoin() construction function returns the new WizCoin object, but the __init__() method never has a return statement with a return value. Adding a return value causes this error: TypeError: __init__() should return None.

## Attributes

Attributes are variables associated with an object. The Python documentation describes attributes as “any name following a dot.” For example, consider the birthday.year expression in the previous section. The year attribute is a name following a dot.

Every object has its own set of attributes. When the wcexample1.py program created two WizCoin objects and stored them in the purse and coinJar variables, their attributes had different values. You can access and set these attributes just like any variable. To practice setting attributes, open a new file editor window and enter the following code, saving it as wcexample2.py in the same folder as the wizcoin.py file:

```
import wizcoin

change = wizcoin.WizCoin(9, 7, 20)
print(change.sickles) # Prints 7.
change.sickles += 10
print(change.sickles) # Prints 17.

pile = wizcoin.WizCoin(2, 3, 31)
print(pile.sickles) # Prints 3.
pile.someNewAttribute = 'a new attr' # A new attribute is created.
print(pile.someNewAttribute)
```

When you run this program, the output looks like this:

```
7
17
3
a new attr
```

You can think of an object's attributes as similar to a dictionary's keys. You can read and modify their associated values and assign an object new attributes. Technically, methods are considered attributes of a class, as well.

## Private Attributes and Private Methods

In languages such as C++ or Java, attributes can be marked as having private access, which means the compiler or interpreter only lets code inside the class's methods access or modify the attributes of objects of that class. But
in Python, this enforcement doesn't exist. All attributes and methods are effectively *public access*: code outside of the class can access and modify any attribute in any object of that class.

But private access is useful. For example, objects of a BankAccount class could have a balance attribute that only methods of the BankAccount class should have access to. For those reasons, Python's convention is to start *private* attribute or method names with a single underscore. Technically, there is nothing to stop code outside the class from accessing private attributes and methods, but it's a best practice to let only the class's methods access them.

Open a new file editor window, enter the following code, and save it as *privateExample.py*. In it, objects of a BankAccount class have private _name and _balance attributes that only the deposit() and withdraw() methods should directly access:

```python
class BankAccount:
    def __init__(self, accountHolder):
        # BankAccount methods can access self._balance, but code outside of
        # this class should not:
        self._balance = 0
        self._name = accountHolder
        with open(self._name + 'Ledger.txt', 'w') as ledgerFile:
            ledgerFile.write('Balance is 0\n')

    def deposit(self, amount):
        if amount <= 0:
            return # Don't allow negative "deposits".
        self._balance += amount
        with open(self._name + 'Ledger.txt', 'a') as ledgerFile:
            ledgerFile.write('Deposit ' + str(amount) + '\n')
            ledgerFile.write('Balance is ' + str(self._balance) + '\n')

    def withdraw(self, amount):
        if self._balance < amount or amount < 0:
            return # Not enough in account, or withdraw is negative.
        self._balance -= amount
        with open(self._name + 'Ledger.txt', 'a') as ledgerFile:
            ledgerFile.write('Withdraw ' + str(amount) + '\n')
            ledgerFile.write('Balance is ' + str(self._balance) + '\n')

acct = BankAccount('Alice') # We create an account for Alice.
acct.deposit(120) # _balance can be affected through deposit()
acct.withdraw(40) # _balance can be affected through withdraw()

# Changing _name or _balance outside of BankAccount is impolite, but allowed:
acct._balance = 1000000000
acct.withdraw(1000)

acct._name = 'Bob' # Now we're modifying Bob's account ledger!
acct.withdraw(1000) # This withdrawal is recorded in BobLedger.txt!
```

When you run *privateExample.py*, the ledger files it creates are inaccurate because we modified the _balance and _name outside the class, which resulted in invalid states. *AliceLedger.txt* inexplicably has a lot of money in it:

```
Balance is 0
Deposit 120
Balance is 120
Withdraw 40
Balance is 80
Withdraw 1000
Balance is 999999000
```

Now there’s a *BobLedger.txt* file with an inexplicable account balance, even though we never created a BankAccount object for Bob:

```
Withdraw 1000
Balance is 999998000
```

Well-designed classes will be mostly self-contained, providing methods to adjust the attributes to valid values. The _balance and _name attributes are marked as private ① ②, and the only valid way of adjusting the BankAccount class’s value is through the deposit() and withdraw() methods. These two methods have checks ③ ⑤ to make sure _balance isn’t put into an invalid state (such as a negative integer value). These methods also record each transaction to account for the current balance ④ ⑥.

Code outside the class that modifies these attributes, such as acct._balance = 1000000000 ⑦ or acct._name = 'Bob' ⑧ instructions, can put the object into an invalid state and introduce bugs (and audits from the bank examiner). By following the underscore prefix convention for private access, you make debugging easier. The reason is that you know the cause of the bug will be in the code in the class instead of anywhere in the entire program.

Note that unlike Java and other languages, Python has no need for public getter and setter methods for private attributes. Instead Python uses properties, as explained in Chapter 17.

## The type() Function and __qualname__ Attribute

Passing an object to the built-in type() function tells us the object’s data type through its return value. The objects returned from the type() function are type objects, also called class objects. Recall that the terms type, data type, and class all have the same meaning in Python. To see what the type() function returns for various values, enter the following into the interactive shell:

```
>>> type(42)  # The object 42 has a type of int.
<class 'int'>
>>> int  # int is a type object for the integer data type.
<class 'int'>
>>> type(42) == int  # Type check 42 to see if it is an integer.
True
>>> type('Hello') == int  # Type check 'Hello' against int.
False
>>> import wizcoin
>>> type(42) == wizcoin.WizCoin  # Type check 42 against WizCoin.
False
>>> purse = wizcoin.WizCoin(2, 5, 10)
>>> type(purse) == wizcoin.WizCoin # Type check purse against WizCoin.
True
```

Note that int is a type object and is the same kind of object as what type(42) returns, but it can also be called as the int() constructor function: the int('42') function doesn't convert the '42' string argument; instead, it returns an integer object based on the argument.

Say you need to log some information about the variables in your program to help you debug them later. You can only write strings to a logfile, but passing the type object to str() will return a rather messy-looking string. Instead, use the __qualname__ attribute, which all type objects have, to write a simpler, human-readable string:

```
>>> str(type(42))  # Passing the type object to str() returns a messy string.
"<class 'int'>"
>>> type(42).__qualname__ # The __qualname__ attribute is nicer looking.
'int'
```

The __qualname__ attribute is most often used for overriding the __repr__() method, which is explained in more detail in Chapter 17.

## Non-OOP vs. OOP Examples: Tic-Tac-Toe

At first, it can be difficult to see how to use classes in your programs. Let's look at an example of a short tic-tac-toe program that doesn't use classes, and then rewrite it so it does.

Open a new file editor window and enter the following program; then save it as tictactoe.py:

```
# tictactoe.py, A non-OOP tic-tac-toe game.

ALL_SPACES = list('123456789')  # The keys for a TTT board dictionary.
X, O, BLANK = 'X', 'O', ' '  # Constants for string values.

def main():
    """Runs a game of tic-tac-toe."""
    print('Welcome to tic-tac-toe!')
    gameBoard = getBlankBoard()  # Create a TTT board dictionary.
    currentPlayer, nextPlayer = X, O  # X goes first, O goes next.

    while True:
        print(getBoardStr(gameBoard))  # Display the board on the screen.

        # Keep asking the player until they enter a number 1-9:
```
