
each interval is a [start, end] list.
fired is the index of the lifeguard to fire.

Return the number of time units covered by all lifeguards except the one fired.

```
covered = set()
for i in range(len(intervals)):
    if i != fired:
        interval = intervals[i]
        for j in range(interval[0], interval[1]):
            covered.add(j)
return len(covered)
```

*Listing 9-1. Solving when one particular lifeguard is fired*

The first parameter is a list of lifeguard time intervals; the second is the index of the lifeguard to fire. Enter the code into the Python shell. Here are two sample calls of the function:

```
>>> num_covered([[5, 8], [10, 15], [9, 20], [17, 25]], 0)
16
>>> num_covered([[5, 8], [10, 15], [9, 20], [17, 25]], 1)
19
```

These calls confirm that we can cover 16 units of time if we fire lifeguard 0 and can cover 19 units of time if we fire lifeguard 1.

Now let’s understand how the function operates. We begin by creating a set that will hold the units of time that are covered. Whenever a unit of time is covered, the code will add the start of that unit of time to the set. For example, if the unit of time from 0 to 1 is covered, then the code will add 0 to the set; if the unit of time from 4 to 5 is covered, it will add 4 to the set.

We loop through the lifeguard time intervals. If a lifeguard isn’t fired, then we loop through this lifeguard’s time interval to consider each unit of covered time. We add each of these time units to the set, as promised. Recall that sets don’t retain duplicate values; we don’t have to worry if we try to add the same unit of time multiple times. We’ve gone through all of the nonfired lifeguards and added to the set all units of time that are covered. We therefore simply return the number of values in the set.

## The Main Program

The main part of our program is in Listing 9-2. It uses the num_covered function to determine the number of units of time that are covered when separately firing each lifeguard. Be sure to enter our num_covered function (Listing 9-1) before this code for a complete solution to the problem.

```
input_file = open('lifeguards.in', 'r')
output_file = open('lifeguards.out', 'w')

n = int(input_file.readline())

intervals = []

for i in range(n):
    ① interval = input_file.readline().split()
    interval[0] = int(interval[0])
    interval[1] = int(interval[1])
    intervals.append(interval)

max_covered = 0

② for fired in range(n):
    ③ result = num_covered(intervals, fired)
    if result > max_covered:
        max_covered = result

output_file.write(str(max_covered) + '\n')

input_file.close()
output_file.close()
```

Listing 9-2: Main program

We’re working with files here, not standard input and standard output.

The program begins by reading the number of lifeguards and then uses a range for loop to read each lifeguard’s time interval. We read each time interval from the input ①, convert each of its components to an integer, and append it as a two-value list to our list of intervals.

We use the max_covered variable to track the maximum number of time units that can be covered.

Now we separately fire each lifeguard using a range for loop ②. We call num_covered ③ to determine the number of time units that are covered given the firing of one lifeguard. We update max_covered whenever we’re able to cover a greater number of time units.

When that loop completes, we’ll have checked the number of units of time that can be covered by firing each lifeguard, and we’ll have remembered the maximum. We output this maximum to solve the problem.

Feel free to submit our code to the USACO judge. For Python code, this judge uses a time limit per test case of four seconds, but our solution shouldn’t come close to that limit. For example, I just ran the code here, and each test case finished in no more than 130 milliseconds.

## Efficiency of Our Program

The reason our code is so fast is because there are so few lifeguards—only at most 100 of them. If there were a large number of lifeguards, then our code would no longer solve the problem within the time limit. We’d be fine if there were a few hundred lifeguards. We might squeak through if we had as many as 3,000 or 4,000 lifeguards. Any more than that, though, and our code would be too slow. We probably couldn’t make it in time with 5,000 lifeguards, for example. We’d need to design a new algorithm, likely one that uses something faster than complete search.

You might think that 5,000 is a huge number of lifeguards and that it’s okay that our algorithm can’t go that high. But it’s not! Think back to the Email Addresses problem in Chapter 8. There, we had to contend with up to 100,000 email addresses. And think back to the Cities and States problem in the same chapter. There, we had to contend with up to 200,000 cities. By comparison, 5,000 is not a lot of lifeguards.

A complete-search solution often works fine for a small amount of input. Large test cases are often where complete-search solutions break down.

The reason that our complete-search solution for Lifeguards doesn’t work well with large test cases is because it does a lot of repeated work. Imagine that we’re solving a test case with 5,000 lifeguards. We’ll fire lifeguard 0 and call `num_covered` to determine the number of units of time covered by the remaining lifeguards. Then, we’ll fire lifeguard 1 and call `num_covered` again. Now, what `num_covered` does this time is similar to what it did on the previous call. After all, things haven’t changed much. The only change is that lifeguard 0 is back and lifeguard 1 is fired. The other 4,998 lifeguards are the same as they were! But `num_covered` doesn’t know that. It grinds through all of the lifeguards again. That same thing happens when we fire lifeguard 2, then lifeguard 3, and so on. Each time,

num_covered does all of its work from scratch, without learning anything about what it did previously.

Remember that, while useful, complete-search algorithms do have limitations. Given a new problem that we want to solve, a complete-search algorithm is a useful starting point, even if it ultimately turns out to be too inefficient. That’s because the act of designing that algorithm may deepen our appreciation of the problem and lead to new ideas for solving it.

In the next section, we’ll see another problem where we’ll be able to use complete search.

## CONCEPT CHECK

Is the following version of num_covered correct?

```
def num_covered(intervals, fired):
    """
    intervals is a list of lifeguard intervals;
    each interval is a [start, end] list.
    fired is the index of the lifeguard to fire.

    Return the number of time units covered by all lifeguards
    except the one fired.
    """
    covered = set()
    intervals.pop(fired)
    for interval in intervals:
        for j in range(interval[0], interval[1]):
            covered.add(j)
    return len(covered)
```

- A. Yes
- B. No

Answer: B. This function removes the fired lifeguard from the list of lifeguards. That’s not allowed, because the docstring doesn’t say anything about the function modifying the list. With this version of the function, our program will fail many test cases because

lifeguard information is lost over time. For example, when we test firing lifeguard 0, lifeguard 0 is removed from the list. When we later test firing lifeguard 1, lifeguard 0 is unfortunately still gone! If you want to use a version of the function where the fired lifeguard is removed from the list, you need to work with a copy of the list rather than the original.

## Problem #22: Ski Hills

Sometimes, the problem description makes it clear what we should search through in a complete-search solution. For example, in Lifeguards, we were asked to fire one lifeguard, so it made sense to try firing each one. Other times, we’ll have to be more creative to determine what to search through. As you read this next problem, think about what you would search in a complete-search solution.

This is USACO 2014 January Bronze Contest problem Ski Course Design.

## The Challenge

Farmer John has n hills on his farm, each with a height between 0 and 100. He would like to register his farm as a ski training camp.

A farm can be registered as a ski training camp only if the difference in height between the highest and lowest hills is 17 or less. Farmer John may therefore need to increase the heights of some of his hills and decrease the heights of others. He is able to change the heights only by integer amounts.

The cost of changing a hill’s height by x units is x². For example, changing a hill from height 1 to height 4 costs (4 − 1)² = 9.

Determine the minimum amount that Farmer John will need to pay to change the heights of hills so that he can register his farm as a ski training camp.

## Input

Read input from the file named skidesign.in.

The input consists of the following lines:
