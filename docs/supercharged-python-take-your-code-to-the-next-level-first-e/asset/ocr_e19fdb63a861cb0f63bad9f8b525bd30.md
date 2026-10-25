```
False
>>> issubclass(int, int)
True
>>> issubclass(f, int) # TypeError: f is not a
class
```

# # ITER(OBJ)

Iteration function. This function call assumes that _obj_ is an iterable—an object that returns an iterator. This includes standard collections and sequences, as well as generators.

If the argument is not an iterable, then calling `iter(obj)` causes a **TypeError** exception to be raised. If _obj_ is an iterable, the call to `iter` should return an iterator object. Such an object does the actual stepping through a sequence of values by responding to `next`.

A few examples should clarify. First of all, you can’t legally call `iter(obj)` if the object is not an iterable. For example:

Click here to view code image

```
>>> gen = iter(5) # This raises a TypeError
```

However, the call is valid if the target object is a list, even if (as in this case) it is a short list containing only a single member.

```
>>> gen = iter([5])
```

It’s more common, of course, to use `iter` on a longer list containing at least two members. The object returned—called