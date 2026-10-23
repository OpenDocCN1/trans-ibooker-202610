We’ll be searching for a value that we know is in the list, so we won’t have to worry about getting an error.

# Solving the Problem

We’re there! We can now update our partial solution to handle any valid test case. The new program is in Listing 5-5.

```
YEAR_COSTS = [12, 10, 7, 5]

for dataset in range(10):
    trip_cost = int(input())
    proportions = input().split()
    num_students = int(input())

    for i in range(len(proportions)):
        proportions[i] = float(proportions[i])

    students_per_year = []

    for proportion in proportions:
        students = int(num_students * proportion)
        students_per_year.append(students)

① counted = sum(students_per_year)
    uncounted = num_students - counted
    most = max(students_per_year)
    where = students_per_year.index(most)
② students_per_year[where] = students_per_year[where] + uncounted

    total_raised = 0

    for i in range(len(students_per_year)):
        total_raised = total_raised + students_per_year[i] * YEAR_COSTS[i]

    if total_raised / 2 < trip_cost:
        print('YES')
    else:
        print('NO')
```

Listing 5-5: Solving School Trip

The only new code is the five lines starting at ①. We use sum to calculate how many students we’ve counted so far and then subtract this from the total number of students to arrive at the number of uncounted students. We then use max and index to identify the index of the year to