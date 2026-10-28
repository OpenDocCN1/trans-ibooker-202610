```
the_list[:]
['a', 'b', 'c', 'd', 'e', 'f']
```

+   - the_list[the_index:] returns a new list containing the items in the_list from position the_index to the end. If the_index is positive and ≥ len(the_list), we get an empty list.

```
the_list[2:]
['c', 'd', 'e', 'f']
```

```
the_list[10:]
[]
```

+   - the_list[:the_index] returns a new list from the items in the original list up to but not including the item at the_index.

```
the_list[:2]
['a', 'b']
```

+   - the_list[the_first_index:the_second_index] returns a new list from the items in the original list starting with the item at position the_first_index up to but not including the item at the_second_index.

```
the_list[1:3]
['b', 'c']
```

+   - the_list[::2] returns a new list from the items in the original list starting with the first, and then including every second item.

```
the_list[::2]
['a', 'c', 'e']
```

+   - the_list[1::3] returns a new list from the items in the original list starting with the second, and then including every third item.

```
the_list[1::3]
['b', 'e']
```

+   - the_list[::-1] returns a copy of the_list in reverse order.

```
the_list[::-1]
['f', 'e', 'd', 'c', 'b', 'a']
```

I think the technique of reversing the list is marvelous.