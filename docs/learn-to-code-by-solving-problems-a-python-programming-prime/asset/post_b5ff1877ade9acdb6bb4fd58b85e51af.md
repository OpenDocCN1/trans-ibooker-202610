
Here’s an example:

```python
>>> lst1 = [1, 2, 3]
>>> lst2 = [4, 5, 6]
>>> lst1.extend(lst2)
>>> lst1
[1, 2, 3, 4, 5, 6]
>>> lst2
[4, 5, 6]
```

If you want to insert into a list at a position other than its end, you can use the insert method. It takes an index and a value and inserts the value at the index:

```python
>>> lst = [10, 20, 30, 40]
>>> lst.insert(1, 99)
>>> lst
[10, 99, 20, 30, 40]
```

## Sorting a List

The sort method *sorts* a list, putting its values in order. If we call it with no arguments, it sorts from smallest to largest:

```python
>>> positions = [20, 50, 4, 19, 15, 1]
>>> positions.sort()
>>> positions
[1, 4, 15, 19, 20, 50]
```

If we call it with a reverse argument of value True, it sorts from largest to smallest:

```python
>>> positions.sort(reverse=True)
>>> positions
[50, 20, 19, 15, 4, 1]
```

The syntax that I’ve used, reverse=True, is new. Based on how we’ve called methods and functions to this point in the book, you might expect that True by itself would work. But no: sort requires the whole reverse=True to be there, for reasons I’ll explain in Chapter 6.

## Removing Values from a List

The pop method removes a value by index. If no argument is provided, pop both removes and returns the rightmost value.

```python
>>> lst = [50, 30, 81, 40]
>>> lst.pop()
40
```

We can pass the index of the value to remove as an argument to pop. Here, we remove and return the value at index 0:

```python
>>> lst.pop(0)
50
```

Since pop returns something—unlike methods like append and sort—it makes sense to assign its return value to a variable:

```python
>>> lst
[30, 81]
>>> value = lst.pop()
>>> value
81
>>> lst
[30]
```

The remove method removes by value, not index. Pass it the value to remove, and it removes the leftmost occurrence of that value from the list. If the value is not present, remove produces an error. In the following, there are two occurrences of 50 in the list, so remove(50) works twice before producing an error:

```python
>>> lst = [50, 30, 81, 40, 50]
>>> lst.remove(50)
>>> lst
[30, 81, 40, 50]
>>> lst.remove(50)
>>> lst
[30, 81, 40]
>>> lst.remove(50)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: list.remove(x): x not in list
```

## CONCEPT CHECK

What is the value of lst after the following code runs?

```python
lst = [2, 4, 6, 8]
lst.remove(4)
lst.pop(2)
```

- A. [2, 4]
- B. [6, 8]
- C. [2, 6]
- D. [2, 8]
- E. This code produces an error

Answer: C. The remove call removes the value 4, leaving [2, 6, 8]. Now the pop call removes the value at index 2, which is value 8. That leaves a final list of [2, 6].

## Solving the Problem

Suppose that we’ve successfully read and sorted the village positions. Here’s what our list would look like at that point:

```python
>>> positions = [1, 4, 15, 19, 20, 50]
>>> positions
[1, 4, 15, 19, 20, 50]
```

To find the size of the smallest neighborhood, we start by finding the size of the neighborhood for the village at index 1. (Notice that we don’t start at index 0: the village at index 0 is the leftmost one, and per the problem description, we can ignore it.) We can find that neighborhood size like this:

```python
>>> left = (positions[1] - positions[0]) / 2
>>> right = (positions[2] - positions[1]) / 2
>>> min_size = left + right
>>> min_size
7.0
```

The left variable stores the size of the left part of the neighborhood, and right stores the size of the right part. We then add them up to obtain the total size of the neighborhood. We get a value of 7.0.

That’s the value to beat. How do we know whether any other village has a smaller neighborhood? We can use a loop to process those other villages. If we find a neighborhood that’s smaller than our current smallest, we update our current smallest to that smaller size.

The code for our solution is in Listing 5-1.

```python
n = int(input())

❶ positions = []

❷ for i in range(n):
    ❸ positions.append(int(input()))

❹ positions.sort()

❺ left = (positions[1] - positions[0]) / 2
right = (positions[2] - positions[1]) / 2
min_size = left + right

❻ for i in range(2, n - 1):
    left = (positions[i] - positions[i - 1]) / 2
    right = (positions[i + 1] - positions[i]) / 2
    size = left + right
    ❼ if size < min_size:
        min_size = size

print(min_size)
```

