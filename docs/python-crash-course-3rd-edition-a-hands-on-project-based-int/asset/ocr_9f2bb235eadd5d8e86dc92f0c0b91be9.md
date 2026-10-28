Python returns all the elements in the tuple, just as it would for a list:

```
```
200
50
```
```

# # Writing Over a Tuple

Although you can’t modify a tuple, you can assign a new value to a variable that represents a tuple. For example, if we wanted to change the dimensions of this rectangle, we could redefine the entire tuple:

```
```
dimensions = (200, 50)
print("Original dimensions:")
for dimension in dimensions:
    print(dimension)

dimensions = (400, 100)
print("\nModified dimensions:")
for dimension in dimensions:
    print(dimension)
```
```

The first four lines define the original tuple and print the initial dimensions. We then associate a new tuple with the variable `dimensions`, and print the new values. Python doesn’t raise any errors this time, because reassigning a variable is valid:

```
```
Original dimensions:
200
50

Modified dimensions:
400
100
```
```

When compared with lists, tuples are simple data structures. Use them when you want to store a set of values