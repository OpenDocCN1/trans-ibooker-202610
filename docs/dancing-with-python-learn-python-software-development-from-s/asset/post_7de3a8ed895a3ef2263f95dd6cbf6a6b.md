
# Collecting Things Together

Whoops, they are in the wrong order! There are several ways to fix the problem, but this works:

```
```python
brands = original_brands.copy()
index = 0

for brand in ["Epiphone", "Paul Reed Smith"]:
    brands.insert(index, brand)
    index += 1

brands
```
```

`['Epiphone', 'Paul Reed Smith', 'Fender', 'Gibson', 'Ibanez']`

![](img/4ffeafa3e3f1373f02971e43b0755ea6_124_0.png)

**Exercise 3.13**

Write code that works with our guitar brand lists to perform the sixth activity from the start of this section: “Add items from another list at a specific index for our list.”

# 3.2.9 Unpacking lists

We’ve seen how we can access individual items and sub-lists of a list. Via assignment, we can unpack a list into its parts. Given this list,

```
```python
models = [
    "Stratocaster",
    "Telecaster",
    "Les Paul",
    "Flying V",
    "RG"
]
```
```

we extract the first and remaining items by

```
```python
first_model, *remaining_models = models
```
```

```
```python
first_model
```
```

`'Stratocaster'`

```
```python
remaining_models
```
```

`['Telecaster', 'Les Paul', 'Flying V', 'RG']`

# Exercise 3.15

**Optional:** If you know the Lisp or Scheme programming languages, how would you define *car* or *cdr*? Do one version of each that uses list unpacking and another that uses slicing.

I think list unpacking is an underused feature in Python.

# 3.2.10 Reversing and sorting lists

Let’s create a list of years when our guitar models were introduced.

```
years = [1954, 1950, 1952, 1958, 1987]
years
```

[1954, 1950, 1952, 1958, 1987]

The *reverse* method flips the order of items in the **list**. It changes the list itself; it doesn’t make a copy.

```
years.reverse()
years
```

[1987, 1958, 1952, 1950, 1954]

By default, *sort* puts the list elements in ascending order.

```
years.sort()
years
```

[1950, 1952, 1954, 1958, 1987]

With an optional keyword second parameter, you can sort them in descending order.

```
years.sort(reverse=True)
years
```

[1987, 1958, 1954, 1952, 1950]

Though I’ve shown methods and operations that physically change the **list** and its contained items, some similar functions leave the original intact.

It takes the same amount of time to get the length, whether the list is tiny or immense. There is still some system- and implementation-dependent overhead, but now

the time to compute the length ≤ C × 1 = C.

This method is O(1). It’s wonderful!

# Exercise 3.16

What does it mean for a list algorithm to be O(n²)? We say that the algorithm is quadratic or has quadratic complexity.

Show that the selection sort from section 2.12.3 is O(n²).

Instead of analyzing how much time an algorithm uses, we could also ask how much memory it needs and use Big O for that. We analyze the best case, worst case, and average case for execution time and memory usage.

An algorithm such as O(2ⁿ) is exponential and should be avoided unless n is small and you have no alternative. The “2” might be another number greater than 1.

I cover complexity and Big O notation in section 2.8 of Dancing with Qubits. [D.W.Q]

# Exercise 3.17

I give you an O(3ⁿ) algorithm and tell you that C = 17.4. What is the upper bound on the time it takes for the algorithm’s implementation to run for n = 1, n = 10, and n = 100?

# 3.4 Tuples

Tuples are like lists except:

+   - You use parentheses instead of square brackets, and the parentheses are often optional.
+   - You can’t change the structure of a tuple: they are immutable.

We saw tuples when we did multiple assignments. We did not need parentheses then, but we could have included them if we wished.

```
a, b = 3, -2
print(f"{a = }    {b = }")

a, b = (9, 0.3)
print(f"{a = }    {b = }")

(a, b) = ("a", "b")
print(f"{a = }    {b = }")
```

a = 3    b = -2
a = 9    b = 0.3
a = 'a'  b = 'b'

# Exercise 3.18

What exception does Python raise if you try to assign a value to an item in a tuple via an index?

Use *tuple* to turn a list into a tuple and *list* to turn a tuple into a list.

```
t = 2, -4, 9
t
list(t)
tuple([-16, 25, -36])
```

