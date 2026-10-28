```
for j in range(len(string)):
    if have_a_match(j):
        return (j, len(substring))

return None
```

I show the characters of string and their indices in Figure 12.1.

| E | c | o | n | o | m | i | c | , |   | e | c | o | l | o |
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
| g | i | c | a | l |   | e | c | o | s | y | s | t | e | m |
| 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 |

Figure 12.1: The string "Economic, ecological ecosystem" and its indices

Now we try some searches. Check the “edge cases” of the start and end of the string:

```
print(my_search(string, "Eco"))
```

(0, 3)

```
print(my_search(string, "stem"))
```

(26, 4)

Test when none of the characters of the substring are in the string:

```
print(my_search(string, "ABC"))
```

None

Test when we have a partial match that is ultimately not in the string:

```
print(my_search(string, "ecom"))
```

None

Test searching for our specific substring:

```
print(my_search(string, "ecos"))
```

(21, 4)