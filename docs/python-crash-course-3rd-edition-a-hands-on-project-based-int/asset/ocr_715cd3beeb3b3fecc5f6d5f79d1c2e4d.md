individual lines in a file, you don’t need to strip any whitespace when reading the file. The `splitlines()` method returns a list of all lines in the file, and we assign this list to the variable `lines`. We then loop over these lines and print each one:

```
3.1415926535
  8979323846
  2643383279
```

Since we haven’t modified any of the lines, the output matches the original text file exactly.

# Working with a File’s Contents

After you’ve read the contents of a file into memory, you can do whatever you want with that data, so let’s briefly explore the digits of *pi*. First, we’ll attempt to build a single string containing all the digits in the file with no whitespace in it:

# pi_string.py

```
from pathlib import Path

path = Path('pi_digits.txt')
contents = path.read_text()

lines = contents.splitlines()
pi_string = ''
for line in lines:
    pi_string += line

print(pi_string)
print(len(pi_string))
```

We start by reading the file and storing each line of digits in a list, just as we did in the previous example. We then create a variable, `pi_string`, to hold the digits of *pi*. We write