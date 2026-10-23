# CONCEPT CHECK

What is the output of the following code?

```
game = 'Lost Vikings'

print(game[2:-6])
```

+   A. st V
+   B. ost V
+   C. iking
+   D. st Vi
+   E. Viking

Answer: A. The character at index 2 is the 's' in 'Lost'. The character at index -6 is the first 'i' in 'Vikings'. Since we go from index 2 up to but not including index -6, we get the slice 'st V'.

# CONCEPT CHECK

Which password gets us out of the following loop?

```
valid = False

while not valid:
    s = input()
    valid = len(s) == 5 and s[:2] == 'xy'
```

+   A. xyz
+   B. xyabc
+   C. abcxy