

You can, however, pass a list to a function and write the function in such a way that the function modifies some or all of the values in that list. This is possible because lists (in contrast to strings and tuples) are mutable. Here’s an example:

![](img/6e45221a8147f2442c653a3d4a866570_197_0.png)

```python
def set_list_vals(list_arg):
    list_arg[0] = 100
    list_arg[1] = 200
    list_arg[2] = 150

a_list = [0, 0, 0]
set_list_vals(a_list)
print(a_list)  # Prints [100, 200, 150]
```

This approach works because the values of the list are changed in place, without creating a new list and requiring variable reassignment. But the following example fails to change the list passed to it.

![](img/6e45221a8147f2442c653a3d4a866570_197_1.png)

```python
def set_list_vals(list_arg):
    list_arg = [100, 200, 150]

a_list = [0, 0, 0]
set_list_vals(a_list)
print(a_list)  # Prints [0, 0, 0]
```

With this approach, the values of the list, `a_list`, were not changed after the function returned. What happened?

The answer is that the list argument, `list_arg`, was reassigned to refer to a completely new list. The association between the variable `list_arg` and the original data, `[0, 0, 0]`, was broken.

However, slicing and indexing are different. Assigning into an indexed item or a slice of a list does not change what the name refers to; it still refers to the same list, but the first element of that list is modified.

> Click here to view code image

```python
my_list[0] = new_data  # This really changes list data.
```

## 3.18 MULTIDIMENSIONAL LISTS

List elements can themselves be lists. So you can write code like the following:

> Click here to view code image

```python
weird_list = [ [1, 2, 3], 'John', 'George' ]
```

But much more common is the true _multidimensional list_, or _matrix_. The following assignment creates a 3 × 3 list and assigns it to the variable `mat`:

> Click here to view code image

```python
mat = [[10, 11, 21], [20, 21, 22], [25, 15, 15]]
```

The right side of this assignment creates three rows, and each has three values:

```python
[10, 11, 12],
[20, 21, 22],
[25, 15, 15]
```

You can index an individual element within this two-dimensional list as follows:

> Click here to view code image

```python
list_name[row_index][column_index]
```

As usual, indexes in Python run from 0 to N–1, where N is the length of the dimension. You can use negative indexes, as usual. Therefore, `mat[1][2]` (second row, third column) produces the value 22.

> Note
> This chapter describes how to use the core Python language to create multidimensional lists. Chapter 12 describes the use of the **numpy** package, which enables the use of highly optimized routines for manipulating multidimensional arrays, especially arrays (or matrices) of numbers.

## 3.18.1 Unbalanced Matrixes

Although you’ll probably most often create matrixes that are rectangular, you can use Python to create _unbalanced matrixes_. Here’s an example:

> Click here to view code image

```python
weird_mat = [[1, 2, 3, 4], [0, 5], [9, 8, 3]]
```

Program code can determine the exact size and shape of a Python matrix through inspection. Taking the length of such a list (in this case, a matrix) gets the number of elements at the top level. Here’s an example:

> Click here to view code image

```python
len(weird_mat)   # Equal to 3.
```

This result tells you that there are three rows. You can then get the length of each of these rows, within the matrix, as follows:

> Click here to view code image

```python
len(weird_mat[0])   # Equal to 4.
len(weird_mat[1])   # Equal to 2.
len(weird_mat[2])   # Equal to 3.
```

This process can be repeated to any depth.

## 3.18.2 Creating Arbitrarily Large Matrixes

Creating an arbitrarily large multidimensional list is a challenge in Python. Fortunately, this section provides the simplest solution (other than using the dedicated **numpy** package described in Chapter 12).

Remember, Python has no concept of data declaration. Therefore, Python matrixes cannot be declared; they must be built.

It might seem that list multiplication would solve the problem. It does, in the case of one-dimensional lists.

```python
big_list = [0] * 100  # Create a list of 100 elements each initialized to 0.
```

