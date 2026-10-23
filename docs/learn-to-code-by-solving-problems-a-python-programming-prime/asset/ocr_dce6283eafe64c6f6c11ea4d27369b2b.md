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