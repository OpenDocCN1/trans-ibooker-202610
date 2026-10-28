
Great resource if you’re interested in working with literary texts in your programming projects.

Let’s pull in the text of *Alice in Wonderland* and try to count the number of words in the text. To do this, we’ll use the string method `split()`, which by default splits a string wherever it finds any whitespace:

```python
from pathlib import Path

path = Path('alice.txt')
try:
    contents = path.read_text(encoding='utf-8')
except FileNotFoundError:
    print(f"Sorry, the file {path} does not exist.")
else:
    # Count the approximate number of words in the file:
    words = contents.split()
    num_words = len(words)
    print(f"The file {path} has about {num_words} words.")
```

I moved the file *alice.txt* to the correct directory, so the try block will work this time. We take the string contents, which now contains the entire text of *Alice in Wonderland* as one long string, and use `split()` to produce a list of all the words in the book ❶. Using `len()` on this list ❷ gives us a good approximation of the number of words in the original text. Lastly, we print a statement that reports how many words were found in the file. This code is placed in the else block because it only works if the code in the try block was executed successfully.

The output tells us how many words are in *alice.txt*:

```
The file alice.txt has about 29594 words.
```

The count is a little high because extra information is provided by the publisher in the text file used here, but it’s a good approximation of the length of *Alice in Wonderland*.

## Working with Multiple Files

Let’s add more books to analyze, but before we do, let’s move the bulk of this program to a function called `count_words()`. This will make it easier to run the analysis for multiple books:

### word_count.py

```python
from pathlib import Path

def count_words(path):
    """Count the approximate number of words in a file."""
    try:
        contents = path.read_text(encoding='utf-8')
    except FileNotFoundError:
        print(f"Sorry, the file {path} does not exist.")
    else:
        # Count the approximate number of words in the file:
        words = contents.split()
        num_words = len(words)
        print(f"The file {path} has about {num_words} words.")

path = Path('alice.txt')
count_words(path)
```

Most of this code is unchanged. It’s only been indented, and moved into the body of `count_words()`. It’s a good habit to keep comments up to date when you’re modifying a program, so the comment has also been changed to a docstring and reworded slightly ❶.

Now we can write a short loop to count the words in any text we want to analyze. We do this by storing the names of the files we want to analyze in a list, and then we call `count_words()` for each file in the list. We’ll try to count the words for *Alice in Wonderland*, *Siddhartha*, *Moby Dick*, and *Little Women*, which are all available in the public domain. I’ve intentionally left *siddhartha.txt* out of the directory containing *word_count.py*, so we can see how well our program handles a missing file:

```python
from pathlib import Path

def count_words(filename):
    --snip--

filenames = ['alice.txt', 'siddhartha.txt', 'moby_dick.txt',
             'little_women.txt']
for filename in filenames:
    path = Path(filename)
    count_words(path)
```

The names of the files are stored as simple strings. Each string is then converted to a `Path` object ❶, before the call to `count_words()`. The missing *siddhartha.txt* file has no effect on the rest of the program’s execution:

```text
The file alice.txt has about 29594 words.
Sorry, the file siddhartha.txt does not exist.
The file moby_dick.txt has about 215864 words.
The file little_women.txt has about 189142 words.
```

Using the `try-except` block in this example provides two significant advantages. We prevent our users from seeing a traceback, and we let the program continue analyzing the texts it’s able to find. If we don’t catch the `FileNotFoundError` that *siddhartha.txt* raises, the user would see a full traceback, and the program would stop running after trying to analyze *Siddhartha*. It would never analyze *Moby Dick* or *Little Women*.

## Failing Silently

In the previous example, we informed our users that one of the files was unavailable. But you don’t need to report every exception you catch. Sometimes, you’ll want the program to fail silently when an exception occurs and continue on as if nothing happened. To make a program fail silently, you write a try block as usual, but you explicitly tell Python to do nothing in the except block. Python has a pass statement that tells it to do nothing in a block:

```python
def count_words(path):
    """Count the approximate number of words in a file."""
    try:
        --snip--
    except FileNotFoundError:
        pass
    else:
        --snip--
```

The only difference between this listing and the previous one is the pass statement in the except block. Now when a FileNotFoundError is raised, the code in the except block runs, but nothing happens. No traceback is produced, and there’s no output in response to the error that was raised. Users see the word counts for each file that exists, but they don’t see any indication that a file wasn’t found:

```
The file alice.txt has about 29594 words.
The file moby_dick.txt has about 215864 words.
The file little_women.txt has about 189142 words.
```

The pass statement also acts as a placeholder. It’s a reminder that you’re choosing to do nothing at a specific point in your program’s execution and that you might want to do something there later. For example, in this program we might decide to write any missing filenames to a file called missing_files.txt. Our users wouldn’t see this file, but we’d be able to read the file and deal with any missing texts.

## Deciding Which Errors to Report

How do you know when to report an error to your users and when to let your program fail silently? If users know which texts are supposed to be analyzed, they might appreciate a message informing them why some texts were not analyzed. If users expect to see some results but don't know which books are supposed to be analyzed, they might not need to know that some texts were unavailable. Giving users information they aren't looking for can decrease the usability of your program. Python's error-handling structures give you fine-grained control over how much to share with users when things go wrong; it's up to you to decide how much information to share.

Well-written, properly tested code is not very prone to internal errors, such as syntax or logical errors. But every time your program depends on something external such as user input, the existence of a file, or the availability of a network connection, there is a possibility of an exception being raised. A little experience will help you know where to include exception-handling blocks in your program and how much to report to users about errors that arise.

## Try It Yourself

- **10-6. Addition:** One common problem when prompting for numerical input occurs when people provide text instead of numbers. When you try to convert the input to an `int`, you’ll get a `ValueError`. Write a program that prompts for two numbers. Add them together and print the result. Catch the `ValueError` if either input value is not a number, and print a friendly error message. Test your program by entering two numbers and then by entering some text instead of a number.

- **10-7. Addition Calculator:** Wrap your code from Exercise 10-6 in a `while` loop so the user can continue entering numbers, even if they make a mistake and enter text instead of a number.

- **10-8. Cats and Dogs:** Make two files, *cats.txt* and *dogs.txt*. Store at least three names of cats in the first file and three names of dogs in the second file. Write a program that tries to read these files and print the contents of the file to the screen. Wrap your code in a `try-except` block to catch the `FileNotFoundError`, and print a friendly message if a file is missing. Move one of the files to a different location on your system, and make sure the code in the `except` block executes properly.

- **10-9. Silent Cats and Dogs:** Modify your `except` block in Exercise 10-8 to fail silently if either file is missing.

- **10-10. Common Words:** Visit Project Gutenberg ([https://gutenberg.org](https://gutenberg.org)) and find a few texts you’d like to analyze. Download the text files for these works, or copy the raw text from your browser into a text file on your computer.

  You can use the `count()` method to find out how many times a word or phrase appears in a string. For example, the following code counts the number of times `'row'` appears in a string:

  ```python
  >>> line = "Row, row, row your boat"
  >>> line.count('row')
  2
  >>> line.lower().count('row')
  3
  ```

  Notice that converting the string to lowercase using `lower()` catches all appearances of the word you’re looking for, regardless of how it’s formatted.

  Write a program that reads the files you found at Project Gutenberg and determines how many times the word `'the'` appears in each text. This will be an approximation because it will also count words such as `'then'` and `'there'`. Try counting `'the '`, with a space in the string, and see how much lower your count is.