This works so well, you might be tempted to just generalize to a second dimension.

```python
mat = [[0] * 100] * 200
```

But although this statement is legal, it doesn’t do what you want. The inner expression, `[0] * 100`, creates a list of 100 elements. But the code repeats that data 200 times—not by creating 200 separate rows but instead by creating 200 references to the same row.

The effect is to create 200 rows that aren’t separate. This is a shallow copy; you get 200 redundant references to the same row. This is frustrating. The way around it is to append each of the 200 rows one at a time, which you can do in a for loop:

```python
mat = [ ]
for i in range(200):
    mat.append([0] * 100)
```

In this example, `mat` starts out as an empty list, just like any other.

Each time through the loop, a row containing 100 zeros is appended. After this loop is executed, `mat` will refer to a true two-dimensional matrix made up of 20,000 fully independent cells. It can then be indexed as high as `mat[199][99]`. Here’s an example:

```python
mat[150][87] = 3.141592
```

As with other **for** loops that append data to a list, the previous example is a great candidate for list comprehension.

> Click here to view code image

```python
mat = [ [0] * 100 for i in range(200) ]
```

The expression `[0] * 100` is the _value_ part of this list-comprehension expression; it specifies a one-dimensional list (or “row”) that consists of 100 elements, each set to 0. This expression should not be placed in an additional pair of brackets, by the way, or the effect would be to create an extra, and unnecessary, level of indexing.

The expression `for i in range(200)` causes Python to create, and append, such a row ... 200 times.

![](img/6e45221a8147f2442c653a3d4a866570_202_0.png)

> Click here to view code image

```python
matrix_name = [[init] * ncols for var in range(nrows)]
```

In this syntax display, _init_ is the initial value you want to assign each element to, and _ncols_ and _nrows_ are the number of columns and rows, respectively.

Because `var` isn’t important and need not be used again, you can replace it with the trivial name “_” (just an underscore), which is basically a placeholder. For example, to declare a 30 × 25 matrix, you would use this statement:

![](img/6e45221a8147f2442c653a3d4a866570_203_0.png)

```python
mat2 = [ [0] * 25 for _ in range(30) ]
```

You can use this technique to build matrixes of even higher dimensions, each time adding a level of list comprehension. Here is a 30×20 × 25 three-dimensional list:

![](img/6e45221a8147f2442c653a3d4a866570_203_1.png)

```python
mat2 = [ [ [0] * 25 for _ in range(20) ]
for _ in range(30) ]
```

And here is a 10 × 10 × 10 × 10 four-dimensional list:

![](img/6e45221a8147f2442c653a3d4a866570_203_2.png)

```python
mat2 = [[[ [0] * 10 for _ in range(10) ]
for _ in range(10) ]
for _ in range(10) ]
```

You can build matrixes of higher dimensions still, but remember that as dimensions increase, things get bigger—fast!

# CHAPTER 3 SUMMARY

This chapter has demonstrated just how powerful Python lists are. Many of these same abilities are realized in functions, such as `len`, `count`, and `index`, which apply to other collection classes as well, including strings and tuples. However, because lists are mutable, there are some list capabilities not supported by those other types, such as `sort` and `reverse`, which alter list data “in place.”

This chapter also introduced some exotic abilities, such as the use of `functools` and lambda functions. It also explained techniques for creating multidimensional lists, an ability that Chapter 12 provides efficient and superior alternatives to; still, it’s useful to know how to create multidimensional lists using the core language.

# CHAPTER 3 REVIEW QUESTIONS

+ 1 Can you write a program, or a function, that uses both positive and negative indexing? Is there any penalty for doing so?

+ 2 What’s the most efficient way of creating a Python list that has 1,000 elements to start with? Assume every element should be initialized to the same value.

+ 3 How do you use slicing to get every other element of a list, while ignoring the rest? (For example, you want to create a new list that has the first, third, fifth, seventh, and so on element.)

+ 4 Describe some of the differences between indexing and slicing.

