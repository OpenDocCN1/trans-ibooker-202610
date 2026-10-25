
pickles data but treats an entire file as one big dictionary. The location of any desired object, according to its key, is looked up, and the object is found quickly through random access.

## 8.3 THE FILE/DIRECTORY SYSTEM

The Python download comes with an **os** (operating system) package that enables you to inspect the file/directory system as well as control processes. You can get a complete summary by importing the package and getting help for it.

```python
import os
help(os)
```

The number of functions supported by the **os** package is large and too numerous to fully list or describe here. However, the following list provides an overview.

- Functions that start, end, or repeat processes: These include **spawn**, **kill**, **abort**, and **fork**. The **fork** function spawns a new process based on an existing one.
- Functions that make changes to, or navigate through, the file/directory system: These include **rename**, **removedirs**, **chroot**, **getcwd** (get current working directory), and **rmdir** (remove directory). Also included are **listdir**, **makedir**, and **mkdir**.
- Functions that modify file flags and other attributes: These include **chflags**, **chmod**, and **chown**.
- Functions that get or alter environment variables: These include `getenv`, `getenvb`, and `putenv`.
- Functions that execute new system commands: These include functions that start with the name `exec`.
- Functions that provide low-level access to file I/O: Python read/write functions are built on top of these. These include `open`, `read`, and `write`.

The `os` and `os.path` packages can effectively check for the existence of a file before you try to open it, as well as giving you the ability to delete files from the disk. You might want to use that one with care.

The following IDLE session checks the working directory, switches to the Documents subdirectory, and checks the current working directory again. Then it checks for the existence of a file named `pythag.py`, confirming that it exists. The session finally removes this file and confirms that the file has been removed.

![](img/6e45221a8147f2442c653a3d4a866570_472_0.png)

```python
>>> import os
>>> os.getcwd()
'/Users/brianoverland'
>>> os.chdir('Documents')
>>> os.path.isfile('pythag.py')
True
>>> os.remove('pythag.py')
>>> os.path.isfile('pythag.py')
False
```

Checking for the existence of a file by calling the `os.path.isfile` function is often a good idea. Another useful function is `os.listdir`, which returns a list of all the names of files in the current directory (by default) or of a specified directory.

```python
os.listdir()
```

## 8.4 HANDLING FILE-OPENING EXCEPTIONS

Whenever you open a file, a number of runtime errors (exceptions) can arise. Your programs will always be more professional and easy to use if you handle exceptions gracefully rather than letting the program “bomb out.”

One of the most common exceptions is raised by the attempt to open a nonexistent file for reading. That can easily happen because the user might mistype a character. The result is that the FileNotFoundError exception gets raised.

![](img/6e45221a8147f2442c653a3d4a866570_473_0.png)

```python
try:
    statement_block_1
except exception_class:
    statement_block_2
```

If, during execution of statement_block_1, an exception is raised, that exception causes the program to terminate abruptly unless the **except** clause catches the exception by specifying a matching _exception_class_. If you want the program to look for more than one type of exception, you can do so by using multiple **except** clauses.

```python
try:
    statement_block_1
except exception_class_A:
    statement_block_A
[ except exception_class_B:
    statement_block_B ]...
```

In this case, the brackets are not intended literally but indicate optional items. The ellipses (. .) indicate that there may be any number of such optional clauses. There are also two more optional clauses: **else** and **finally**. You can use either one or both.

![](img/6e45221a8147f2442c653a3d4a866570_474_0.png)

```python
try:
    statement_block_1
except exception_class_A:
    statement_block_A
[ except exception_class_B:
    statement_block_B ]...
[ else:
    statement_block_2 ]
[ finally:
    statement_block_3 ]
```

The optional **else** clause is executed if the first statement block completes execution with no exceptions. The **finally** clause, if present, is executed after all the other blocks are, unconditionally.

Here’s how you might use these features to open a file for reading, in text mode:

Click here to view code image

```python
try:
    fname = input('Enter file to read:')
    f = open(fname, 'r')
    print(f.read())
except FileNotFoundError:
    print('File', fname, 'not found. Terminating.')
```

The use of **except** in this case handles the exception raised if the file can’t be found. By handling this exception, you can terminate nicely or perform other actions. However, it doesn’t automatically reprompt the user for the right name, which is usually what you’d want.

Therefore, you may want to set up a loop that does not terminate until (1) the user enters a file name that is successfully found, or (2) the user indicates he or she wants to quit by entering an empty string.

So, for more flexibility, you can combine **try/except** syntax with a **while** loop. The loop has **break** conditions and so is not truly infinite. It prompts the user until she either enters a valid file name or else she quits by entering an empty string.

Click here to view code image

```python
while True:
    try:
        fname = input('Enter file name: ')
        if not fname:     # Quit on empty string.
            break
        f = open(fname)   # Attempt file open here.
        print(f.read())
        f.close()
        break
    except FileNotFoundError:
        print('File could not be found. Re-enter.')
```

Here’s a version of this code that uses the **else** clause. This version calls the **close** function if and only if there were no exceptions raised. The behavior of the code at run time should be the same, but this version uses the keywords more selectively.

> Click here to view code image

```python
while True:
    fname = input('Enter file name: ')
    if not fname:
        break
    try:
        f = open(fname)   # Attempt file open here.
    except FileNotFoundError:
        print('File could not be found. Re-enter.')
    else:
        print(f.read())
        f.close()
        break
```

## 8.5 USING THE “WITH” KEYWORD

The most obvious way to do file operations is to open a file, perform file I/O, and close the file. But what if an exception is raised in the middle of a file I/O read? The program abruptly ends without tying up loose ends and politely closing down resources.

A nice shortcut is to employ a **with** statement. The action is to open a file and permit access through a variable. If, during execution of the block, an exception is raised, the file is automatically closed, so that no file handles remain open.

![](img/6e45221a8147f2442c653a3d4a866570_477_0.png)

Click here to view code image

```python
with open(filename, mode_str) as file_obj:
    statements
```

In this syntax, the *filename* and *mode_str* arguments have the same meaning they do in the **open** statement as described in the next section. The *file_obj* is a variable name that you supply; this variable gets assigned the file object returned by the **open** statement. The statements are then executed, until (of course) an exception is raised.

Here’s an example that reads a text file using the **with** keyword:

Click here to view code image

```python
with open('stuff.txt', 'r') as f:
    lst = f.readlines()
    for thing in lst:
        print(thing, end='')
```

## 8.6 SUMMARY OF READ/WRITE OPERATIONS

Table 8.1 summarizes the basic syntax for reading and writing to text, binary, and pickled files.

# Table 8.1. Syntax for Reading and Writing to Files

| Function or method | Description |
|---|---|
| `file = open(name, mode)` | Opens a file so that it can be written to or read. Common modes include text modes “w” and “r”, as well as binary modes “wb” and “rb”. Text-file mode (whether read or write) is the default. Also note that adding a plus sign (+) to the “r” or “w” mode specifies read/write mode. |
| `str = file.read()` | Text-file read operation. Reads the next line of text by reading up to the newline and returning the string that was read. The trailing newline is read as part of the string returned; therefore at least a newline is returned, except for one situation: If and |
