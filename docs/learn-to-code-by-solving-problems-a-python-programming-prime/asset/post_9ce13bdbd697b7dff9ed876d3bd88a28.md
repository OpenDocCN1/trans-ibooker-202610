
we’d have to be careful with our choice for pasture 1. We wouldn’t want to give some cow two pastures with the same grass type, because that would break the rules! We haven’t chosen any grass types yet, so nothing can go wrong no matter what we choose for pasture 1. Since we want the smallest grass types, though, we’ll choose grass type 1.

I’ll collect our grass-type decisions in tables. Here’s the decision we just made, grass type 1 for pasture 1:

| Pasture | Grass type |
|---|---|
| 1 | 1 |

Let’s move on. What grass type should we choose for pasture 2? The cows that care about pasture 2 are cows 1, 4, and 5, so we focus on those. One of cow 4’s pastures is pasture 1, and we chose grass type 1 for that pasture, so grass type 1 is eliminated as a grass type for pasture 2. If we used grass type 1 for pasture 2, then we’d be giving cow 4 two pastures with the same grass type, and that would break the rules. Cows 1 and 5, however, don’t eliminate any other grass types, because we haven’t chosen grass types for their pastures yet. We therefore choose grass type 2, the smallest-numbered grass type that’s available. Here’s where we stand:

| Pasture | Grass type |
|---|---|
| 1 | 1 |
| 2 | 2 |

What grass type should we choose for pasture 3? The only cow that cares about pasture 3 is cow 2. Cow 2’s pastures are pastures 3 and 5. That cow doesn’t eliminate any grass types, however, because we haven’t assigned a grass type to pasture 5! To get the smallest number, we’ll use grass type 1 for pasture 3. Here’s our next snapshot:

| Pasture | Grass type |
|---|---|
| 1 | 1 |
| 2 | 2 |

| Pasture | Grass type |
|---|---|
| 3 | 1 |

I can see three tasks in our top-down design crystallizing here. First, we need to obtain the cows that care about the current pasture. Second, we need to determine which grass types those cows eliminate from consideration. Third, we need to choose the smallest-numbered grass type that wasn’t eliminated. Each of those is a prime candidate for a function.

Let’s keep going. We have three cows that care about pasture 4: cows 0, 1, and 3. Cow 0 doesn’t eliminate any grass types, because we haven’t assigned grass types to its pastures yet. Cow 1 eliminates grass type 2 because we assigned grass type 2 to pasture 2 (its other pasture). And cow 3 eliminates grass type 1 because we assigned grass type 1 to pasture 1 (its other pasture). The smallest available grass type, then, is 3, so that’s what we use for pasture 4:

| Pasture | Grass type |
|---|---|
| 1 | 1 |
| 2 | 2 |
| 3 | 1 |
| 4 | 3 |

On to pasture 5. The cows that care about pasture 5 are cows 0, 2, and 5. Cow 0 eliminates grass type 3; cow 2 eliminates grass type 1; and cow 5 eliminates grass type 2. So grass types 1, 2, and 3 are out. Our only choice is grass type 4.

That was close! We almost ran out of grass types there. Lucky for us, there was no other cow that cared about pasture 5 and eliminated grass type 4.

Or, wait. This wasn’t luck at all, because of this bit from the problem description: “It’s guaranteed that a pasture is the favorite of no more than three cows.” This means that at most three grass types can be eliminated for each pasture. We’ll never be stuck! And we don’t even have to worry about the ramifications of past choices on our next decision. No matter

what we did in the past, we’ll always have at least one available grass type.

Let’s add pasture 5 to our table:

| Pasture | Grass type |
|---|---|
| 1 | 1 |
| 2 | 2 |
| 3 | 1 |
| 4 | 3 |
| 5 | 4 |

There are three pastures to go. But no cow cares about any of them, so we can just use grass type 1 in each case. That gives us this:

| Pasture | Grass type |
|---|---|
| 1 | 1 |
| 2 | 2 |
| 3 | 1 |
| 4 | 3 |
| 5 | 4 |
| 6 | 1 |
| 7 | 1 |
| 8 | 1 |