(-16, 25, -36)

# Exercise 3.19

What are three ways to make an empty tuple?

The syntax gets confusing if you try to create a tuple with a single element.

```
(3.14)
```

3.14

These parentheses grouped a simple expression. Instead, add a trailing comma to make the tuple.

```
(3.14,)
```

(3.14,)

Tuples show up in many ways in Python coding, and you may not always realize you are using them. Coders often return multiple values from functions via tuples. In the next example, I define quo_rem to compute the quotient and remainder of two integers. The second integer must not be zero.

```
def quo_rem(dividend, divisor):
    if not isinstance(dividend, int):
        raise TypeError("dividend must be an integer")
    if not isinstance(divisor, int):
        raise TypeError("divisor must be an integer")

    if divisor == 0:
        raise ZeroDivisionError("divisor is 0")

    return dividend // divisor, dividend % divisor
```

```
quo_rem(11, 4)
```

(2, 3)

Though I could have coded only the last line containing the return, I added three conditionals to ensure that the arguments were valid. In the first two on lines 2 and 4, I used isinstance to make sure that the arguments were integers. That is, I checked that they were instances of type int. It should not surprise you that a value having the wrong type should raise a TypeError exception.

Had I tried to divide by zero, Python would have raised a ZeroDivisionError itself. I wanted to be explicit in my function, so I added the test on line 7.

# Exercise 3.20

Should I have used elif for the second and third conditionals in quo_rem?

> If the implementation is hard to explain, it’s a bad idea.
> If the implementation is easy to explain, it may be a good idea.
> Namespaces are one honking great idea – let’s do more of those!

* Guido van Rossum, the creator of Python, is Dutch. [PEP020] [ZEN]

# 3.7 Nested comprehensions

You can nest comprehensions to create lists within lists. Our task in this section is to explore several ways of making this list of four lists, each of which has four numbers:

[ [1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], [0, 0, 0, 1] ]
[1 0 0 0]
[0 1 0 0]
[0 0 1 0]
[0 0 0 1]

On the left is the list of lists, and on the right is a matrix. In particular, it is a 4 by 4 identity matrix, as you see in linear algebra. The matrix is diagonal: the only non-zero entries are on the main diagonal. Coders frequently use lists of lists to implement matrices in Python.

Linear algebra is at the heart of quantum computing and many other disciplines. For now, we look at how to build the list.

In this case, it is not onerous to type in the list, but we use matrices of size 2ⁿ by 2ⁿ in quantum computing. When n is 15, we get a 32768 by 32768 matrix, for example.

Let’s begin by creating a 4 by 4 list of lists filled with zeros.

```
m = [[0 for i in range(4)] for j in range(4)]
m
[[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
```

Note how there are two for loops, one acting on the outer list, and the other on the four contained lists. That’s what makes this a nested comprehension. Now we set the diagonal elements.

```
for i in range(4): m[i][i] = 1
m
[[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], [0, 0, 0, 1]]
```

Since the loop and the action are so small, I put them both on the same line. I can define a function and do everything in the nested list comprehension.

```
def one_or_zero(x, y):
    if x == y:
        return 1
    return 0

[[one_or_zero(i, j) for i in range(4)] for j in range(4)]
```

[[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], [0, 0, 0, 1]]

I can be more Pythonic and improve the function:

```
def one_or_zero(x, y): return 1 if x == y else 0

[[one_or_zero(i, j) for i in range(4)] for j in range(4)]
```

[[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], [0, 0, 0, 1]]

We haven’t seen this before! Python allows you to put a conditional in the middle of an expression. Finally, I skip the function entirely.

```
[[1 if i == j else 0 for i in range(4)] for j in range(4)]
```

[[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], [0, 0, 0, 1]]

Using a conditional in this way is an additional technique that allows you to write concise and readable code.

# Exercise 3.23

What is happening in the following comprehension?

```
[f"i + j = {i+j} for {i = } and {j =}"
    for i in range(2) for j in range(3)]
```

['i + j = 0 for i = 0 and j =0',
'i + j = 1 for i = 0 and j =1',
'i + j = 2 for i = 0 and j =2',
'i + j = 1 for i = 1 and j =0',
'i + j = 2 for i = 1 and j =1',
'i + j = 3 for i = 1 and j =2']

Is this nested or are the for loops running in parallel?
