libraries that are involved in opening and reading from files. You don’t usually need to read through or understand all of these lines in a traceback.

To handle the error that’s being raised, the try block will begin with the line that was identified as problematic in the traceback. In our example, this is the line that contains read_text():

```
from pathlib import Path

path = Path('alice.txt')
try:
    contents = path.read_text(encoding='utf-8')
except FileNotFoundError:
    print(f"Sorry, the file {path} does not exist.")
```

In this example, the code in the try block produces a FileNotFoundError, so we write an except block that matches that error ①. Python then runs the code in that block when the file can’t be found, and the result is a friendly error message instead of a traceback:

```
Sorry, the file alice.txt does not exist.
```

The program has nothing more to do if the file doesn’t exist, so this is all the output we see. Let’s build on this example and see how exception handling can help when you’re working with more than one file.

# Analyzing Text

You can analyze text files containing entire books. Many classic works of literature are available as simple text files because they are in the public domain. The texts used in this section come from Project Gutenberg (https://gutenberg.org). Project Gutenberg maintains a collection of literary works that are available in the public domain, and it’s a