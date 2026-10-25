

a separate copy from `b_list`, so that if changes are made to `b_list`, then `a_list` would be unaffected.

Click here to view code image

```python
a_list = [1, 2, [5, 10]]
b_list = a_list[:]   # Member-by-member copy.
```

Now, let's modify `b_list` through indexing, setting each element to 0:

```python
b_list[0] = 0
b_list[1] = 0
b_list[2][0] = 0
b_list[2][1] = 0
```

You’d probably expect none of these assignments to affect `a_list`, because that’s a separate collection from `b_list`. But if you print `a_list`, here’s what you get:

```python
>>> print(a_list)
[1, 2, [0, 0]]
```

This may seem impossible, because `a_list` had the last element set to `[5, 10]`. Changes to `b_list` shouldn’t have any effect on the contents of `a_list`, but now the latter’s last element is `[0, 0]`! What happened?

The member-by-member copy, carried out earlier, copied the values 1 and 2, followed by a reference to the list-within-a-list. Consequently, changes made to `b_list` can affect `a_list` if they involve the second level.

Figure 3.4 illustrates the concept. Shallow copying makes new copies of top-level data only.

![](img/6e45221a8147f2442c653a3d4a866570_162_0.png)

# Figure 3.4. Shallow copying

And now you can see the problem. A member-by-member copy was carried out, but the list within the list was a *reference*, so both lists ended up referring to the same data in the final position.

The solution is simple. You need to do a *deep copy* to get the expected behavior. To get a deep copy, in which even embedded list items get copied, import the **copy** package and use **copy.deepcopy**.

Click here to view code image

```python
import copy
```

```python
a_list = [1, 2, [5, 10]]
b_list = copy.deepcopy(a_list)  # Create a DEEP COPY.
```

After these statements are executed, b_list becomes a new list completely unconnected to a_list. The result is illustrated in Figure 3.5, in which each list gets its own, separate copy of the list-within-a-list.

![](img/6e45221a8147f2442c653a3d4a866570_163_0.png)

With deep copying, the depth of copying extends to every level. You could have collections within collections to any level of complexity.

If changes are now made to b_list after being copied to a_list, they will have no further effect on a_list. The last element of a_list will remain set to [5, 10] until changed directly. All this functionality is thanks to deep copying.

# 3.8 LIST FUNCTIONS

When you work with lists, there are several Python functions you’ll find useful: These include **len**, **max**, and **min**, as well as **sorted**, **reversed**, and **sum**.

These are functions, not methods. The main difference is that methods use the dot (.) syntax; the other difference is that methods represent built-in abilities, whereas the functions here implement abilities that are useful with collections generally. Admittedly, this is sometimes a very fine distinction.

![](img/6e45221a8147f2442c653a3d4a866570_164_0.png)

Click here to view code image

```python
len(collection)         # Return length of the collection
max(collection)         # Return the elem with maximum value.
min(collection)         # Return the elem with minimum value.
reversed(collection)    # Produce iter in reversed order.
sorted(collection)      # Produce list in sorted order.
sum(collection)         # Adds up all the elements, which must be numeric.
```

The **len** function returns the number of elements in a collection. This includes lists, strings, and other Python collection types. In the case of dictionaries, it returns the number of keys.

You’ll often use **len** when working with lists. For example, the following loop doubles every item in a list. It’s necessary to use **len** to make this a general solution.

```python
for i in range(len(a_list)):
    a_list[i] *= 2
```

The **max** and **min** functions produce maximum and minimum elements, respectively. These functions work only on lists that have elements with compatible types, such as all numeric elements or all string elements. In the case of strings, alphabetical order (or rather, _code point order_) enables comparisons.

Here’s an example:

> Click here to view code image

```python
a_list = [100, -3, -5, 120]
print('Length of the list is', len(a_list))
print('Max and min are', max(a_list), min(a_list))
```

This prints the following:

```
Length of the list is 4
Max and min are 120 -5
```

The **sorted** and **reversed** functions are similar to the **sort** and **reverse** methods, presented in Section 3.11. But whereas those methods reorganize a list in place, these functions produce new lists.

These functions work on tuples and strings as well as lists, but the **sorted** function always produces a list. Here's an example:

![](img/6e45221a8147f2442c653a3d4a866570_166_0.png)

```python
a_tup = (30, 55, 15, 45)
print(sorted(a_tup))   # Print [15, 30, 45, 55]
```

The **reversed** function is unusual because it produces an iterable but not a collection. In simple terms, this means you need a **for** loop to print it or else use a **list** or **tuple** conversion. Here's an example:

```python
a_tup = (1, 3, 5, 0)
for i in reversed(a_tup):
    print(i, end=' ')
```

This prints

```
0 5 3 1
```

Alternatively, you can use the following:

```python
print(tuple(reversed(a_tup)))
```

This produces

```
(0, 5, 3, 1)
```

Finally, there is the **sum** function, which is extremely convenient. You could write a loop yourself to perform this function, but it’s nice not to have to do so. The **sum** function is supported for those arrays that are made up only of numeric types, such as **int** and **float**.

One possible use is to quickly and easily figure the average for any list of numbers. Here’s an example:

Click here to view code image

```python
>>> num_list = [2.45, 1, -10, 55.5, 100.03, 40, -3]
>>> print('The average is ', sum(num_list) / len(num_list))
The average is 26.56857142857143
```

# 3.9 LIST METHODS: MODIFYING A LIST

The largest single group of list methods includes those that modify list data in place, modifying data in place rather than creating a new list.

![](img/6e45221a8147f2442c653a3d4a866570_167_0.png)

Click here to view code image

```python
list.append(value) # Append a value
list.clear() # Remove all contents
list.extend(iterable) # Append a series of values
list.insert(index, value) # At index, insert value
list.remove(value) # Remove first instance of value
```

The **append** and **extend** methods have a similar purpose: to add data to the end of a list. The difference is that the **append** method adds a single element to the end of the list in question, whereas the **extend** method appends a series of elements from a collection or iterable.

Click here to view code image

```python
a_list = [1, 2, 3]
a_list.append(4)
a_list.extend([4]) # This has the same effect.
a_list.extend([4, 5, 6]) # Adds 3 elements to the list.
```

The **insert** method has a purpose similar to **append**. However, **insert** places a value at the position indicated by the _index_ argument; that is, the method places the new value just before whichever element is specified by the _index_ argument.

If the index is out of range, the method places the new value at the end of the list if the index is too high to be in range, and it inserts the new value at the beginning of the list if the index is too low. Here’s an example:

```python
a_list = [10, 20, 40] # Missing 30.
a_list.insert(2, 30 ) # At index 2 (third), insert 30.
print(a_list) # Prints [10, 20, 30, 40]
a_list.insert(100, 33)
print(a_list) # Prints [10, 20, 30, 40, 33]
a_list.insert(-100, 44)
print(a_list) # Prints [44, 10, 20, 30, 40, 33]
```

The **remove** method removes the first occurrence of the specified argument from the list. There must be at least one occurrence of this value, or Python raises a **ValueError** exception.

```python
my_list = [15, 25, 15, 25]
my_list.remove(25)
print(my_list) # Prints [15, 15, 25]
```

You may want to use **in**, **not in**, or the **count** method to verify that a value is in a list before attempting to remove it. Here’s a practical example that combines these methods. In competitive gymnastics, winners are determined by a panel of judges, each of whom submits a score. The highest and lowest scores are thrown out, and then the average of the remaining scores is taken. The following function performs these tasks:

