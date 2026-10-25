```
@make_timer
def count_nums(n):
    for i in range(n):
        for j in range(1000):
            pass
```

```
>>> iter1 = reversed([1, 2, 3, 4])
>>> print(iter1)
<list_reverseiterator object at 0x1111d7f28>
```

```
>>> iter1 = reversed([1, 2, 3, 4])
>>> for i in iter1:
    print(i, end=' ')

4 3 2 1
```

```
def make_evens_gen():
    for n in range(2, 11, 2):
        yield n
```

```
>>> make_evens_gen()
<generator object make_evens_gen at 0x1068bd410>
```

```
>>> my_gen = make_evens_gen()
>>> next(my_gen)
2
>>> next(my_gen)
4
>>> next(my_gen)
6
```