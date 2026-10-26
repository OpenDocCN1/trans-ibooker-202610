A dictionary key can be associated with a list of values:

```
states_visited = {
    'Eric': ['AK', 'WY', 'WA', 'NH'],
    'Isaac': ['CO', 'NY', 'AK'],
    --snip--
}
```

When you loop through the items, each value will be a list. To do this, nest a for loop for the list inside a for loop for the dictionary:

```
for name, states in states_visited.items():
    print(f"{name} has visited:")
    for state in states:
        print(f"  {state}")
```

```
Eric has visited:
AK
WY
WA
NH

Isaac has visited:
CO
NY
AK
```