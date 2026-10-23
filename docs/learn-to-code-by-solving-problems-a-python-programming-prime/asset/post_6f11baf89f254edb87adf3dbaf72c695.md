
```
...   for num2 in lst:
...     print(num1, num2)
...
1 1
1 9
9 1
9 9
```

We can similarly loop through all triples of values using three nested loops, like this:

```
>>> for num1 in lst:
...   for num2 in lst:
...     for num3 in lst:
...       print(num1, num2, num3)
...
1 1 1
1 1 9
1 9 1
1 9 9
9 1 1
9 1 9
9 9 1
9 9 9
```

Using three nested loops like this gives us a starting point for solving the Cow Baseball problem. For each triple, we can check whether it matches Farmer John’s observations. See Listing 9-5 for the code.

```
input_file = open('baseball.in', 'r')
output_file = open('baseball.out', 'w')

n = int(input_file.readline())

positions = []

for i in range(n):
  ① positions.append(int(input_file.readline()))

total = 0

② for position1 in positions:
   ③ for position2 in positions:
      first_two_diff = position2 - position1
   ④ if first_two_diff > 0:
      low = position2 + first_two_diff
      high = position2 + first_two_diff * 2
```

```
⑤ for position3 in positions:
    if position3 >= low and position3 <= high:
        total = total + 1

output_file.write(str(total) + '\n')

input_file.close()
output_file.close()
```

## Listing 9-5: Using three nested for loops

We read all of the cow positions into the positions list ①. We then loop over all positions in the list using a for loop ②. For each of these positions, we loop through all positions in the list using a nested for loop ③. At this point, position1 and position2 refer to two positions from the list. We need a third nested loop, yes, but not yet. We first need to calculate the difference between position1 and position2 because that tells us the range of position3s that we’ll be looking for.

We require from the problem description that position2 be on the right of position1. If it is ④, then we calculate the low end and high end of the range for position3 and store them using low and high, respectively. For example, if position1 is 1 and position2 is 6, then we’ll calculate 6 + 5 = 11 for low and 6 + 5 * 2 = 16 for high. Then we loop through the list with a third nested for loop ⑤, looking for positions that are between low and high. For each such position3, we increase our total by 1.

Following the three nested loops, we have calculated the total number of triples. We finish up by outputting that number to the output file.

Let’s try our program on a small test case to make sure nothing weird is happening. Here it is:

```
7
16
14
23
18
1
6
11
```

The correct answer for this test case is 11. The 11 satisfying triples are as follows:

- 14, 16, 18
- 14, 18, 23
- 1, 6, 16
- 1, 6, 14
- 1, 6, 11
- 1, 11, 23
- 6, 14, 23
- 6, 11, 16
- 6, 11, 18
- 11, 16, 23
- 11, 14, 18

Good news: our program outputs 11 for this test case! It does so because it eventually finds each satisfying triple. For example, at some point, position1 will be 14, position2 will be 16, and position3 will be 18. That triple satisfies the distance requirements, so our program will count it in our total. Don’t be worried about what will happen, later, when position1 is 18, position2 is 16, and position3 is 14. We definitely don’t want to count that one, because these throws are not going to the right. We’re fine, though: the if statement ④ prevents these triples from being processed.

Our program is correct. But as you’ll see if you submit it to the judge, it is not efficient enough. For this problem, and many competitive programming problems, the first few test cases are small—just a few cows, a few lifeguards, or a few ski hills. Our program should be able to solve those in time. The remaining test cases test our program closer and closer to the limit of acceptable input. Our program does not solve those in time. It’s too slow.

## Efficiency of Our Program

To understand why our program is so slow, it helps to think about the number of triples that it must go through. Think back to the test case we just studied, which had seven cows. How many triples will our program check? Well, for the first cow, there are seven choices: 16, 14, 23, and so on. There are also seven choices for the second cow, and seven choices for the third cow. Multiplying these together, we see that our program checks 7 * 7 * 7 = 343 triples.

What if we had eight cows instead of seven? Then our program would check 8 * 8 * 8 = 512 triples.

We can give an expression for the number of triples that works for any number of cows. Let’s use n for the number of cows; it could be 7, 8, 50, 1,000, and so on, depending on the test case. Then we can say that the number of triples our program checks is n * n * n, or n³.

We can substitute any number of cows for n to determine the number of triples that we check. For example, we can verify that the number of triples for seven cows is 7³ = 343 and that the number of triples for eight cows is 8³ = 512. These numbers—343 and 512—are tiny. It would take any computer no more than a few milliseconds to check those many triples. As a conservative guide, you can think of a Python program as being able to check or do about 5,000,000 things per second. The time limit for this problem is four seconds per test case, so we’ll be able to check about 20,000,000 triples.

Let’s substitute larger numbers for n and see what happens. For 50 cows, we have 50³ = 125,000 triples. No big deal: checking 125,000 things is easy for today’s computers. For 100 cows, we have 100³ = 1,000,000 triples. Again, no problem. We can check a million things in less than a second. For 200 cows, we have 200³ = 8,000,000 triples. We’re still OK for four seconds, but I hope you’re starting to get a little worried. The number of triples is shooting up pretty quickly here, and we’re only at 200 cows. Remember that we need to be able to support up to 1,000 cows.

For 400 cows, we have 400³ = 64,000,000 triples. That’s too many for us to process in four seconds. To add insult to injury, let’s try 1,000 cows, the maximum we’ll ever get. For 1,000 cows, we have 1,000³ = 1,000,000,000 triples. That’s one billion. Nope. There’s no way we’re ever going to be able to check that many triples in four seconds. We’ll need to make our program more efficient.

## Sorting First

Sorting is helpful here. Let’s look at how to use sorting and then discuss the efficiency of our resulting solution.

## The Code

Our cow positions can come in any order—there’s certainly no guarantee from the problem description that they’re sorted. Unfortunately, this leads our program to check many triples that have no chance of satisfying the requirements. For example, checking the triple 18, 16, 14 is pointless, because the numbers aren’t in increasing order. If we sorted the cow positions at the outset, then we could avoid ever checking these out-of-order triples.

There’s another benefit to sorting. Suppose that position1 refers to some cow position and position2 refers to another. For this pair of positions, we know the smallest value of position3 and largest value of position3 that we care about. We can use the fact that the positions are sorted to cut down on the number of values that we need to check for this range. Before continuing, think about why this is the case. How can we use the fact that the positions are sorted to look at fewer values?

When you’re ready, see Listing 9-6 for our code that uses sorting.

```
input_file = open('baseball.in', 'r')
output_file = open('baseball.out', 'w')

n = int(input_file.readline())

positions = []

for i in range(n):
    positions.append(int(input_file.readline()))

positions.sort()

total = 0

for i in range(n):
    for j in range(i + 1, n):
        first_two_diff = positions[j] - positions[i]
        low = positions[j] + first_two_diff
        high = positions[j] + first_two_diff * 2

        left = j + 1
        while left < n and positions[left] < low:
            left = left + 1

        right = left
        while right < n and positions[right] <= high:
            right = right + 1

        total = total + right - left
```
