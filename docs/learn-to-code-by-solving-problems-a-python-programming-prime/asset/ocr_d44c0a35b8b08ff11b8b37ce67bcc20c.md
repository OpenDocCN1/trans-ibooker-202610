A pasture might be the favorite of more than one cow. But it’s guaranteed that a pasture is the favorite of no more than three cows.

Determine the grass type to use in each pasture. Each pasture is required to use a grass type between 1 and 4, and each cow’s two favorite pastures must have different grass types.

# Input

Read input from the file named revegetate.in.

The input consists of the following lines:

+   A line containing two integers separated by a space. The first integer is n, the number of pastures; it’s between 2 and 100. The second integer is m, the number of cows; it’s between 1 and 150.

+   m lines, each of which gives the two favorite pasture numbers for a cow. These pasture numbers are integers between 1 and n and are separated by a space.

# Output

Write output to the file named revegetate.out.

Output a valid way to seed the pastures. The output is a line of n characters, each of which is a '1', '2', '3', or '4'. The first character is the grass type for pasture 1, the second is the grass type for pasture 2, and so on.

We can interpret these n characters as an integer with n digits. For example, if we have the five grass types '11123', then we can interpret this as the integer 11123.

This integer interpretation comes into play when we have a choice of what to output. If there are multiple valid ways to seed the pastures, we must output the one that’s smallest when interpreted as an integer. For example, if both '11123' and '22123' are valid, we output the string '11123' because 11123 is less than 22123.

# Exploring a Test Case

We’re going to use top-down design to arrive at a solution for this problem. Working through a test case will help us sift the tasks.

Here’s the test case: