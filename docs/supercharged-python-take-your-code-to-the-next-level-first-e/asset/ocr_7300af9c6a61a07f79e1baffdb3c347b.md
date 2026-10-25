This block of code produces the result you’d expect:

```
2 4 6 8 10
```

But let’s take a look at what’s really happening. The **for** block calls `make_evens_gen` one time. The result of the call is to get a generator object. That object then provides the values in the **for** loop. The same effect is achieved by the following code, which breaks the function call onto an earlier line.

```
>>> my_gen = make_evens_gen()
>>> for i in my_gen:
    print(i, end=' ')
```

Remember that `my_gen` is an iterator object. If you instead referred to `make_evens_gen` directly, Python would raise an exception.

Click here to view code image

```
for i in make_evens_gen:     # ERROR! Not an
iterable!
    print(i, end=' ')
```

Once you understand that the object returned by the generator function is the generator object, also called the iterator, you can call it anywhere an _iterable_ or _iterator_ is accepted in the syntax. For example, you can convert a generator object to a list, as follows.

Click here to view code image