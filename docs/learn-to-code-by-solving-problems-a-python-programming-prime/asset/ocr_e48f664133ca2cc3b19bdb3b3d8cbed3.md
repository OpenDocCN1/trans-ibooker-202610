# Task 1: Read Input

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