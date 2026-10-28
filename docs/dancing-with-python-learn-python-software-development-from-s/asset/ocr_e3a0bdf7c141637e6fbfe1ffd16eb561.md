I can call *raiser* using *positional arguments*, where Python assigns the first argument to base and the second to exponent.

```
```python
raiser(2, 3)
```
```

8.0

I can also explicitly use the names of the parameters in *keyword arguments*.

```
```python
raiser(base=2, exponent=3)
```
```

8.0

By itself, that’s not very exciting, but it is clearer what the arguments are providing to the function. You can use keyword arguments in any order.

```
```python
raiser(exponent=3, base=2)
```
```

8.0

Positional arguments must come before keyword arguments. This works:

```
```python
raiser(2, exponent=3)
```
```

8.0

but this does not:

```
```python
raiser(base=2, 3)
```
```

SyntaxError: positional argument follows keyword argument

If you want to force users of your code to pass keyword arguments, put a “*” in the parameter list.

```
```python
def raiser(*, base, exponent):
    return math.pow(base, exponent)
```
```

```
```python
raiser(2, 3)
```
```

TypeError: raiser() takes 0 positional arguments
           but 2 were given