# # SET_OBJ.UNION_UPDATE(OTHER_R_SET)

Performs the same action as the **union** method, except that the results are placed in _set_obj_ and the value returned is **None**.

The union-assignment operator (`|=`) performs the same action.

Click here to view code image

```
a_set |= b_set    # Put the union in a_set
```

This operator provides an easy way to extend the contents of a set. For example:

Click here to view code image

```
a_set = {1, 2, 3}
a_set |= {200, 300}
print(a_set)    # Prints {1, 2, 3, 200, 300}
```