We can read the grass types from top to bottom to obtain the correct output for this example. The output is as follows:

12134111

## Top-Down Design

With a good understanding of the tasks that we’ll need to complete, we’ll turn to a top-down design of this problem.

## The Top Level

We discovered three tasks in the previous section as we worked through a test case. Before our program can solve any of those tasks, we need to read the input, so that’s a fourth task. We also need to write the output. That will take some thought and a few lines of code, so let’s call that our fifth task.

Here are our five main tasks:

- 1. Read input.
- 2. Identify cows that care about current pasture.
- 3. Eliminate grass types for current pasture.
- 4. Choose smallest-numbered grass type for current pasture.
- 5. Write output.

As we did when solving Action Figures in Chapter 6, we’ll start with a framework of TODO comments and remove each TODO as we solve it.

We begin with mostly comments. Since we’ll need to open the files at the start and close them at the end, I’ve also added that code.

Here’s where we begin:

```
# Main Program

input_file = open('revegetate.in', 'r')
output_file = open('revegetate.out', 'w')

# TODO: Read input

# TODO: Identify cows that care about pasture

# TODO: Eliminate grass types for pasture

# TODO: Choose smallest-numbered grass type for pasture

# TODO: Write output

input_file.close()
output_file.close()
```

## Task 1: Read Input

Reading the first line of input, with the integers n and m, is something we know how to do. It’s straightforward enough that I don’t think we need a function for it, so let’s do it directly. Next we need to read the pasture information for the m cows, and here a function seems warranted. Let’s remove the TODO in the Read input comment, handle the first line of input, and call the read_cows function, which we’ll write shortly:

```
# Main Program

input_file = open('revegetate.in', 'r')
output_file = open('revegetate.out', 'w')

# Read input
lst = input_file.readline().split()
num_pastures = int(lst[0])
num_cows = int(lst[1])
① favorites = read_cows(input_file, num_cows)

# TODO: Identify cows that care about pasture

# TODO: Eliminate grass types for pasture

# TODO: Choose smallest-numbered grass type for pasture

# TODO: Write output

input_file.close()
output_file.close()
```

The read_cows function that we’re calling ① will take a file that’s already open for reading, and read the two favorite pastures for each cow. It’ll return a list of lists, where each inner list contains the two pasture numbers for a given cow. Here’s the code:

```
def read_cows(input_file, num_cows):
    """
    input_file is a file open for reading; cow information is next to read.
    num_cows is the number of cows in the file.

    Read the cows' favorite pastures from input_file.
    Return a list of each cow's two favorite pastures;
    each value in the list is a list of two values giving the
    favorite pastures for one cow.
    """
    favorites = []
```

```
for i in range(num_cows):
    ① lst = input_file.readline().split()
    lst[0] = int(lst[0])
    lst[1] = int(lst[1])
    ② favorites.append(lst)
return favorites
```

This function accumulates the cows’ favorite pastures into the favorites list. It does that using a range for loop that loops num_cows times, once for each cow. We need the loop because the number of lines to read depends on the number of cows in the file.

On each iteration of the loop, we read the next line and split it into its two components ①. We then use int to convert the components from strings to integers. When we append this list to favorites ②, we’re therefore appending a list of two integers.

The last thing we do is return the list of favorite pastures.

Before we continue, let’s make sure that we know how to call this function. We’ll practice calling it on its own, independent from the larger program that we’re building. It’s useful to test functions like this so that we can fix any errors that we might find along the way.

Use your text editor to create a file named revegetate.in with the following contents (the same as the test case we studied earlier):

```
8 6
5 4
2 4
3 5
4 1
2 1
5 2
```

Now, in the Python shell, enter the code for our read_cows function.
Here’s what we do to call read_cows:

```
>>> input_file = open('revegetate.in', 'r')
① >>> input_file.readline()
'8 6\n'
② >>> read_cows(input_file, 6)
[[5, 4], [2, 4], [3, 5], [4, 1], [2, 1], [5, 2]]
```

The read_cows function reads only the information for the cows. Since we’re testing this function in isolation, outside of our program, we need
