```
```python
# Sort boxes
endpoints.sort()

# Determine whether boxes are organized
if all_endpoints_ok(endpoints):
    print('YES')
else:
    print('NO')
```
```

# Listing 6-2: Solving Action Figures

This is the largest program that we’ve written to this point in the book. But look how tidy and minimal the main part of the program is: it’s mostly calls to functions, with a little bit of if-else logic to glue them together.

We’re calling each function only once here. Compare that to the `no_high` Card Game function that we called four times. Even if a function is called only once, it still contributes to organized, readable code.

Time to submit to the Timus judge. You should see that all test cases pass.

# CONCEPT CHECK

In Task 2, we wrote function `box_ok` for determining whether a single box has its heights in order. It uses a range for loop. Is the following while loop version of `box_ok` correct?

```
```python
def box_ok(box):
    """
    box is the list of action figure heights in a given box.

    Return True if the heights in box are in nondecreasing order,
    False otherwise.
    """
    ok = True
    i = 0
    while i < len(box) - 1 and ok:
        if box[i] > box[i + 1]:
            ok = False
        i = i + 1
    return ok
```
```