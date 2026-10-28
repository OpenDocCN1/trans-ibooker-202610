`getsizeof` is a function in the **sys** module that returns the number of bytes of memory an object uses. In the case of a string, it includes the length, the characters themselves, plus system overhead for processes like garbage collection. If we add some text, the size gets larger, as we expect.

```
```python
a = "Better a witty fool, than a foolish wit."
len(a)
```
```

40

```
```python
getsizeof(a)
```
```

89

The size of a string object with 100 times the number of characters is roughly 100 times bigger, allowing for the overhead.

```
```python
b = 100 * a
len(b)
```
```

4000

```
```python
getsizeof(b)
```
```

4049

What about a list containing the two strings?

```
```python
getsizeof([a, b])
```
```

72

That’s odd, and it is much less than the sum of the sizes of the strings. The size is the same if we have a list of two copies of the longer string.

```
```python
getsizeof([b, b])
```
```

72

If we include a third copy, the number of bytes increases modestly.

```
```python
getsizeof([b, b, b])
```
```

80