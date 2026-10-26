The following figure demonstrates the distance between different musical notes when represented as musical notations:

# Musical Intervals

![](bbox=[0.069, 0.215, 0.917, 0.414])

You are a music theorist who is curious about how many times a permutation of a particular note set appears within the melodies of various composers. Given the notes of a complete melody and a set of notes, count the number of times any permutation of the note set appears within the melody. For any valid permutation, notes can be repeated any amount of times and can occur in any order:

```
        0     1     2     3     4     5     6

Melody:    { "A", "B", "C", "C", "E", "C", "A" }
Note set:  { "A", "C", "E" }
```

Subsets:

+   - `{ 0, 2, 4 }` → `{ "A", "C", "E" }`
+   - `{ 0, 3, 4 }` → `{ "A", "C", "E" }`
+   - `{ 0, 4, 5 }` → `{ "A", "E", "C" }`
+   - `{ 2, 4, 6 }` → `{ "C", "E", "A" }`
+   - `{ 3, 4, 6 }` → `{ "C", "E", "A" }`
+   - `{ 4, 5, 6 }` → `{ "E", "C", "A" }`
+   - `{ 0, 2, 3, 4 }` → `{ "A", "C", "C", "E" }`