*Listing 5-1: Solving Village Neighborhood*

We begin by reading n, the number of villages, from the input. We also set positions to refer to an empty list ❶.

Each iteration of the first range for loop ❷ is responsible for reading one village position and appending it to the positions list. It does that by using input to read the next village position, int to convert it to an integer, and the list method append to append that integer to the list ❸. That one line ❸ is equivalent to these three separate lines:

```python
position = input()
position = int(position)
positions.append(position)
```

Having read the village positions, we next sort them in increasing order ❹. We then find the size of the neighborhood of the village at index 1, storing it using min_size ❺.

Next, in a second loop, we loop through each of the other villages whose neighborhood sizes we need to compute ❻. Those villages start at index 2 and end at index n - 2. (We don’t want to consider the village at index n - 1, because that’s the rightmost village.) We therefore use range with a first argument of 2 (thus starting at 2) and a second argument of n - 1 (thus ending at n - 2).

Inside the loop, we calculate the size of the current village’s neighborhood, exactly as we did for the first village. The size of the smallest neighborhood that we’ve found so far is referred to by min_size. Is the current village’s neighborhood smaller than our smallest so far? To answer that, we use an if statement ❼. If this village’s neighborhood is smaller than min_size, we update min_size to the size of this neighborhood. If this village’s neighborhood isn’t smaller than min_size, then we do nothing, because this village doesn’t change the size of the smallest neighborhood.

Having gone through all of the villages, min_size must be the size of the smallest neighborhood. We therefore output the value of min_size.

The “Output” section of this problem description specified “Include exactly one digit after the decimal point.” What if the smallest size was something like 6.25 or 8.33333? Shouldn’t we do something about that?

No. We’re safe with what we’ve done. The only neighborhood sizes we can get are numbers like 3.0 (with a 0 after the decimal point) and 3.5 (with a .5 after the decimal point). Here’s why. When we calculate the left part of a neighborhood, we subtract two integers and divide that resulting integer by 2. If we have an even integer before dividing by 2, then dividing gives us a .0 number (no remainder). And if we have an odd integer before dividing by 2, then dividing gives us a .5 number. The same goes for the right part of the neighborhood: the size will be a .0 number or a .5 number. Adding the left and right parts to get the total size is therefore guaranteed to give us another .0 or .5 number.

## Avoiding Code Duplication: Two More Solutions

It’s a little disappointing that we’re including the “compute neighborhood size” code both prior to and in the second range for loop.

In general, repeated code is a sign that we might be able to improve the code’s design. We’d like to avoid repeated code because it adds to the amount of code that we must maintain, and it makes it harder to fix problems in the code if it turns out that the repeated code is flawed. Here, the repeated code seems acceptable to me (it’s only three lines), but let’s talk about two ways to avoid it. These are general approaches that you’ll be able to apply to other similar problems.

### Using a Huge Size

The only reason we’re calculating the size of a village’s neighborhood before the loop is so that the loop has something to compare the other neighborhood sizes against. If we entered the loop without a value for `min_size`, we’d get an error when the code tries to compare it to the size of the current village.

If we set `min_size` to 0.0 before the loop, then the loop will never find a smaller size, and we’ll incorrectly output 0.0 no matter the test case. Using 0.0 would be a bug!

But a huge value, one at least as big as every possible neighborhood size, *will* work. We just need to make it so huge that the first iteration of the loop is guaranteed to find a size that’s no bigger, ensuring that our fake huge size never gets output.

From the “Input” section of this problem description, we know that each position is between −1,000,000,000 and 1,000,000,000. The biggest neighborhood we could ever have, then, occurs when we have a village at position −1,000,000,000, another at position 1,000,000,000, and a village somewhere in between. That in-between village will have a neighborhood size of 1,000,000,000. We can therefore start `min_size` with a size of 1000000000.0 or greater. This alternate approach is in Listing 5-2.

```python
n = int(input())

positions = []

for i in range(n):
    positions.append(int(input()))

positions.sort()

min_size = 1000000000.0

for i in range(1, n - 1):
    left = (positions[i] - positions[i - 1]) / 2
```
