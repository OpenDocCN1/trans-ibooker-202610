Loop through a dictionary to access just the keys. This is the default behavior when looping through a dictionary (example continues from Card 4.1):

```
for state in capitals:
    print(f"State: {state}")
```

```
State: AK
State: AL
State: AZ
```

You can also access just the values in a dictionary:

```
for capital in capitals.values():
    print(f"Capital: {capital}")
```

```
Capital: Juneau
Capital: Montgomery
Capital: Phoenix
```

To work with both keys and values, use the items() method:

```
for state, capital in capitals.items():
    print(f"{capital}, {state}")
```

```
Juneau, AK
Montgomery, AL
Phoenix, AZ
```