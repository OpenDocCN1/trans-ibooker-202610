
## Method Resolution Order

Our tic-tac-toe program now has four classes to represent boards, three with defined getBoardStr() methods and one with an inherited getBoardStr() method, as shown in Figure 16-2.

![](img/ab52fe24e511e29be73786864ebe0410_337_0.png)

Figure 16-2: The four classes in our tic-tac-toe board program

When we call getBoardStr() on a HybridBoard object, Python knows that the HybridBoard class doesn't have a method with this name, so it checks its parent class. But the class has two parent classes, both of which have a getBoardStr() method. Which one gets called?

You can find out by checking the HybridBoard class's MRO, which is the ordered list of classes that Python checks when inheriting methods or when a method calls the super() function. You can see the HybridBoard class's MRO by calling its mro() method in the interactive shell:

```
>>> from tictactoe_oop import *
>>> HybridBoard.mro()
[<class 'tictactoe_oop.HybridBoard'>, <class 'tictactoe_oop.HintBoard'>,
<class 'tictactoe_oop.MiniBoard'>, <class 'tictactoe_oop.TTTBoard'>, <class 'object'>]
```

From this return value, you can see that when a method is called on HybridBoard, Python first checks for it in the HybridBoard class. If it's not there, Python checks the HintBoard class, then the MiniBoard class, and finally the TTTBoard class. At the end of every MRO list is the built-in object class, which is the parent class of all classes in Python.

For single inheritance, determining the MRO is easy: just make a chain of parent classes. For multiple inheritance, it's trickier. Python's MRO follows the C3 algorithm, whose details are beyond the scope of this book. But you can determine the MRO by remembering two rules:

- Python checks child classes before parent classes.
- Python checks inherited classes listed left to right in the class statement.

If we call getBoardStr() on a HybridBoard object, Python checks the HybridBoard class first. Then, because the class’s parents from left to right are HintBoard and MiniBoard, Python checks HintBoard. This parent class has a getBoardStr() method, so HybridBoard inherits and calls it.

But it doesn’t end there: next, this method calls super().getBoardStr(). Super is a somewhat misleading name for Python’s super() function, because it doesn’t return the parent class but rather the next class in the MRO. This means that when we call getBoardStr() on a HybridBoard object, the next class in its MRO, after HintBoard, is MiniBoard, not the parent class TTTBoard. So the call to super().getBoardStr() calls the MiniBoard class’s getBoardStr() method, which returns the miniature tic-tac-toe board string. The remaining code in the HintBoard class’s getBoardStr() after this super() call appends the hint text to this string.

If we change the HybridBoard class’s class statement so it lists MiniBoard first and HintBoard second, its MRO will put MiniBoard before HintBoard. This means HybridBoard inherits getBoardStr() from MiniBoard, which doesn’t have a call to super(). This ordering is what caused the bug that made the miniature tic-tac-toe board display without hints: without a super() call, the MiniBoard class’s getBoardStr() method never calls the HintBoard class’s getBoardStr() method.

Multiple inheritance allows you to create a lot of functionality in a small amount of code but easily leads to overengineered, hard-to-understand code. Favor single inheritance, mixin classes, or no inheritance. These techniques are often more than capable of carrying out your program’s tasks.

## Summary

Inheritance is a technique for code reuse. It lets you create child classes that inherit the methods of their parent classes. You can override the methods to provide new code for them but also use the super() function to call the original methods in the parent class. A child class has an “is a” relationship with its parent class, because an object of the child class is a kind of object of the parent class.

In Python, using classes and inheritance is optional. Some programmers see the complexity that heavy use of inheritance creates as not worth its benefits. It’s often more flexible to use composition instead of inheritance, because it implements a “has a” relationship with an object of one class and objects of other classes rather than inheriting methods directly from those other classes. This means that objects of one class can have an object of another class. For example, a Customer object could have a birthdate method that is assigned a Date object rather than the Customer class subclassing Date.

Just as type() can return the type of the object passed to it, the isinstance() and issubclass() functions return type and inheritance information about the object passed to them.

Classes can have object methods and attributes, but they can also have class methods, class attributes, and static methods. Although these are rarely used, they can enable other object-oriented techniques that global variables and functions can't provide.

Python lets classes inherit from multiple parents, although this can lead to code that is difficult to understand. The super() function and a class's methods figure out how to inherit methods based on the MRO. You can view a class's MRO in the interactive shell by calling the mro() method on the class.

This chapter and the previous one covered general OOP concepts. In the next chapter, we'll explore Python-specific OOP techniques.

## 17

## PYTHONIC OOP: PROPERTIES AND DUNDER METHODS

![](img/ab52fe24e511e29be73786864ebe0410_341_0.png)

Many languages have OOP features, but Python has some unique OOP features, including properties and dunder methods. Learning how to use these Pythonic techniques can help you write concise and readable code.

Properties allow you to run some specific code each time an object's attribute is read, modified, or deleted to ensure the object isn't put into an invalid state. In other languages, this code is often called *getters* or *setters*. Dunder methods allow you to use your objects with Python's built-in operators, such as the + operator. This is how you can combine two `datetime.timedelta` objects, such as `datetime.timedelta(days=2)` and `datetime.timedelta(days=3)`, to create a new `datetime.timedelta(days=5)` object.

In addition to using other examples, we'll continue to expand the `WizCoin` class we started in Chapter 15 by adding properties and overloading operators with dunder methods. These features will make `WizCoin` objects more expressive and easier to use in any application that imports the `wizcoin` module.

### Properties

The BankAccount class that we used in Chapter 15 marked its _balance attribute as private by placing an underscore at the start of its name. But remember that designating an attribute as private is only a convention: all attributes in Python are technically public, meaning they're accessible to code outside the class. There's nothing to prevent code from intentionally or maliciously changing the _balance attribute to an invalid value.

But you can prevent accidental invalid changes to these private attributes with properties. In Python, properties are attributes that have specially assigned getter, setter, and deleter methods that can regulate how the attribute is read, changed, and deleted. For example, if the attribute is only supposed to have integer values, setting it to the string '42' will likely cause bugs. A property would call the setter method to run code that fixes, or at least provides early detection of, setting an invalid value. If you've thought, “I wish I could run some code each time this attribute was accessed, modified with an assignment statement, or deleted with a del statement,” then you want to use properties.

### Turning an Attribute into a Property

First, let's create a simple class that has a regular attribute instead of a property. Open a new file editor window and enter the following code, saving it as regularAttributeExample.py:

```
class ClassWithRegularAttributes:
    def __init__(self, someParameter):
        self.someAttribute = someParameter

obj = ClassWithRegularAttributes('some initial value')
print(obj.someAttribute)  # Prints 'some initial value'
obj.someAttribute = 'changed value'
print(obj.someAttribute)  # Prints 'changed value'
del obj.someAttribute  # Deletes the someAttribute attribute.
```

This ClassWithRegularAttributes class has a regular attribute named someAttribute. The __init__() method sets someAttribute to 'some initial value', but we then directly change the attribute's value to 'changed value'. When you run this program, the output looks like this:

```
some initial value
changed value
```

This output indicates that code can easily change someAttribute to any value. The downside of using regular attributes is that your code can set the someAttribute attribute to invalid values. This flexibility is simple and convenient, but it also means someAttribute could be set to some invalid value that causes bugs.
