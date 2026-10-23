
## 5

## ORGANIZING VALUES USING LISTS

![](img/3e15323aee4cec3bf673834bfafe8024_142_0.png)

We’ve seen that we can use strings to work with a sequence of characters. In this chapter, we’ll learn about lists, which help us work with sequences of other types of values, such as integers and floats. We’ll also learn that we can nest lists inside of lists, which lets us work with grids of data.

We’ll solve three problems using lists: finding the smallest neighborhood of a collection of villages, determining whether sufficient money has been raised for a school trip, and calculating the number of bonuses offered by a bakery.

## Problem #11: Village Neighborhood

In this problem, we’re going to find the size of the smallest neighborhood of a collection of villages. We’ll find it helpful to store all of the neighborhood sizes. We might have as many as 100 villages, though, and using a separate variable for each village would be a nightmare. We’ll see that lists allow us to aggregate what would otherwise be separate variables into one collection. We’ll also learn about Python’s powerful list operations for modifying, searching, and sorting a list.

This is DMOJ problem ccc18s1.

## The Challenge

There are $n$ villages located at distinct points on a straight road. Each village is represented by an integer that indicates its position on the road.

A village's left neighbor is the village with the next smallest position; a village's right neighbor is the village with the next biggest position. The *neighborhood* of a village consists of half the space between that village and its left neighbor, plus half the space between that village and its right neighbor. For example, if there's a village at position 10, with its left neighbor at position 6 and its right neighbor at position 15, then this village's neighborhood starts from position 8 (halfway between 6 and 10) and ends at position 12.5 (halfway between 10 and 15).

The leftmost and rightmost villages have only one neighbor, so the definition of a neighborhood doesn't make sense for them. We'll ignore the neighborhoods of those two villages in this problem.

The *size* of a neighborhood is calculated as the neighborhood's rightmost position minus the neighborhood's leftmost position. For example, the neighborhood that goes from 8 to 12.5 has size $12.5 - 8 = 4.5$.

Determine the size of the smallest neighborhood.

### Input

The input consists of the following lines:

+   - A line containing integer $n$, the number of villages. $n$ is between 3 and 100.
+   - $n$ lines, each of which gives the position of a village. Each position is an integer between $-1,000,000,000$ and $1,000,000,000$. The positions need not come in order from left to right; the neighbor of a village could be anywhere in these lines.

### Output

Output the size of the smallest neighborhood. Include exactly one digit after the decimal point.

### Why Lists?

As part of reading the input, we'll need to read $n$ integers (the integers that represent the positions of the villages). We dealt with this once

already when solving Data Plan in Chapter 3. There, we used a range for loop to loop exactly *n* times. We’ll do that here, too.

There’s one crucial difference between Data Plan and Village Neighborhood. In Data Plan, we read an integer, used it, and never referred to it again. We didn’t need to keep it around. But in Village Neighborhood, it’s not enough to see each integer just once. A village’s neighborhood depends on its left and right neighbors. Without access to those neighbors, we can’t calculate the size of the village’s neighborhood. We need to store all of the village positions for later use.

For an example of why we need to store all of the village positions, consider this test case:

```
6
20
50
4
19
15
1
```

There are six villages here. To find the size of a village’s neighborhood, we need that village’s left and right neighbors.

The first village in the input is at position 20. What’s the size of that village’s neighborhood? To answer that, we need access to all of the village positions so that we can find its left and right neighbors. Scanning through the positions, you can identify that the left neighbor is at position 19 and the right neighbor is at position 50. The size of this village’s neighborhood is therefore (20 − 19)/2 + (50 − 20)/2 = 15.5.

The second village in the input is at position 50. What’s the size of that village’s neighborhood? Again, we need to look through the positions to figure it out. This village happens to be the rightmost one, so we ignore this village’s neighborhood.

The third village in the input is at position 4. The left neighbor is at position 1, and the right neighbor is at position 15, so the size of this village’s neighborhood is (4 − 1)/2 + (15 − 4)/2 = 7.

The fourth village in the input is at position 19. The left neighbor is at position 15, and the right neighbor is at position 20, so the size of this village’s neighborhood is (19 − 15)/2 + (20 − 19)/2 = 2.5.

The only remaining village that we need to consider is at position 15. If you calculate its neighborhood size, you should get an answer of 7.5.

Comparing all of the neighborhood sizes that we calculated, we see that the minimum—and the correct answer for this test case—is 2.5.

We need a way to store all of the village positions so that we can find the neighbors of each village. A string won’t help, because strings store characters, not integers. Python lists to the rescue!

## Lists

A *list* is a Python type that stores a sequence of values. (You’ll sometimes see list values referred to as *elements*.) We use opening and closing square brackets to delimit the list.

We can store only characters in strings, but we can store any type of value in lists. This list of integers holds the village positions from the prior section.

```
>>> [20, 50, 4, 19, 15, 1]
[20, 50, 4, 19, 15, 1]
```

Here’s a list of strings:

```
>>> ['one', 'two', 'hello']
['one', 'two', 'hello']
```

We can even create a list whose values are of different types:

```
>>> ['hello', 50, 365.25]
['hello', 50, 365.25]
```

Much of what you learned about strings applies to lists as well. For example, lists support the + operator for concatenation and the * operator for replication:

```
>>> [1, 2, 3] + [4, 5, 6]
[1, 2, 3, 4, 5, 6]
>>> [1, 2, 3] * 4
[1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3]
```

We even have the in operator, which tells us whether a value is in a list or not:

```
>>> 'one' in ['one', 'two', 'hello']
True
```

```
>>> 'n' in ['one', 'two', 'three']
False
```

And we have the len function to give us the length of a list:

```
>>> len(['one', 'two', 'hello'])
3
```

A list is a sequence, and we can use a for loop to loop through its values:

```
>>> for value in [20, 50, 4, 19, 15, 1]:
... print(value)
20
50
4
19
15
1
```

We can make variables refer to lists, just as we make them refer to strings, integers, and floats. Let’s make two variables refer to lists and then concatenate them to produce a new list.

```
>>> lst1 = [1, 2, 3]
>>> lst2 = [4, 5, 6]
>>> lst1 + lst2
[1, 2, 3, 4, 5, 6]
```

While we displayed the concatenated list, we did not store it, as we can see by looking at the lists again:

```
>>> lst1
[1, 2, 3]
>>> lst2
[4, 5, 6]
```

To make a variable refer to the concatenated list, we use assignment:

```
>>> lst3 = lst1 + lst2
>>> lst3
[1, 2, 3, 4, 5, 6]
```

Names like `lst`, `lst1`, and `lst2` can be used when there’s no need to be more specific about what a list contains.

But don’t use `list` itself as a variable name. It’s already a name that we can use to convert a sequence to a list:

```
>>> list('abcde')
['a', 'b', 'c', 'd', 'e']
```

If you make a variable named `list`, you’ll lose this valuable behavior, and you’ll confuse readers who will expect `list` not to be tampered with.

Finally, lists support indexing and slicing. Indexing returns a single value, and slicing returns a list of values:

```
>>> lst=[50, 30, 81, 40]
>>> lst[1]
30
>>> lst[-2]
81
>>> lst[1:3]
[30, 81]
```

If we have a list of strings, we can access one of its string’s characters by indexing twice, first to select a string and then to select a character:

```
>>> lst= ['one', 'two', 'hello']
>>> lst[2]
'hello'
>>> lst[2][1]
'e'
```

## CONCEPT CHECK

What does the following code store in the total variable?

```
lst= [a list of numbers]
total = 0
i = 1

while i <= len(lst):
```
