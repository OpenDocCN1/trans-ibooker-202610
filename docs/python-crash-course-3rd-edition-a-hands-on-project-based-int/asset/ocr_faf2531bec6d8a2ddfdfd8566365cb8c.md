When you’re using loops for the first time, keep in mind that the set of steps is repeated once for each item in the list, no matter how many items are in the list. If you have a million items in your list, Python repeats these steps a million times—and usually very quickly.

Also keep in mind when writing your own `for` loops that you can choose any name you want for the temporary variable that will be associated with each value in the list. However, it’s helpful to choose a meaningful name that represents a single item from the list. For example, here’s a good way to start a `for` loop for a list of cats, a list of dogs, and a general list of items:

```
for cat in cats:
for dog in dogs:
for item in list_of_items:
```

These naming conventions can help you follow the action being done on each item within a `for` loop. Using singular and plural names can help you identify whether a section of code is working with a single element from the list or the entire list.

# Doing More Work Within a for Loop

You can do just about anything with each item in a `for` loop. Let’s build on the previous example by printing a message to each magician, telling them that they performed a great trick:

```
**magicians.py**

```python
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
```
```

The only difference in this code is where we compose a message to each magician, starting with that magician’s