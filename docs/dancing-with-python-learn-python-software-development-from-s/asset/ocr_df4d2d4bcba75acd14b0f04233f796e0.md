# 3.2.8 Extending lists

How can we add items to an existing list? There are several things we might like to do:

+   1. Append an item to the end of our list.
+   2. Prepend an item at the beginning of our list.
+   3. Insert an item at a specific index for our list.
+   4. Extend our list at the end with items from another list.
+   5. Add items from another list to the beginning of our list.
+   6. Add items from another list at a specific index for our list.

The first four are easy, but the last two require more work on your part. We again work with guitar brands.

```
brands = [
    "Fender",
    "Gibson",
    "Ibanez"
]

original_brands = brands.copy()
```

To add an item at the end of our list, use *append*.

```
brands.append("Gretsch")
brands
```

['Fender', 'Gibson', 'Ibanez', 'Gretsch']

To add an item at the beginning of our list, use *insert* with index 0.

```
brands = original_brands.copy()
brands.insert(0, "Gretsch")
brands
```

['Gretsch', 'Fender', 'Gibson', 'Ibanez']

More generally, use the index where you want to place the new item with *insert*.

```
brands = original_brands.copy()
brands.insert(1, "Gretsch")
brands
```

['Fender', 'Gretsch', 'Gibson', 'Ibanez']