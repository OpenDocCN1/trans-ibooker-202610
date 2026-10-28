Sometimes you’ll want to prevent a function from modifying a list. For example, say that you start with a list of unprinted designs and write a function to move them to a list of completed models, as in the previous example. You may decide that even though you’ve printed all the designs, you want to keep the original list of unprinted designs for your records. But because you moved all the design names out of <u>unprinted_designs</u>, the list is now empty, and the empty list is the only version you have; the original is gone. In this case, you can address this issue by passing the function a copy of the list, not the original. Any changes the function makes to the list will affect only the copy, leaving the original list intact.

You can send a copy of a list to a function like this:

```
```python
function_name(list_name[:])
```
```

The slice notation `[:]` makes a copy of the list to send to the function. If we didn’t want to empty the list of unprinted designs in *printing_models.py*, we could call `print_models()` like this:

```
```python
print_models(unprinted_designs[:], completed_models)
```
```

The function `print_models()` can do its work because it still receives the names of all unprinted designs. But this time it uses a copy of the original unprinted designs list, not the actual <u>unprinted_designs</u> list. The list <u>completed_models</u> will fill up with the names of printed models like it did before, but the original list of unprinted designs will be unaffected by the function.

Even though you can preserve the contents of a list by passing a copy of it to your functions, you should pass the original list to functions unless you have a specific reason to pass a copy. It’s more efficient for a function to work with an