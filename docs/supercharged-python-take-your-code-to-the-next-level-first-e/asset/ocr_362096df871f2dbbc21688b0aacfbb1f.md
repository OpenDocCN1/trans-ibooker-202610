# # DICT_OBJ.VALUES()

Returns a sequence containing all the associated values in the dictionary. To treat this sequence as a list, you can use a list conversion. For example:

Click here to view code image

```
```
grades = {'Moe':1.5, 'Larry':1.0, 'Curly':1.0,
          'BillG': 4.0}
print(grades.values())
```
```

These statements print the following:

```
```
dict_values([1.5, 1.0, 1.0, 4.0])
```
```

# # DICT_OBJ.UPDATE(SEQUENCE)

This method extends the dictionary object by adding all the key-value entries in sequence to the dict_obj. The sequence argument is either another dictionary or a sequence of tuples containing key-value pairs.

For example, the following statements start with two entries in grades1 and then adds three more entries to that dictionary.

Click here to view code image

```
```
grades1 = {'Moe':1.0, 'Curly':1.0}
grades2 = {'BillG': 4.0}
grades3 = [('BrianO', 3.9), ('SillySue', 2.0)]
grades1.update(grades2)
grades1.update(grades3)
print(grades1)
```
```