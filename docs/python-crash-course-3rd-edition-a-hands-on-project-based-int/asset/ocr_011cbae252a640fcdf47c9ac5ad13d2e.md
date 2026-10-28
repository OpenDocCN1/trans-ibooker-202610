checks if 'erin' is in this sequence. Because she’s not, a message is printed inviting her to take the poll:

```
Erin, please take our poll!
```

# Looping Through a Dictionary’s Keys in a Particular Order

Looping through a dictionary returns the items in the same order they were inserted. Sometimes, though, you’ll want to loop through a dictionary in a different order.

One way to do this is to sort the keys as they’re returned in the for loop. You can use the sorted() function to get a copy of the keys in order:

```
favorite_languages = {
    'jen': 'python',
    'sarah': 'c',
    'edward': 'rust',
    'phil': 'python',
}

for name in sorted(favorite_languages.keys()):
    print(f"{name.title()}, thank you for taking the poll.")
```

This for statement is like other for statements, except that we’ve wrapped the sorted() function around the dictionary.keys() method. This tells Python to get all the keys in the dictionary and sort them before starting the loop. The output shows everyone who took the poll, with the names displayed in order:

```
Edward, thank you for taking the poll.
Jen, thank you for taking the poll.
Phil, thank you for taking the poll.
Sarah, thank you for taking the poll.
```

# Looping Through All Values in a Dictionary