```
① path = Path('numbers.json')
② contents = path.read_text()
③ numbers = json.loads(contents)

print(numbers)
```

We make sure to read from the same file we wrote to ①. Since the data file is just a text file with specific formatting, we can read it with the read_text() method ②. We then pass the contents of the file to json.loads() ③. This function takes in a JSON-formatted string and returns a Python object (in this case, a list), which we assign to numbers. Finally, we print the recovered list of numbers and see that it’s the same list created in number_writer.py:

```
[2, 3, 5, 7, 11, 13]
```

This is a simple way to share data between two programs.

# Saving and Reading User-Generated Data

Saving data with json is useful when you’re working with user-generated data, because if you don’t store your user’s information somehow, you’ll lose it when the program stops running. Let’s look at an example where we prompt the user for their name the first time they run a program and then remember their name when they run the program again.

Let’s start by storing the user’s name:

# remember_me.py

```
from pathlib import Path
import json

① username = input("What is your name? ")

② path = Path('username.json')
contents = json.dumps(username)
```