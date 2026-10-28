
As an alternative to del, use the pop list method.

```
brands.pop(0)
```

'Fender'

```
brands
```

['Gibson']

```
brands.pop()
```

'Gibson'

pop returns the removed item while del does not. If you use pop with no arguments, it returns the last item if the list is not empty.

Since I’m going to show several techniques for removing items, let’s make two lists with the same guitar brands.

```
brands = [
    "Fender",
    "Gibson",
    "Ibanez"
]

original_brands = brands.copy()
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_114_0.png)

Figure 3.4: Variables referring to two copies of a list

If we don’t know if "Ibanez" is in the list or where it is, we can use the *index* method to locate the first occurrence.

```
```python
brands.index("Ibanez")
```

2
```

If the item is not in the list, *index* raises an exception:

```
```python
brands.index("Gretsch")
```

ValueError: 'Gretsch' is not in list
```

One way of removing "Ibanez" from the list is to catch the exception:

```
```python
try:
    del brands[brands.index("Ibanez")]
    print("'Ibanez' was present and was removed")
except ValueError:
    print("'Ibanez' was not present and so not removed")

brands
```

'Ibanez' was present and was removed
['Fender', 'Gibson']
```

```
```python
brands = original_brands.copy()

try:
    del brands[brands.index("Gretsch")]
    print("'Gretsch' was present and was removed")
except ValueError:
    print("'Gretsch' was not present and so not removed")

brands
```

'Gretsch' was not present and so not removed
['Fender', 'Gibson', 'Ibanez']
```

## Exercise 3.4

The clear function removes all items from a list. Explain the difference in behavior between

```
a = [1, 2, 3]
b = a
a = []
print(f"{a = }")
print(f"{b = }")
```

a = []
b = [1, 2, 3]

and

```
a = [1, 2, 3]
b = a
a.clear()
print(f"{a = }")
print(f"{b = }")
```

a = []
b = []

How does this show the mutability of lists?

## 3.2.5 Changing list items

We can change lists, and that’s what “mutable means.” The same syntax we use to refer to a list item works on the left-hand side of an assignment.

```
brands = original_brands.copy()

