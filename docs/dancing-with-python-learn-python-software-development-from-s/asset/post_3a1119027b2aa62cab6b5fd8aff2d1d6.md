
Now we wrap this code in a function and allow arguments.

```
```python
def least_first(items):
    for index in range(1, len(items)):
        if items[0] > items[index]:
            items[0], items[index] = items[index], items[0]
    return items

least_first([3, 2, 1, 4])
```
```

```
```text
[1, 3, 2, 4]
```
```

```
```python
least_first([5, -1, 8, -3, 0])
```
```

```
```text
[-3, 5, 8, -1, 0]
```
```

This technique worked well, but what if the list has only one item or is empty? Will we get an error?

```
```python
least_first([10])
```
```

```
```text
[10]
```
```

```
```python
least_first([])
```
```

```
```text
[]
```
```

These *edge cases* worked because of the way *range* works and because we did not access `items[0]` for the empty list.

The variables `items` and `index` are both *local to* `least_first`. I have not defined them in the Python environment, and they are not known there.

![](img/4ffeafa3e3f1373f02971e43b0755ea6_99_0.png)

Figure 2.10: Local variables in the “least_first” function

```
```text
items
```
```

```
```text
NameError: name 'items' is not defined
```
```

Though you should try to use descriptive names for variables, it is common practice for coders to use i, j, and k as indices, as I did in this function definition.

# Exercise 2.24

Remember that `range` can return an `int` up to but not including its second parameter. Why was this second parameter `len(items) - 1` for i and `len(items)` for j?

# Exercise 2.25

What would you change to sort the list in descending order? Add a Boolean-valued parameter to `selection_sort` to allow the user to select the order of sorting.

# Exercise 2.26

For our original `selection_sort`, calculate the number of comparisons and item swaps it does for each of the following lists.

```
[]
[5]
[5, 4]
[5, 4, 3]
[5, 4, 3, 2]
[5, 4, 3, 2, 1]
```

Augment the function with calls to `print` to show the counts. Can you see a pattern?

Why would you consider these to be “worst-case scenarios” for sorting lists of numbers of the given lengths?

# 2.12.4 Nesting function definitions

You can define one function inside another function. Here I define `test_and_swap` as a nested function within `nested_selection_sort`.

```
def nested_selection_sort(items):
    def test_and_swap(first_index, second_index):
        if items[first_index] > items[second_index]:
```

```
```python
recursive_selection_sort([3, 2, 1, 4])

[1, 2, 3, 4]
```
```

If you call *recursive_selection_sort* with only one argument, the list of numbers, Python gives the second parameter *first_index* a value of 0. By convention, we omit the spaces around the equal sign “=”. We return to recursion in section 6.12.

Though I have been showing the code of our functions, I should be inserting comments. These remarks allow other coders to understand what the code does, improve it, and maybe fix it. In section 7.7, I discuss the conventions we use to comment Python code. Any line that begins with a hash symbol “#” is a comment and Python does not execute it.

```
```python
def do_nothing():
    # do_nothing does nothing whatsoever
    pass
```
```

Use the *pass* keyword when syntax requires an expression to be present, but we don’t want to evaluate anything. Its presence is usually self-documenting.

![](img/4ffeafa3e3f1373f02971e43b0755ea6_103_0.png)

# 2.13 Summary

In this chapter, we looked at many of the general features of programming languages from *Chapter 1, Doing the Things That Coders Do*, and saw how Python implements them. My coverage here was a survey of some of Python’s core functionality. The remaining chapters in this part go into greater detail and breadth. Parts II and III look at many of the advanced features and libraries that give Python its preeminence among coding environments.

These different schemes affect how easy it is to retrieve a specific guitar from the collection. In the first and third cases, you might need to look at every guitar to find the one you need. In the second case, you only need to start at the left and move to the right until you either find the guitar or see one where the model name is alphabetically later than the one you seek.

You could be even smarter and look at the guitar in the middle of the collection. If that is the guitar you seek, you are finished. If your model is before it, you only need to search the first half of the guitars. If it is after, you search the second half. Repeat this kind of search in your chosen half.

As with guitars, collections in Python have different storage and retrieval schemes. They are more or less efficient for a given use. In this chapter, we look at the three core collection types in Python: lists, dictionaries, and sets. Along the way, we encounter *tuples*, which we saw when we did multiple assignments in section 2.4.

# Topics covered in this chapter

+   3.1 The big three ......................................................... 78
+   3.2 Lists ......................................................... 80
+   3.3 The joy of O(1) ......................................................... 99
+   3.4 Tuples ......................................................... 100
+   3.5 Comprehensions ......................................................... 104
+   3.6 What does “Pythonic” mean? ......................................................... 106
+   3.7 Nested comprehensions ......................................................... 107
+   3.8 Parallel traverse ......................................................... 109
+   3.9 Dictionaries ......................................................... 110
+   3.10 Sets ......................................................... 119
+   3.11 Summary ......................................................... 126

# 3.1 The big three

In section 2.3, I informally introduced lists. These are objects of type `list` and can contain items of any type. We access them via an `int` index. They are mutable and in a defined order. In Figure 3.1, the list on the left contains six items, two of which are duplicates.

```
try_try(20)
```

Whoops, something went wrong.
We can finalize processing here.
1

Here except: catches any kind of exception raised within the evaluated code. You should be as specific as possible in naming the exception you want to catch. In addition to the several dozen built-in Python exceptions, you can define your own. Let’s write a simple function with several except clauses.

```
def error_tester(index, divisor):
    try:
        print(brands[index // divisor])
    except IndexError as error:
        print(f"Index problem:\n'{error}'")
    except ZeroDivisionError:
        print("Did you mean to divide by 0?")
    except:
        print("Something unexpected happened.")
```

```
error_tester(6, 1)
```

Index problem:
'list index out of range'

```
error_tester(2, 0)
```

Did you mean to divide by 0?

```
error_tester(4, "hello")
```

Something unexpected happened.

# Exercise 3.1

Evaluate brands[2/1]. What kind of error is raised?

The Python Standard Library lists the full set of built-in exceptions. [PYL] Note how I use as with error to store the information that Python provides with the exception. The value of error can help you determine why the exception was raised.

![](img/4ffeafa3e3f1373f02971e43b0755ea6_112_0.png)

Figure 3.2: Flowchart of the general form of Python exception handling

Figure 3.2 shows the general flow of Python exception handling. As we see in the next section, you can use exceptions as a style of coding in addition to catching bad behavior.

# 3.2.4 Removing list items

Let’s recall our list of electric guitar brand names and make a copy of it so we can use the original later:

```
brands = [
    "Fender",
    "Gibson",
    "Ibanez"
]
brands_2 = brands

print(f"{brands = }")
print(f"{brands_2 = }")
```

brands = ['Fender', 'Gibson', 'Ibanez']
brands_2 = ['Fender', 'Gibson', 'Ibanez']

I also assigned brands to brands_2, and we’ll see why in a moment.

Let’s remove the list entry "Ibanez". We can see by inspection that this is at index 2. Python uses del to remove a list item.

```
del brands[2]
print(f"{brands = }")
print(f"{brands_2 = }")
```

brands = ['Fender', 'Gibson']
brands_2 = ['Fender', 'Gibson']

brands changes and so does brands_2! When I did the assignment brands_2 = brands, I was not making two lists. I was giving two names to the same list. Therefore, when I changed the underlying list, we saw it whether we referred to it by brands or brands_2.

![](img/4ffeafa3e3f1373f02971e43b0755ea6_113_0.png)

Figure 3.3: Two variables referring to the same list object
