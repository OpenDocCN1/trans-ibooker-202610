```
output = "".join(pasture_types_str)
output_file.write(output + '\n')

# Main Program

input_file = open('revegetate.in', 'r')
output_file = open('revegetate.out', 'w')

# Read input
lst = input_file.readline().split()
num_pastures = int(lst[0])
num_cows = int(lst[1])
favorites = read_cows(input_file, num_cows)

pasture_types = [0]

for i in range(1, num_pastures + 1):

    # Identify cows that care about pasture
    cows = cows_with_favorite(favorites, i)

    # Eliminate grass types for pasture
    eliminated = types_used(favorites, cows, pasture_types)

    # Choose smallest-numbered grass type for pasture
    pasture_type = smallest_available(eliminated)
    pasture_types.append(pasture_type)

# Write output
pasture_types.pop(0)
write_pastures(output_file, pasture_types)

input_file.close()
output_file.close()
```

Listing 7-2: Solving Farm Seeding

We did it! An intimidating problem, made more manageable by the application of top-down design. Feel free to submit our work to the USACO judge.

It’s easy to be overwhelmed by a problem when you first read it. But remember that you don’t need to tackle it in one huge step. Break it down, solve each task that you can solve, and you’ll be well on your way to a solution for the overall problem. You’ve made huge strides in the amount of Python that you know and your ability to design programs and solve problems. Solving these problems is within your grasp!