The `json.dumps()` function takes one argument: a piece of data that should be converted to the JSON format. The function returns a string, which we can then write to a data file:

# number_writer.py

```
```python
from pathlib import Path
import json

numbers = [2, 3, 5, 7, 11, 13]

path = Path('numbers.json')
contents = json.dumps(numbers)
path.write_text(contents)
```
```

We first import the `json` module, and then create a list of numbers to work with. Then we choose a filename in which to store the list of numbers ❶. It’s customary to use the file extension `.json` to indicate that the data in the file is stored in the JSON format. Next, we use the `json.dumps()` ❷ function to generate a string containing the JSON representation of the data we’re working with. Once we have this string, we write it to the file using the same `write_text()` method we used earlier.

This program has no output, but let’s open the file `numbers.json` and look at it. The data is stored in a format that looks just like Python:

```
```text
[2, 3, 5, 7, 11, 13]
```
```

Now we’ll write a separate program that uses `json.loads()` to read the list back into memory:

# number_reader.py

```
```python
from pathlib import Path
import json
```
```