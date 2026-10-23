```
right = (positions[i + 1] - positions[i]) / 2
size = left + right
if size < min_size:
    min_size = size

print(min_size)
```

# Listing 5-2: Solving Village Neighborhood with a huge value

Careful! We need to start computing sizes at index 1 now ❶ (not 2); otherwise, we’d forget to include the neighborhood of the village at index 1.

# Building a List of Sizes

Another way to avoid the code duplication is to store each neighborhood size in a list of sizes. Python has a built-in min function that takes a sequence and returns its minimum:

```
>>> min('qwerty')
'e'
>>> min([15.5, 7.0, 2.5, 7.5])
2.5
```

(Python also has a max function that returns the maximum of a sequence.)

See Listing 5-3 for a solution that uses min on a list of neighborhood sizes.

```
n = int(input())

positions = []

for i in range(n):
    positions.append(int(input()))

positions.sort()

sizes = []

for i in range(1, n - 1):
    left = (positions[i] - positions[i - 1]) / 2
    right = (positions[i + 1] - positions[i]) / 2
    size = left + right
    sizes.append(size)

min_size = min(sizes)
print(min_size)
```