brands[2] = "Gretsch"
brands
```

['Fender', 'Gibson', 'Gretsch']

## 3.2.6 Slicing: Referencing sub-lists

Slice notation is a convenient way to specify a group of zero or more items in a list. When you first start slicing, it can appear both powerful and mystifying. Over time, you’ll appreciate its elegance, and your comfort will increase.

We start by looking at how we use slicing to pull out a sub-list of items from the original list. The items might not be next to each other in the original, though they are picked out from the usual or reverse item sequence. For a list called the_list, a slice looks like

```
the_list[start=0 : end=len(the_list)] : step=1
```

There are three parameters separated by colons, and they each have default values.

## Exercise 3.7

For each of the following examples, shade the item boxes in a diagram like that in Figure 3.5 to show the list items in the slice.

![](img/4ffeafa3e3f1373f02971e43b0755ea6_119_0.png)

Figure 3.5: List, index, and slice example

Fill in the values of start, end, and step.

I now show several examples of how to slice lists.

```
the_list = ["a", "b", "c", "d", "e", "f"]
the_list
```

['a', 'b', 'c', 'd', 'e', 'f']

- the_list[0:] and the_list[:] return a copy of the_list.

```
the_list[0:]
```

['a', 'b', 'c', 'd', 'e', 'f']

```
the_list[:]
['a', 'b', 'c', 'd', 'e', 'f']
```

- the_list[the_index:] returns a new list containing the items in the_list from position the_index to the end. If the_index is positive and ≥ len(the_list), we get an empty list.

```
the_list[2:]
['c', 'd', 'e', 'f']
```

```
the_list[10:]
[]
```

- the_list[:the_index] returns a new list from the items in the original list up to but not including the item at the_index.

```
the_list[:2]
['a', 'b']
```

- the_list[the_first_index:the_second_index] returns a new list from the items in the original list starting with the item at position the_first_index up to but not including the item at the_second_index.

```
the_list[1:3]
['b', 'c']
```

- the_list[::2] returns a new list from the items in the original list starting with the first, and then including every second item.

```
the_list[::2]
['a', 'c', 'e']
```

- the_list[1::3] returns a new list from the items in the original list starting with the second, and then including every third item.

```
the_list[1::3]
['b', 'e']
```

- the_list[::-1] returns a copy of the_list in reverse order.

```
the_list[::-1]
['f', 'e', 'd', 'c', 'b', 'a']
```

I think the technique of reversing the list is marvelous.

**Exercise 3.8**

Write a Python function to generate all possible questions for this book with negative, zero, and positive integer values of `start`, `end`, and `step`. Just kidding, but do experiment.

**Exercise 3.9**

Devise formal rules for how slicing works with allowable values of `start`, `end`, and `step`.

## 3.2.7 Slicing: Changing sub-lists

Just as we saw that we could **change** and **remove** list items, we can use slice notation with assignment to alter a list’s structure.

The idea is that we create a slice expression to specify part of the list, and then use assignment to substitute another list for the slice.

```
the_list = ["a", "b", "c", "d", "e", "f"]
```

```
the_list[:1] = [1, 2, 3]
the_list
```

```
[1, 2, 3, 'b', 'c', 'd', 'e', 'f']
```

```
the_list[6:] = [7, 8, 9]
the_list
```

```
[1, 2, 3, 'b', 'c', 'd', 7, 8, 9]
```

```
the_list[4:6] = ["alpha", "beta", "gamma"]
the_list
```

```
[1, 2, 3, 'b', 'alpha', 'beta', 'gamma', 7, 8, 9]
```

**Exercise 3.10**

While these examples work fine, Python requires the slice and the list to the right of the assignment to have the same length when you use a step other than one. Experiment with positive and negative steps to see what works and what doesn’t.

## 3.2.8 Extending lists

How can we add items to an existing list? There are several things we might like to do:

- 1. Append an item to the end of our list.
- 2. Prepend an item at the beginning of our list.
- 3. Insert an item at a specific index for our list.
- 4. Extend our list at the end with items from another list.
- 5. Add items from another list to the beginning of our list.
- 6. Add items from another list at a specific index for our list.

The first four are easy, but the last two require more work on your part. We again work with guitar brands.

```
brands = [
    "Fender",
    "Gibson",
    "Ibanez"
]

original_brands = brands.copy()
```

To add an item at the end of our list, use *append*.

```
brands.append("Gretsch")
brands
```

['Fender', 'Gibson', 'Ibanez', 'Gretsch']

To add an item at the beginning of our list, use *insert* with index 0.

```
brands = original_brands.copy()
brands.insert(0, "Gretsch")
brands
```

['Gretsch', 'Fender', 'Gibson', 'Ibanez']

More generally, use the index where you want to place the new item with *insert*.

```
brands = original_brands.copy()
brands.insert(1, "Gretsch")
brands
```

['Fender', 'Gretsch', 'Gibson', 'Ibanez']

## Exercise 3.11

Suppose you want to put an item at the beginning of an empty list. Should you use *append* or *insert*? Try both approaches.

## Exercise 3.12

How does *insert* behave when you use an invalid index for your original list?

The *extend* method is like *append*, but it takes all items of a given list and puts them at the end of our list.

```
brands = original_brands.copy()
brands.extend(["Epiphone", "Paul Reed Smith"])
brands
```

['Fender', 'Gibson', 'Ibanez', 'Epiphone', 'Paul Reed Smith']

It does the same as this loop:

```
brands = original_brands.copy()

for brand in ["Epiphone", "Paul Reed Smith"]:
    brands.append(brand)

brands
```

['Fender', 'Gibson', 'Ibanez', 'Epiphone', 'Paul Reed Smith']

This technique gives us a hint at how to insert the items at the beginning. Let’s try this:

```
brands = original_brands.copy()

for brand in ["Epiphone", "Paul Reed Smith"]:
    brands.insert(0, brand)

brands
```

['Paul Reed Smith', 'Epiphone', 'Fender', 'Gibson', 'Ibanez']
