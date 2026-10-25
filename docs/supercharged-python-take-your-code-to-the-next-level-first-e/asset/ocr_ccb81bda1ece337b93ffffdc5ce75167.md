raised, that exception causes the program to terminate abruptly
unless the **except** clause catches the exception by specifying a
matching _exception_class_. If you want the program to look for
more than one type of exception, you can do so by using
multiple **except** clauses.

```
try:
    statement_block_1
except exception_class_A:
    statement_block_A
[ except exception_class_B:
    statement_block_B ]...
```

In this case, the brackets are not intended literally but
indicate optional items. The ellipses (. .) indicate that there
may be any number of such optional clauses.
There are also two more optional clauses: **else** and
**finally**. You can use either one or both.

![](bbox=[0.148, 0.567, 0.255, 0.647])

```
try:
    statement_block_1
except exception_class_A:
    statement_block_A
[ except exception_class_B:
    statement_block_B ]...
[ else:
    statement_block_2 ]
[ finally:
    statement_block_3 ]
```