the special pairs involving MAMI cities, we need to know the cities that start with MI and have state MA. That is, we need to know the number of MIMA cities. There are three MIMA cities. They happen to be MIDDLEBOROUGH MA, MILFORD MA, and MIDDLETON MA, but all we care about is that there are three of them. Okay—so we have two MAMI cities and three MIMA cities. The total special pairs for these combos is therefore 2 * 3 = 6, because for each of the two MAMI cities, we have a choice of three MIMA cities.

If you’re not convinced, here are the six special pairs for these combos:

+   - MANISTEE MI and MIDDLEBOROUGH MA
+   - MANISTEE MI and MILFORD MA
+   - MANISTEE MI and MIDDLETON MA
+   - MADISON MI and MIDDLEBOROUGH MA
+   - MADISON MI and MILFORD MA
+   - MADISON MI and MIDDLETON MA

If we could map combos—SCPA, PASC, MAMI, MIMA, and so on—to the number of occurrences, we could loop through the combos to find the number of special pairs of cities. A dictionary is the perfect tool to store this mapping.

Here’s the dictionary that we’d like to create for our test case:

```
{'SCPA': 1, 'MAMI': 2, 'NANH': 1, 'PASC': 1, 'LACO': 2,
'MIMA': 3, 'COLA': 1}
```

With this dictionary, we can figure out the number of special pairs of cities. Let’s work through the process.

The first key is 'SCPA'; its value is 1. To find special pairs of cities involving 'SCPA', we need to look up the value for 'PASC'. That value is also 1. We multiply the two values together, yielding 1 * 1 = 1 special pair of cities involving these combos. We need to carry out this same procedure for each other key in the dictionary.

The next key is 'MAMI'; its value is 2. To find special pairs of cities involving 'MAMI', we need to look up the value for 'MIMA'. That value is 3. We multiply the two values together, yielding 2 * 3 = 6 special pairs of cities involving these combos. With the 1 we found previously, we now have a total of 7.