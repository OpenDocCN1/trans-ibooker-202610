These statements maintain word/frequency pairs in the dictionary. Each time a new word is found, it’s added to the dictionary with a value of 1; otherwise, the existing value is incremented by 1. These statements produce the following output:

> Click here to view code image

```
{'I': 3, 'am': 3, 'what': 1, 'and': 1, 'that': 2,
'is': 1, 'all': 1, 'am.': 1}
```

# # DICT_OBJ.ITEMS()

Returns a sequence of tuples, in the format (_key_, _value_), containing all the key-value pairs in the dictionary. For example:

> Click here to view code image

```
grades = {'Moe':1.5, 'Larry':1.0, 'BillG':4.0}
print(grades.items())
```

This code prints the following:

> Click here to view code image

```
dict_items([('Moe', 1.5), ('Larry', 1.0), ('BillG',
4.0)])
```

# # DICT_OBJ.KEYS()

Returns a sequence containing all the keys in the dictionary. For example: