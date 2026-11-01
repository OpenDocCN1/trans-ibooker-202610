`value in m` An O(1) operation for dictionaries, which is much faster than using `in` with sequences.
`for key in m:` An O(n) operation.
`len(m)` An O(1) operation, because Python keeps track of how many items are in a mapping, so it doesn't need to recount them when passed to `len()`.

Although a list generally has to search through its items from start to finish, dictionaries use the key to calculate the address, and the time needed to look up the key's value remains constant. This calculation is called a *hashing algorithm*, and the address is called a *hash*. Hashing is beyond the scope of this book, but it's the reason so many of the mapping operations are O(1) constant time. Sets also use hashing, because sets are essentially dictionaries with keys only instead of key-value pairs.

But keep in mind that converting a list to a set is an O(n) operation, so you don't achieve any benefit by converting a list to a set and then accessing the items in the set.

# Analyzing Big O at a Glance

Once you've become familiar with performing big O analysis, you usually won't need to run through each of the steps. After a while you'll be able to just look for some telltale features in the code to quickly determine the big O order.

Keeping in mind that *n* is the size of the data the code operates on, here are some general rules you can use:

+   - If the code doesn't access any of the data, it's O(1).
+   - If the code loops over the data, it's O(n).
+   - If the code has two nested loops that each iterate over the data, it's O(n²).
+   - Function calls don't count as one step but rather the total steps of the code inside the function. See “Big O Order of Common Function Calls” on page 242.
+   - If the code has a *divide and conquer* step that repeatedly halves the data, it's O(log n).
+   - If the code has a *divide and conquer* step that is done once per item in the data, it's an O(n log n).
+   - If the code goes through every possible combination of values in the *n* data, it's O(2ⁿ), or some other exponential order.
+   - If the code goes through every possible permutation (that is, ordering) of the values in the data, it's O(n!).
+   - If the code involves sorting the data, it will be at least O(n log n).

These rules are good starting points. But they're no substitute for actual big O analysis. Keep in mind that big O order isn't the final