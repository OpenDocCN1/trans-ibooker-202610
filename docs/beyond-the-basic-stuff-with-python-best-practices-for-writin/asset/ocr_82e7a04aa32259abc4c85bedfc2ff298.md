```
>>> id(bacon)
33870056
③ >>> bacon[0] = 'J'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

Strings are immutable, so you cannot change their value. Although it looks like the string's value in bacon is being changed from 'Goodbye' to 'Hello' ①, it's actually being overwritten by a new string object with a new identity. Similarly, an expression using string concatenation creates a new string object ② with a new identity. Attempting to modify the string in-place with item assignment isn't allowed in Python ③.

A tuple's value is defined as the objects it contains and the order of those objects. Tuples are immutable sequence objects that enclose values in parentheses. This means that items in a tuple can't be overwritten:

```
>>> eggs = ('cat', 'dog', [2, 4, 6])
>>> id(eggs)
39560896
>>> id(eggs[2])
40654152
>>> eggs[2] = eggs[2] + [8, 10]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

But a mutable list inside an immutable tuple can still be modified in-place:

```
>>> eggs[2].append(8)
>>> eggs[2].append(10)
>>> eggs
('cat', 'dog', [2, 4, 6, 8, 10])
>>> id(eggs)
39560896
>>> id(eggs[2])
40654152
```

Although this is an obscure special case, it's important to keep in mind. The tuple still refers to the same objects, as depicted in Figure 7-3. But if a tuple contains a mutable object and that object changes its value—that is, if the object mutates—the value of the tuple also changes.

I, and almost every Pythonista, call tuples immutable. But whether some tuples can be called mutable depends on your definition. I explore this topic more in my PyCascades 2019 talk, “The Amazing Mutable, Immutable Tuple” at https://invpy.com/amazingtuple/. You can also read Luciano Ramalho's explanation in Chapter 2 of Fluent Python. (O'Reilly Media, 2015)