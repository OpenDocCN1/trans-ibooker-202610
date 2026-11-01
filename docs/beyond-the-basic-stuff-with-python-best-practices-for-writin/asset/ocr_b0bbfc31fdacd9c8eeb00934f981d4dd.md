In Python, all variables are technically references, not containers of values, regardless of their data type. The box metaphor is simple but also flawed. Instead of thinking of variables as boxes, you can think of variables as labels for objects in memory. Figure 7-2 shows labels on the previous spam and eggs examples.

![](bbox=[0.27, 0.24, 0.62, 0.4])

Figure 7-2: Variables can also be thought of as labels on values.

Because multiple variables can refer to the same object, that object can be “stored” in multiple variables. Multiple boxes can’t store the same object, so it might be easier for you to use the label metaphor instead. Ned Batchelder’s PyCon 2015 talk, “Facts and Myths about Python Names and Values” has more information on this topic at https://youtu.be/_AEJHKgK9ns.

Without understanding that the = assignment operator always copies the reference, not the object, you might introduce bugs by thinking that you’re making a duplicate copy of an object when really you’re copying the reference to the original object. Fortunately, this isn’t an issue for immutable values like integers, strings, and tuples for reasons that I’ll explain in “Mutable and Immutable” on page 114.

You can use the is operator to compare whether two objects have the same identity. In contrast, the == operator checks only whether object values are the same. You can consider x is y to be shorthand for id(x) == id(y). Enter the following into the interactive shell to see the difference:

```
>>> spam = {'name': 'Zophie'}
>>> eggs = spam
>>> spam is eggs
True
>>> spam == eggs
True
>>> bacon = {'name': 'Zophie'}
>>> spam == bacon
True
>>> spam is bacon
False
```